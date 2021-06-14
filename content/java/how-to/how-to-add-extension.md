Title: Adding CMIS extensions

# Adding CMIS extensions (Server)

The CMIS standard offers to add implementation specific extensions in many places. Here is an
example how to add extensions to an instance of ObjectData.
   
_(since OpenCMIS 0.2.0)_ <br/>

```java
    // we want to attach an extension to an object
    ObjectData object = ...
    
    // some dummy data
    String typeId = "MyType";
    String objectId = "1111-2222-3333";
    String name = "MyDocument";
    
    // find a namespace for the extensions that is different from the CMIS namespaces
    String ns = "http://apache.org/opencmis/example";
    
    // create a list for the first level of our extension
    List<CmisExtensionElement> extElements = new ArrayList<CmisExtensionElement>();
            
    // set up an attribute (Avoid attributes! They will not work with the JSON binding!)
    Map<String, String> attr = new HashMap<String, String>();
    attr.put("type", typeId);
            
    // add two leafs to the extension
    extElements.add(new CmisExtensionElementImpl(ns, "objectId", attr, objectId));
    extElements.add(new CmisExtensionElementImpl(ns, "name", null, name));
    
    // set the extension list
    List<CmisExtensionElement> extensions = new ArrayList<CmisExtensionElement>();
    extensions.add(new CmisExtensionElementImpl(ns, "exampleExtension", null, extElements));
    object.setExtensions(extensions);
```

This should create something like that:

```xml
    <exampleExtension:exampleExtension xmlns="http://apache.org/opencmis/example" xmlns:exampleExtension="http://apache.org/opencmis/example">
        <objectId type="MyType">1111-2222-3333</objectId>
        <name>MyDocument</name>
    </exampleExtension:exampleExtension>
```
