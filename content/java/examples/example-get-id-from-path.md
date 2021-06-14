Title: Getting the id from path

# Getting the ID of an object from its path

A code snippet how to get the id of an object when only a path is known:

```java
    String path = "/User Homes/customer1/document.odt"
    CmisObject object = session.getObjectByPath(path);
    String id = object.getId();
```
