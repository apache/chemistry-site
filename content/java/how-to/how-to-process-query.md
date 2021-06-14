Title: Query Integration

[TOC]

# OpenCMIS Query Integration

The CMIS standard contains a powerful query language that supports full
text and relational metadata query capabilities and is modeled along a
subset of SQL. Many repositories will have the demand to integrate into
this query interface. OpenCMIS provides support to make a query integration
easier. This article explains the various hooks that are provided to
integrate into the query interface. These hooks provide different levels of
comfort and flexibility. OpenCMIS integrates a query parser that uses ANTLR
as parsing engine. However there is no strong dependency on ANTLR. If you
prefer a different language parsing tool it is possible to do this.

There are four different levels how you can integrate query:

1. Implement query in the discovery service
1. Use the built-in ANTLR and ANTLR CMISQL grammar
1. Use OpenCMIS CMISQL grammar and integrate into ANTLR query walker
1. Use predefined query walker and integrate into interface `PredicateWalker`.

## Implement query in the discovery service

The first way is to implement the `query()` method like any other service
method on your own. This gives you the maximum flexibility including using
a parser tool of your choice and extensions of the query grammar as you
like. This is also the method with the highest implementation effort.

## Use built-in ANTLR and ANTLR CMISQL grammar

OpenCMIS comes with a build-in integration of ANTLR and provides a grammar
file for CMISQL. You can reuse this grammar file, modify or extend it and
integrate query by using the ANTLR mechanisms for parsing and walking the
abstract syntax tree. Please refer to the ANTLR documentation for further
information. This is the right level to use if you need custom parser tree
transformations or would like to extend the grammar with your own
constructs. For demonstration purposes OpenCMIS provides an extended
grammar as an example.

## Use OpenCMIS CMSIQL grammar and integrate into ANTLR query walker

If the standard CMISQL grammar is sufficient for you there is another level
of integration. For many repositories there are common tasks for processing
queries: The columns of the select part need to be evaluated and mapped to
type and property definitions. The from area needs to be mapped to type
definitions and some parts of the where part again refer to properties in
types. In addition all aliases defined in the statement need to be resolved
and many validations are performed. OpenCMIS provides a class that performs
these common tasks. You can make use of the resolved types, properties and
aliases and walk the resulting abstract syntax tree (AST) to evaluate the
query. You are free to walk the AST as many times as you need and in the
order you prefer. The basic idea is that the SELECT and FROM parts are
processed by OpenCMIS and you are responsible for the WHERE part. The
InMemory server provides an example for this level of integration: For
each object contained in the repository the tree is traversed and it's checked
if it matches the current query. You can take the InMemory code as an
example if you decide to use this integration level.

## Use predefined query walker

For some repositories a simple and one-pass query traversal is sufficient.
This can be the case if for example your query needs to be translated to a
SQL query statement. Because ANTLR has some complexity OpenCMIS provides a
predefined walker that performs a simple one pass depth-first traversal. If
this is sufficient this interface hides most of the complexity of ANTLR.
All you have to do is to implement a Java interface
(`PredicateWalker`). You can refer to the InMemory server for example
code (`InMemoryWhereClauseWalker`). 

`AbstractPredicateWalker` implements interface `PredicateWalker` and 
implements common functionality useful for traversing the tree. For example
parsing literals like `"abc"`, `-123` to Java objects like `String` 
and `Integer` is handled there.

If the interface of the predefined walker `PredicateWalker` does not
fit your needs you can define your own interface. The code generated
by ANTLR does not make any assumptions how you design the walking of
your tree. The only dependency is contained in the interface 
`PredicateWalkerBase` consisting of a single method. If you start 
defining your own walker you have to implement or extend `PredicateWalkerBase`.
The unit tests contain an example for this. See class `QueryConditionProcessor`
in the unit tests for the InMemory server.

Note: There is currently no predefined walker for JOIN statements. If
you need to support JOINs you have to build your own walker for this part
as outlined in the previous section.

