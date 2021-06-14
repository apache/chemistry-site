Title: Reading the Root Folder

# Reading the Root Folder

## Listing all children

This example lists all objects in the root folder.

```java
    Folder root = session.getRootFolder();
    
    ItemIterable<CmisObject> children = root.getChildren();
    
    for (CmisObject o : children) {
      System.out.println(o.getName());
    }
```

## Retrieving a page

This example retrieves a page of 10 objects starting at the 20th child.

```java
    Folder root = session.getRootFolder();
    
    ItemIterable<CmisObject> page = root.getChildren().skipTo(20).getPage(10);
    
    for (CmisObject o : page) {
      System.out.println(o.getName());
    }
```
