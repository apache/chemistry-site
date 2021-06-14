Title: Reading metadata and content

# Reading metadata and content

## Reading metadata

### Reading Properties - Single Property

```java
    ObjectId id = session.createObjectId("4711");
    Document document = (Document) session.getObject(id);
    Property<String> p = document.getProperty(PropertyIds.OBJECT_ID);
    
    String s = p.getValue();
```

### Reading Properties - All Properties

```java
    ObjectId id = session.createObjectId("4711");
    Document document = (Document) session.getObject(id);
    List<Property<?>> l = document.getProperties();
    Iterator<Property<?>> i = l.iterator();
    while (i.hasNext()) {
      Property<?> p = i.next();
      Object value = p.getValue();
      PropertyType t = p.getType();
    
      switch (t) {
        case INTEGER:
          Integer n = (Integer) value;
          System.out.println(p.getName() + " = " + n);
          break;
        case STRING:
     [...]
    }
```

## Retrieving content

```java
    CmisObject object = session.getObject(session.createObjectId(docId));
    Document document = (Document) object;
    String filename = document.getName();
    InputStream stream = document.getContentStream().getStream();
```
