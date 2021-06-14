Title: Perform a query 

# Simple query pattern

This example demonstrates a simple query that selects all properties from all documents and then prints them.  
(This query shouldn't be used in a real application. Always select only the properties and objects you need!)

```java
    ItemIterable<QueryResult> results = session.query("SELECT * FROM cmis:document", false);

    for(QueryResult hit: results) {  
        for(PropertyData<?> property: hit.getProperties()) {
        
            String queryName = property.getQueryName();
            Object value = property.getFirstValue();
    
            System.out.println(queryName + ": " + value);
        }
        System.out.println("--------------------------------------");
    }
```

# Get document objects from a query

```java
    String myType = "my:documentType";
 
    // get the query name of cmis:objectId
    ObjectType type = session.getTypeDefinition(myType);
    PropertyDefinition<?> objectIdPropDef = type.getPropertyDefinitions().get(PropertyIds.OBJECT_ID);
    String objectIdQueryName = objectIdPropDef.getQueryName();
    
    String queryString = "SELECT " + objectIdQueryName + " FROM " + type.getQueryName();

    // execute query
    ItemIterable<QueryResult> results = session.query(queryString, false);
    
    for (QueryResult qResult : results) {
       String objectId = qResult.getPropertyValueByQueryName(objectIdQueryName);
       Document doc = (Document) session.getObject(session.createObjectId(objectId));
    }
```
