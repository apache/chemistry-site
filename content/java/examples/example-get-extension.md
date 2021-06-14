Title: Getting CMIS extensions

# Getting CMIS extensions

_(since OpenCMIS 0.2.0)_ <br/>
The CMIS specification allows to add extensions at several points in CMIS data structures (object, properties, allowable actions, ACLs, policies,
etc.).
These extensions are XML fragments for the AtomPub and the Web Services binding. (It will be something simpler for the upcoming JSON binding.)
Think of it as a tree structure with named node and leafs. Only the leafs can have a value.

```java
    // get an object from somewhere
    CmisObject object = ...

    // extensions can be attached to different levels
    // in this example we get the extensions on the properties level
    List<CmisExtensionElement> extensions = object.getExtensions(ExtensionLevel.PROPERTIES);
    
    if(extensions == null) {
       // this object has no extensions on this level
       return;
    }

    // iterate through the extensions until we find the one we are looking for
    for(CmisExtensionElement ext: extensions) {
       if("myExtension".equals(ext.getName())) {
         // found it, now print the values of the children	   
         for(CmisExtensionElement child: ext.getChildren()) {
	    System.out.println(child.getName() + ": " + child.getValue());
         }
       }
    }
```
