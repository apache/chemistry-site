Title: OpenCMIS Binding differences

# Differences between the CMIS bindings

OpenCMIS supports both the AtomPub and the web services binding interface.
It hides all the details how to handle the protocol but there are some
subtle differences that need to be reflected in the Java interfaces. This
chapter explains where the differences are and why they are needed.

<a name="HowToBuildAServer-TheObjectInfointerface"></a>
## The ObjectInfo interface

The methods in the interfaces `CmisService` are following the data model
in the CMIS specification. The specification defines the following
services:

* AclService
* DiscoveryService
* ObjectService
* MulitfilingService
* NavigationService
* PolicyService
* RelationshipService
* RepositoryService
* Versioning Service

For each of these services exists a corresponding interface in opencmis.
CmisService is an interface that unifies all interfaces in one interface.
The methods in this interface correspond to the methods in the
specification. For the web service binding this is a one-to-one mapping.
For the AtomPub binding this information is not sufficient in all cases.
Take the creation of a document as an example. The web service returns in
this case a single string value containing the id of the created document.
A response in the AtomPub binding however consists of an AtomPub entry
element which is an XML fragment (for an example look at
`DocumentEntry.xml` in the examples directory of the CMIS specification).
You will notice that generating this XML requires much more information
than the create...() methods in the specification provide as return value.
Your implementation needs to provide all the information so that opencmis
can generate the complete response. For this purpose the `ObjectInfo` was
introduced. `getObjectInfo` is the method of the `CmisService` interface
that provides this information. The `AbstractCmisService` contains a
member `objectInfoMap` that you can fill with this information. If you
ignore this the class `AbstractCmisService` contains a default
implementation which works but is very ineffecient. You should use this
only as a starting point. Beware that a service can be called from multiple
threads, so have to take care to handle threading issues properly.

If the method you are implementing returns a list of objects and not only a
single value (for example methods like `getChildren()`, `getDescendants()`
in the navigation service) you need to provide an `ObjectInfo` for each
element in the collection. `getObjectInfo` therefore returns a map with
the object id for each object as key and the corresponding `ObjectInfo`
as value. You can use the method `addObjectInfo` to add an element to the
map. 



<a name="HowToBuildAServer-Thecreate()methods"></a>
## The create() methods

The web service binding has separate calls for each object type to be
created: policies, relationships, documents and folders:

* createDocument
* createFolder
* createRelationship
* createPolicy

In the AtomPub binding there is one general `create()` method that is
used for all object types. The CmisObjectService interface therefore
contains 5 `create()` methods, the four specific ones are used for the
web service bindings and the general one for the AtomPub binding.


