Title: Getting started with DotCMIS

## Connecting to a CMIS AtomPub endpoint
<a name="GettingstartedwithDotCMIS-ConnectingtoaCMISAtomPubendpoint"></a>

<a name="GettingstartedwithDotCMIS-Connectingtothefirstrepository"></a>
### Connecting to the first repository

```C#
    Dictionary<string, string> parameters = new Dictionary<string, string>();
    
    parameters[SessionParameter.BindingType] = BindingType.AtomPub;
    parameters[SessionParameter.AtomPubUrl] = "http://<host>/<serviceDocumentPath>";
    parameters[SessionParameter.User] = "<username>";
    parameters[SessionParameter.Password] = "<password>";
    
    SessionFactory factory = SessionFactory.NewInstance();
    ISession session = factory.GetRepositories(parameters)[0].CreateSession();
```

<a name="GettingstartedwithDotCMIS-Connectingtoaspecificrepository"></a>
### Connecting to a specific repository

```C#
    Dictionary<string, string> parameters = new Dictionary<string, string>();
    
    parameters[SessionParameter.BindingType] = BindingType.AtomPub;
    parameters[SessionParameter.AtomPubUrl] = "http://<host>/<serviceDocumentPath>";
    parameters[SessionParameter.User] = "<username>";
    parameters[SessionParameter.Password] = "<password>";
    parameters[SessionParameter.RepositoryId] = "<repositoryId>";
    
    SessionFactory factory = SessionFactory.NewInstance();
    ISession session = factory.CreateSession(parameters);
```

<a name="GettingstartedwithDotCMIS-ConnectingtoaCMISWebServicesendpoint"></a>
## Connecting to a CMIS Web Services endpoint

<a name="GettingstartedwithDotCMIS-Connectingtothefirstrepository"></a>
### Connecting to the first repository

```C#
    Dictionary<string, string> parameters = new Dictionary<string, string>();
    
    parameters[SessionParameter.BindingType] = BindingType.WebServices;
    parameters[SessionParameter.WebServicesRepositoryService] = "http://<host>/<RepositoryServiceWSDL>";
    parameters[SessionParameter.WebServicesAclService] = "http://<host>/<AclServiceWSDL>";
    parameters[SessionParameter.WebServicesDiscoveryService] = "http://<host>/<DiscoveryServiceWSDL>";
    parameters[SessionParameter.WebServicesMultifilingService] = "http://<host>/<MultifilingServiceWSDL>";
    parameters[SessionParameter.WebServicesNavigationService] = "http://<host>/<NavigationServiceWSDL>";
    parameters[SessionParameter.WebServicesObjectService] = "http://<host>/<ObjectServiceWSDL>";
    parameters[SessionParameter.WebServicesPolicyService] = "http://<host>/<PolicyServiceWSDL>";
    parameters[SessionParameter.WebServicesRelationshipService] = "http://<host>/<RelationshipServiceWSDL>";
    parameters[SessionParameter.WebServicesVersioningService] = "http://<host>/<VersioningServiceWSDL>";
    parameters[SessionParameter.User] = "<username>";
    parameters[SessionParameter.Password] = "<password>";
    
    SessionFactory factory = SessionFactory.NewInstance();
    ISession session = factory.GetRepositories(parameters)[0].CreateSession();
```

<a name="GettingstartedwithDotCMIS-Connectingtoaspecificrepository"></a>
### Connecting to a specific repository

```C#
    Dictionary<string, string> parameters = new Dictionary<string, string>();
    
    parameters[SessionParameter.BindingType] = BindingType.WebServices;
    parameters[SessionParameter.WebServicesRepositoryService] = "http://<host>/<RepositoryServiceWSDL>";
    parameters[SessionParameter.WebServicesAclService] = "http://<host>/<AclServiceWSDL>";
    parameters[SessionParameter.WebServicesDiscoveryService] = "http://<host>/<DiscoveryServiceWSDL>";
    parameters[SessionParameter.WebServicesMultifilingService] = "http://<host>/<MultifilingServiceWSDL>";
    parameters[SessionParameter.WebServicesNavigationService] = "http://<host>/<NavigationServiceWSDL>";
    parameters[SessionParameter.WebServicesObjectService] = "http://<host>/<ObjectServiceWSDL>";
    parameters[SessionParameter.WebServicesPolicyService] = "http://<host>/<PolicyServiceWSDL>";
    parameters[SessionParameter.WebServicesRelationshipService] = "http://<host>/<RelationshipServiceWSDL>";
    parameters[SessionParameter.WebServicesVersioningService] = "http://<host>/<VersioningServiceWSDL>";
    parameters[SessionParameter.User] = "<username>";
    parameters[SessionParameter.Password] = "<password>";
    parameters[SessionParameter.RepositoryId] = "<repositoryId>";
    
    SessionFactory factory = SessionFactory.NewInstance();
    ISession session = factory.CreateSession(parameters);
```

