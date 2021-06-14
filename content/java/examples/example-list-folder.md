Title: List a folder with paging

# Showing a folder's items, with results paging

```java
    int maxItemsPerPage = 5;
    int skipCount = 10;
    
    CmisObject object = session.getObject(session.createObjectId(folderId));
    Folder folder = (Folder) object;
    OperationContext operationContext = session.createOperationContext();
    operationContext.setMaxItemsPerPage(maxItemsPerPage);
    
    ItemIterable<CmisObject> children = folder.getChildren(operationContext);
    ItemIterable<CmisObject> page = children.skipTo(skipCount).getPage();
    
    Iterator<CmisObject> pageItems = page.iterator();
    while(pageItems.hasNext()) {
        CmisObject item = pageItems.next();
        // Do something with the item.
    }
```