## Using QueryObject

The class `QueryObject` provides all the basic functionality for resolving
types and properties and performs common validation tasks. The `QueryObject`
processes the `SELECT` and `FROM` parts as well as all property references from
the `WHERE` part. It maintains a list of Java objects and an interface that you
can use to access the property and type definitions given your current
position in the statement. For an example refer to the class
`StoreManagerImpl` of the InMemory Server and method `query()`.
To be able to use this object `QueryObj` needs to get access to the types contained in your
repository. For this purpose you need to pass an interface to a `TypeManager`
as input parameter. Your code will typically look like this:

```java
	public class MyWalker extends AbstractPredicateWalker {
                             // extends AbstractPredicateWalker
                             // or implements interface PredicateWalker
							 // or implements interface PredicateWalkerBase
	  // . . .
	}

    TypeManager tm = new MyTypeManager(); // implements interface TypeManager
    MyWalker myWalker = new MyWalker();    
    queryObj = new QueryObject(tm);
    QueryUtil queryUtil = new QueryUtil();

    CmisQueryWalker queryProcessor = queryUtil.traverseStatementAndCatchExc(statement, queryObj, myWalker);
```

`queryUtil` then will process the statement and call the interface methods of
your walker (Note: This code is in opencmis, you don't have to implement it
yourself.):

```java
    try {
        walker = getWalker(statement);
        walker.query(queryObj, pw);
        return walker; 
	} catch (RecognitionException e) {
		String errorMsg = queryObj.getErrorMessage();
		throw new CmisInvalidArgumentException("Walking of statement failed with RecognitionException error: \n   " + errorMsg);
	} catch (CmisBaseException e) {
		throw e;
	} catch (Exception e) {
		throw new CmisInvalidArgumentException("Walking of statement failed with exception: \n   " + e);
    }
```

After this method returns you may for example ask your walker object
`myWalker` for the generated SQL string.

## Processing a node and referencing types and properties

While traversing the tree you often will need to access the property and
type definitions that are referenced in the where clause. The `QueryObject`
provides the necessary information for resolving the references. For
example the statement

    `... WHERE x < 123`

will result in calling the method `walkLessThan()` in your walker callback
implementation:

```java
    public Boolean walkLessThan(Tree ltNode, Tree leftNode, Tree rightNode) {
    
        Object rVal = walkLiteral(rightChild);
        ColumnReference colRef;
    
        CmisSelector sel = queryObj.getColumnReference(columnNode
			     .getTokenStartIndex());
    
        if (null == sel)
           throw new CmisInvalidArgumentException("Unknown property query name " +
		          columnNode.getChild(0));
        else if (sel instanceof ColumnReference)
           colRef = (ColumnReference) sel;
    
       TypeDefinition td = colRef.getTypeDefinition();
       PropertyDefinition pd =
           td.getPropertyDefinitions().get(colRef.getPropertyId());
        
       // process the statement, for example append it to a WHERE
       // in your generated SQL statement.
    }
```

The right child node is a literal and you will get an Integer object with
value 123. The left node is a reference to a property and
`getColumnReference()` will either give you a function (currently the only
supported function is `SCORE()`) or a reference to a property in a type of
your type system. The query object maintains several maps to resolve
references. The key to the map is always the token index in the incoming
token stream (an integer value). You can get the token index for each node
by calling `getTokenStartIndex()` on the node.

## Building the result list

After processing the query an `ObjectList` has to be returned containing the
requested properties and function results. You can ask the query object for
the requested information:

```java
    Map props = queryObj.getRequestedProperties();
    Map funcs = queryObj.getRequestedFuncs();
```

Key of the map is the query name and value is the alias if an alias was
used in the statement or the query name otherwise.

## Limitations

Currently the query parser does not include the full text search part
of the grammar. Support for JOIN is limited. This will be enhanced in a
future version