<a name="GettingstartedwithDotCMIS-Listingfolderchildren"></a>
## Listing folder children

```C#
    /// get the root folder
    IFolder rootFolder = session.GetRootFolder();
    
    // list all children
    foreach (ICmisObject cmisObject in rootFolder.GetChildren())
    {
        Console.WriteLine(cmisObject.Name);
    }
    
    // get a page
    IItemEnumerable<ICmisObject> children = rootFolder.GetChildren();
    IItemEnumerable<ICmisObject> page = children.SkipTo(20).GetPage(10); // children 20 to 30
    
    foreach (ICmisObject cmisObject in page)
    {
        Console.WriteLine(cmisObject.Name);
    }
```


<a name="GettingstartedwithDotCMIS-Fetchingadocument"></a>
## Fetching a document 

```C#
    IObjectId id = session.CreateObjectId("12345678");
    IDocument doc = session.GetObject(id) as IDocument;
    
    // properties
    Console.WriteLine(doc.Name);
    Console.WriteLine(doc.GetPropertyValue("my:property"));
    
    IProperty myProperty = doc["my:property"];
    Console.WriteLine("Id:	  " + myProperty.Id);
    Console.WriteLine("Value: " + myProperty.Value);
    Console.WriteLine("Type:  " + myProperty.PropertyType);
    
    // content
    IContentStream contentStream = doc.GetContentStream();
    Console.WriteLine("Filename:   " + contentStream.FileName);
    Console.WriteLine("MIME type:  " + contentStream.MimeType);
    Console.WriteLine("Has stream: " + (contentStream.Stream != null));
```

<a name="GettingstartedwithDotCMIS-Creatingadocument"></a>
## Creating a document

```C#
    IFolder folder = ...
    
    IDictionary<string, object> properties = new Dictionary<string, object>();
    properties[PropertyIds.Name] = "Hello World Document";
    properties[PropertyIds.ObjectTypeId] = "cmis:document";
    
    byte[] content = UTF8Encoding.UTF8.GetBytes("Hello World!");
    
    ContentStream contentStream = new ContentStream();
    contentStream.FileName = "hello-world.txt";
    contentStream.MimeType = "text/plain";
    contentStream.Length = content.Length;
    contentStream.Stream = new MemoryStream(content);
    
    IDocument doc = folder.CreateDocument(properties, contentStream, null);
```

<a name="GettingstartedwithDotCMIS-Updatingproperties"></a>
## Updating properties

```C#
    ICmisObject cmisObject = ...
    
    IDictionary<string, object> properties = new Dictionary<string, object>();
    properties["my:string"] = "a string";
    properties["my:int"] = 42;
    properties["my:date"] = DateTime.Now;
    
    IObjectId newId = cmisObject.UpdateProperties(properties);
    
    if (newId.Id == cmisObject.Id) 
    {
        // the repository updated this object - refresh the object
        cmisObject.Refresh();
    }
    else
    {
        // the repository created a new version - fetch the new version
        cmisObject = session.GetObject(newId);
    }
```

<a name="GettingstartedwithDotCMIS-Deletinganobject"></a>
## Deleting an object

```C#
    IObjectId newId = session.CreateObjectId("12345678"):
    ICmisObject cmisObject = session.GetObject(newId);
    
    cmisObject.Delete(true);
```

<a name="GettingstartedwithDotCMIS-Performingaquery"></a>
## Performing a query

```C#
    IItemEnumerable<IQueryResult> qr = session.Query("SELECT * FROM cmis:document", false);
    
    foreach (IQueryResult hit in qr)
    {
        Console.WriteLine(hit["cmis:name"].FirstValue + " (" + hit["cmis:objectId"].FirstValue + ")");
    }
```
