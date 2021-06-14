Title: OpenCMIS InMemory Repository

# OpenCMIS InMemory Repository

**This repository is *obviously* not intended for production use!**

The OpenCMIS In-Memory Repository is an implementation of a CMIS repository
that holds content and metadata in memory. Therefore, all data stored in
the repository is lost after each restart. This implementation has two mainpurposes:
  
1. Provide a fast backend for testing purposes without any additional dependencies or configuration.	
1. Provide a sample implementation for the server interface of OpenCMIS.


## Status

The code is intended to provide an example application. It is not a server framework or 
designed for extensibility. Expect any interface to change at any time.
The following features are implemented:
  
* Type System and Repository Service   
* Navigation    
* Object Service
* Query (no joins, text search only on plain text documents)
* Versioning
* ACLs
* Unfiling
* Renditions (fixed icons for folders and Office documents, thumbnails for JPEG inages)   

Not supported are currently:
   
* Relationships   
* Policies	
* Change Tokens (a hard coded value in getContentChanges is returned)

## Build and Deploy the Repository

1. Follow this guide: [Build OpenCMIS](../../how-to/how-to-build.html)
1. A ready-to-use WAR file should now exist in `/chemistry-opencmis-server/chemistry-opencmis-server-inmemory/target`.
1. Deploy the WAR file to your favorite servlet engine.
1. AtomPub endpoint: `http://<host>:<port>/<context>/atom`,<br/>
   Web Services endpoint: `http://<host>:<port>/<context>/services/RepositoryService`

## Download the InMemory Repository

You can download the latest release from the [download page](/java/download.html).

## Configure the Repository

The CMIS specification does not currently provide support for creating a
repository or administrative capabilities such as the creation of type
definitions.

The in-memory repository therefore provides settings that allows the server
to bootstrap itself, so that on successful startup, the server is in a
usable state. These settings include the repository id, its type
definitions and an initial folder/document data set.

The types to be made available must be provided in a Java class as code. By
default, the class
`org.apache.chemistry.opencmis.inmemory.types.DefaultTypeSystemCreator.java`
is provided. You can take this as an example if you want to provide your
own custom types. Such a class must implement the
`org.apache.chemistry.opencmis.inmemory.TypeCreator` interface which
consists of a single method that returns a list of `TypeDefinition`
objects. This method will be called once during startup.
  
Some clients just support read-only access. The in-memory repository
supports such clients by providing the possibility to fill a repository on
start-up with data (folders and documents). The folder and document types
to be used can be configured and also the number of documents and folders
that are created. Those configuration parameters start with the prefix
`RepositoryFiller`.

Bootstrap settings are set in the repository.properties file
(`/WEB-INF/classes/repository.properties`).

```properties
    # In Memory Settings
      # The class that enables the in-memory repository as server implementation
    class=org.apache.chemistry.opencmis.inmemory.server.InMemoryServiceFactoryImpl
    
    # A repository that is created on start-up
    InMemoryServer.RepositoryId=A1
    
    # The class that used to initialize the type system (creates all types that are available)
    InMemoryServer.TypesCreatorClass=org.apache.chemistry.opencmis.inmemory.types.DefaultTypeSystemCreator
    
    # settings to initialize a repository with data on start-up
      # enable or disable
    RepositoryFiller.Enable=true
      # Type id of documents that are created
    RepositoryFiller.DocumentTypeId=ComplexType
      # Type id of folders that are created
    RepositoryFiller.FolderTypeId=cmis:folder
      # Number of documents created per folder
    RepositoryFiller.DocsPerFolder=3
      # Number of folders created per folder
    RepositoryFiller.FolderPerFolder=2
      # number of folder levels created (depth of hierarchy)
    RepositoryFiller.Depth=3
      # Size of content for documents (0=do not create content), default=0
    RepositoryFiller.ContentSizeInKB=32
      # properties to set for a document
    RepositoryFiller.DocumentProperty.0=StringProp
    #RepositoryFiller.DocumentProperty.1=StringPropMV
      # properties to set for a folder
    #RepositoryFiller.FolderProperty.0=StringFolderProp
```
