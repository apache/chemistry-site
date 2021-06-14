Title: Compare client binding with client API

# Compare client binding with client API
<a name="OpenCMISAPIExamples-OpenCMISAPIExamples"></a>

Here are two code snippets performing the same operations. The first
snippet uses the client API; the second snippet uses the provider API. As
you can see the provider API is clunkier and more difficult to use but it
gives you access to all CMIS extension points and provides more
fine-grained control.

<a name="OpenCMISAPIExamples-ClientAPI"></a>
## Client API

[Client API JavaDoc](http://hudson.zones.apache.org/hudson/job/Chemistry%20-%20OpenCMIS%20-%20site/javadoc/org/apache/opencmis/client/api/package-summary.html).
See also [OpenCMIS Client API](client/dev-client-api.html).

    Map<String, String> parameters = new HashMap<String, String>();
    parameters.put(SessionParameter.BINDING_TYPE, BindingType.ATOMPUB.value());
    parameters.put(SessionParameter.ATOMPUB_URL, "http://localhost:8080/opencmis/atom");
    parameters.put(SessionParameter.REPOSITORY_ID, "A1");
    parameters.put(SessionParameter.USER, "test");
    parameters.put(SessionParameter.PASSWORD, "test");

    // create the session
    Session session = SessionFactoryImpl.newInstance().createSession(parameters);

    // get repository info
    RepositoryInfo repInfo = session.getRepositoryInfo();
    System.out.println("Repository name: " + repInfo.getName());

    // get root folder and its path
    Folder rootFolder = session.getRootFolder();
    String path = rootFolder.getPath();
    System.out.println("Root folder path: " + path);

    // list root folder children
    ItemIterable<CmisObject> children = rootFolder.getChildren();
    for (CmisObject object : children) {
        System.out.println("---------------------------------");
        System.out.println("	Id:              " + object.getId());
        System.out.println("	Name:            " + object.getName());
        System.out.println("	Base Type:       " + object.getBaseTypeId());
        System.out.println("	Property 'bla':  " + object.getPropertyValue("bla"));
    
        ObjectType type = object.getType();
        System.out.println("	Type Id:          " + type.getId());
        System.out.println("	Type Name:        " + type.getDisplayName());
        System.out.println("	Type Query Name:  " + type.getQueryName());
    
        AllowableActions actions = object.getAllowableActions();
        System.out.println("	canGetProperties: " + actions.getAllowableActions().contains(Action.CAN_GET_PROPERTIES));
        System.out.println("	canDeleteObject:  " + actions.getAllowableActions().contains(Action.CAN_DELETE_OBJECT));
    }
    
    // get an object
    ObjectId objectId = session.createObjectId("100");
    CmisObject object = session.getObject(objectId);
    
    if (object instanceof Folder) {
        Folder folder = (Folder) object;
        System.out.println("Is root folder: " + folder.isRootFolder());
    }
    
    if (object instanceof Document) {
        Document document = (Document) object;
        ContentStream content = document.getContentStream();
        System.out.println("Document MIME type: " + content.getMimeType());
    }

<a name="OpenCMISAPIExamples-ClientBindingAPI"></a>
## Client Binding API

[Client Binding API JavaDoc](http://hudson.zones.apache.org/hudson/job/Chemistry%20-%20OpenCMIS%20-%20site/javadoc/org/apache/opencmis/commons/provider/package-summary.html). See also [OpenCMIS Client Binding API](client/dev-client-bindings.html).

     Map<String, String> parameters = new HashMap<String, String>();
     parameters.put(SessionParameter.ATOMPUB_URL, "http://localhost:8080/opencmis/atom");
     parameters.put(SessionParameter.USER, "test");
     parameters.put(SessionParameter.PASSWORD, "test");

     // create binding
     CmisBinding binding = CmisBindingFactory.newInstance().createCmisAtomPubBinding(parameters);

     String repositoryId = "A1";

     // get repository info
     RepositoryInfo repInfo = binding.getRepositoryService().getRepositoryInfo(repositoryId, null);
     System.out.println("Repository name: " + repInfo.getName());

     // get root folder and its path
     ObjectData rootFolder = binding.getObjectService().getObject(repositoryId,
         repInfo.getRootFolderId(), "*", true, IncludeRelationships.NONE, null, false, false, null);

     PropertyString pathProperty = (PropertyString)
     rootFolder.getProperties().getProperties().get(PropertyIds.PATH);
     String path = pathProperty.getFirstValue();
     System.out.println("Root folder path: " + path);

     // list root folder children
     ObjectInFolderList childrenList = binding.getNavigationService().getChildren(repositoryId, repInfo.getRootFolderId(), "*", null, true,
         IncludeRelationships.NONE, null, false, BigInteger.valueOf(10000), BigInteger.ZERO, null);

     for (ObjectInFolderData object : childrenList.getObjects()) {
         System.out.println("---------------------------------");

         PropertyString nameProperty = (PropertyString) object.getObject().getProperties().getProperties().get(PropertyIds.NAME);
         PropertyString blaProperty = (PropertyString) object.getObject().getProperties().getProperties().get("bla");
         PropertyId typeProperty = (PropertyId) object.getObject().getProperties().getProperties().get(PropertyIds.OBJECT_TYPE_ID);

         System.out.println("  Id:	           " + object.getObject().getId());
         System.out.println("  Name:             " + nameProperty.getFirstValue());
         System.out.println("  Base Type:        " + object.getObject().getBaseTypeId());
         System.out.println("  Property 'bla':   " + (blaProperty == null ? null : blaProperty.getFirstValue()));

         TypeDefinition type = binding.getRepositoryService().getTypeDefinition(repositoryId, typeProperty.getFirstValue(), null);
         System.out.println("  Type Id:          " + type.getId());
         System.out.println("  Type Name:        " + type.getDisplayName());
         System.out.println("  Type Query Name:  " + type.getQueryName());

         AllowableActions actions = object.getObject().getAllowableActions();
         System.out.println("  canGetProperties: " + actions.getAllowableActions().contains(Action.CAN_GET_PROPERTIES));
         System.out.println("  canDeleteObject:  " + actions.getAllowableActions().contains(Action.CAN_DELETE_OBJECT));
     }

     // get an object
     String objectId = "100";

     ObjectData object = binding.getObjectService().getObject(repositoryId, objectId, "*", false,
         IncludeRelationships.NONE, null, false, false, null);

      if (object.getBaseTypeId() == BaseTypeId.CMIS_FOLDER) {
          System.out.println("Is root folder: " + (repInfo.getRootFolderId().equals(object.getId())));
      }

      if (object.getBaseTypeId() == BaseTypeId.CMIS_DOCUMENT) {
          ContentStream content = binding.getObjectService().getContentStream(repositoryId, objectId, null, null, null, null);
          System.out.println("Document MIME type: " + content.getMimeType());
     }
