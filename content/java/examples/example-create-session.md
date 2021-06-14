Title: Creating a session

# Creating a session
These examples show the first steps that are required in client applications: 
How to create a session and connect to a repository.

A complete list of all session parameters can be found [here](../developing/dev-session-parameters.html).

## AtomPub binding

```java
    // default factory implementation
    SessionFactory factory = SessionFactoryImpl.newInstance();
    Map<String, String> parameter = new HashMap<String, String>();
    
    // user credentials
    parameter.put(SessionParameter.USER, "Otto");
    parameter.put(SessionParameter.PASSWORD, "****");
    
    // connection settings
    parameter.put(SessionParameter.ATOMPUB_URL, "http://<host>:<port>/cmis/atom");
    parameter.put(SessionParameter.BINDING_TYPE, BindingType.ATOMPUB.value());
    parameter.put(SessionParameter.REPOSITORY_ID, "myRepository");
    
    // create session
    Session session = factory.createSession(parameter);
```

## Web Services binding

```java
    // default factory implementation
    SessionFactory factory = SessionFactoryImpl.newInstance();
    Map<String, String> parameter = new HashMap<String, String>();
    
    // user credentials
    parameter.put(SessionParameter.USER, "Otto");
    parameter.put(SessionParameter.PASSWORD, "****");
    
    // connection settings
    parameter.put(SessionParameter.BINDING_TYPE, BindingType.WEBSERVICES.value());
    parameter.put(SessionParameter.WEBSERVICES_ACL_SERVICE, "http://<host>:<port>/cmis/services/ACLService?wsdl");
    parameter.put(SessionParameter.WEBSERVICES_DISCOVERY_SERVICE, "http://<host>:<port>/cmis/services/DiscoveryService?wsdl");
    parameter.put(SessionParameter.WEBSERVICES_MULTIFILING_SERVICE, "http://<host>:<port>/cmis/services/MultiFilingService?wsdl");
    parameter.put(SessionParameter.WEBSERVICES_NAVIGATION_SERVICE, "http://<host>:<port>/cmis/services/NavigationService?wsdl");
    parameter.put(SessionParameter.WEBSERVICES_OBJECT_SERVICE, "http://<host>:<port>/cmis/services/ObjectService?wsdl");
    parameter.put(SessionParameter.WEBSERVICES_POLICY_SERVICE, "http://<host>:<port>/cmis/services/PolicyService?wsdl");
    parameter.put(SessionParameter.WEBSERVICES_RELATIONSHIP_SERVICE, "http://<host>:<port>/cmis/services/RelationshipService?wsdl");
    parameter.put(SessionParameter.WEBSERVICES_REPOSITORY_SERVICE, "http://<host>:<port>/cmis/services/RepositoryService?wsdl");
    parameter.put(SessionParameter.WEBSERVICES_VERSIONING_SERVICE, "http://<host>:<port>/cmis/services/VersioningService?wsdl");
    parameter.put(SessionParameter.REPOSITORY_ID, "myRepository");

    // create session
    Session session = factory.createSession(parameter);
```

## Browser binding

```java
    // default factory implementation
    SessionFactory factory = SessionFactoryImpl.newInstance();
    Map<String, String> parameter = new HashMap<String, String>();
    
    // user credentials
    parameter.put(SessionParameter.USER, "Otto");
    parameter.put(SessionParameter.PASSWORD, "****");
    
    // connection settings
    parameter.put(SessionParameter.BROWSER_URL, "http://<host>:<port>/cmis/browser");
    parameter.put(SessionParameter.BINDING_TYPE, BindingType.BROWSER.value());
    parameter.put(SessionParameter.REPOSITORY_ID, "myRepository");
    
    // create session
    Session session = factory.createSession(parameter);
```

## Local binding

The local binding is specific to OpenCMIS. It lets an OpenCMIS client connect to an OpenCMIS server in the same JVM.

```java
    // default factory implementation
    SessionFactory factory = SessionFactoryImpl.newInstance();
    Map<String, String> parameter = new HashMap<String, String>();
    
    // user credentials
    parameter.put(SessionParameter.USER, "Otto");
    parameter.put(SessionParameter.PASSWORD, "****");

    // connection settings
    parameter.put(SessionParameter.BINDING_TYPE, BindingType.LOCAL.value());
    parameter.put(SessionParameter.LOCAL_FACTORY, "my.local.factory");
    parameter.put(SessionParameter.REPOSITORY_ID, "myRepository");

    // create session
    Session session = factory.createSession(parameter);
```

## Connect to the first repository

Some CMIS endpoints only provide one repository. In this case it is not necessary to provide its repository id.  
The following code snippet gets the list of all available repositories and connects to the first one.

```java
    List<Repository> repositories = factory.getRepositories(parameter);
    Session session = repositories.get(0).createSession();
```
