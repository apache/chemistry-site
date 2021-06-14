Title: Creating and updating CMIS objects

# Creating and updating CMIS objects
 

## Creating a folder

This example creates a folder under the root folder.

```java
    Folder root = session.getRootFolder();

    // properties
    // (minimal set: name and object type id)
    Map<String, Object> properties = new HashMap<String, Object>();
    properties.put(PropertyIds.OBJECT_TYPE_ID, "cmis:folder");
    properties.put(PropertyIds.NAME, "a new folder");

    // create the folder
    Folder newFolder = root.createFolder(properties);
```

## Creating a document

This example creates a document in the folder `parent`.

```java
    Folder parent = ....

    String name = "myNewDocument.txt";

    // properties 
    // (minimal set: name and object type id)
    Map<String, Object> properties = new HashMap<String, Object>();
    properties.put(PropertyIds.OBJECT_TYPE_ID, "cmis:document");
    properties.put(PropertyIds.NAME, name);
    
    // content
    byte[] content = "Hello World!".getBytes();
    InputStream stream = new ByteArrayInputStream(content);
    ContentStream contentStream = new ContentStreamImpl(name, BigInteger.valueOf(content.length), "text/plain", stream);
    
    // create a major version
    Document newDoc = parent.createDocument(properties, contentStream, VersioningState.MAJOR);
```

## Updating properties

This example updates five properties of a CMIS object

```java
    CmisObject cmisobject = ....

    Map<String, Object> updateProperties = new HashMap<String, Object>();

    updateProperties.put("my:property", "new value"); // single-value property
    updateProperties.put("my:int.property", 42);
    updateProperties.put("my:date.property", new GregorianCalendar());
    updateProperties.put("my:bool.property", true);

    List<String> shoppingList = new ArrayList<String>();
    shoppingList.add("milk");
    shoppingList.add("bread");
    shoppingList.add("cheese");
    updateProperties.put("my:shopping.list", shoppingList); // multi-value property

    cmisobject.updateProperties(updateProperties);
```
