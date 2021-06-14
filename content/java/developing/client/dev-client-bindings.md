Title: OpenCMIS Client Bindings

# OpenCMIS Client Bindings
<a name="OpenCMISClientBindings-OpenCMISClientBindings"></a>

The OpenCMIS client bindings layer hides the CMIS AtomPub and Web Services
bindings and provides an interface that is very similar to the [CMIS domain model](http://docs.oasis-open.org/cmis/CMIS/v1.0/cs01/cmis-spec-v1.0.html#_Toc243905381)
. The services, operations, parameters, and structures are named after the
CMIS domain model and behave as described in the CMIS specification.

The primary objective of the client bindings layer is to be complete,
covering all CMIS operations and extension points. The result is a somewhat
clunky interface. The [OpenCMIS Client API](dev-client-api.html)
 sits on top of the provider layer and exposes a nicer and simpler to use
interface. It is the better choice for most applications.

A connection to a CMIS repository is represented by a [`CmisBinding`](http://chemistry.apache.org/java/0.4.0/maven/apidocs/org/apache/chemistry/opencmis/commons/spi/CmisBinding.html)
 object. Such an object can be created by the [`CmisBindingFactory`](http://chemistry.apache.org/java/0.4.0/maven/apidocs/org/apache/chemistry/opencmis/client/bindings/CmisBindingFactory.html).
The factory provides three main methods, one for each binding and third
one for a local connection (same JVM), that require binding specific
connection information. The created `CmisBinding` object exposes a
binding agnostic interface.

`CmisBinding` is the entry point to the CMIS services and a few utility
operations. It contains a transparent cache for repository infos and type
definitions. The object is serializable, although dehydrating can be
expensive. `CmisBinding` is thread-safe.

The get\*Service() methods provide access to the CMIS services. Some
service operations take provider layer specific objects. These objects
should be created with the [`BindingsObjectFactory`](http://chemistry.apache.org/java/0.4.0/maven/apidocs/org/apache/chemistry/opencmis/commons/spi/BindingsObjectFactory.html).
This factory can be obtained through the `getObjectFactory()` method of the `CmisBinding` object.

Please refer to the OpenCMIS Commons [JavaDoc](http://chemistry.apache.org/java/0.4.0/maven/apidocs/)
 and OpenCMIS Client Binding [JavaDoc](http://chemistry.apache.org/java/0.4.0/maven/apidocs/)
 for more details on the interfaces.

<a name="OpenCMISClientBindings-SampleCode"></a>
## Sample Code

<a name="OpenCMISClientBindings-CreatinganAtomPubbindinginstance"></a>
### Creating an AtomPub binding instance

The AtomPub binding requires the URL of the CMIS service document. HTTP
basic authentication is enabled by default and a username and a password
have to be provided.

```java
    Map<String, String> parameters = new HashMap<String, String>();
    
    parameters.put(SessionParameter.USER, user);
    parameters.put(SessionParameter.PASSWORD, password);
    
    parameters.put(SessionParameter.ATOMPUB_URL, url); // service document URL
    
    CmisBindingFactory factory = CmisBindingFactory.newInstance();
    CmisBinding binding = factory.createCmisAtomPubBinding(parameters);
```
 
### Creating a Web Services binding instance
    
The Web Services binding requires a WSDL URL for each CMIS service. This
might the same the URL for all services. WS-Security (UsernameToken) is
enabled by default and a username and a password have to be provided.
    
```java
    Map<String, String> parameters = new HashMap<String, String>();
    
    parameters.put(SessionParameter.USER, username);
    parameters.put(SessionParameter.PASSWORD, password);
    
    parameters.put(SessionParameter.WEBSERVICES_REPOSITORY_SERVICE, repositoryServiceWsdlUrl);
    parameters.put(SessionParameter.WEBSERVICES_NAVIGATION_SERVICE, navigationServiceWsdlUrl);
    parameters.put(SessionParameter.WEBSERVICES_OBJECT_SERVICE, objectServiceWsdlUrl);
    parameters.put(SessionParameter.WEBSERVICES_VERSIONING_SERVICE, versioningServiceWsdlUrl);
    parameters.put(SessionParameter.WEBSERVICES_DISCOVERY_SERVICE, discoveryServiceWsdlUrl);
    parameters.put(SessionParameter.WEBSERVICES_RELATIONSHIP_SERVICE, relationshipServiceWsdlUrl);
    parameters.put(SessionParameter.WEBSERVICES_MULTIFILING_SERVICE, multiFilingServiceWsdlUrl);
    parameters.put(SessionParameter.WEBSERVICES_POLICY_SERVICE, policyServiceWsdlUrl);
    parameters.put(SessionParameter.WEBSERVICES_ACL_SERVICE, aclServiceWsdlUrl);
    
    CmisBindingFactory factory = CmisBindingFactory.newInstance();
    CmisBinding binding = factory.createCmisWebServicesBinding(parameters);
```

<a name="OpenCMISClientBindings-CreatingaLocalbindinginstance"></a>
### Creating a Local binding instance

The Local binding connects to an OpenCMIS server in the same JVM. The
server factory class name has to be supplied.

```java
    Map<String, String> parameters = new HashMap<String, String>();
    
    parameters.put(SessionParameter.USER, user);
    parameters.put(SessionParameter.PASSWORD, password);
    
    parameters.put(SessionParameter.LOCAL_FACTORY, factoryClassName);
    
    CmisBindingFactory factory = CmisBindingFactory.newInstance();
    CmisBinding binding = factory.createCmisLocalBinding(parameters);
```

### Getting an Object
    
The following snippet gets the name of the object "myObject" in repository
"myRepository". The parameters of `getObject()` can be found in the CMIS
specification.
    
```java
    CmisBinding binding = ...
    
    ObjectData myObject = binding.getObjectService().getObject("myRepository", "myObject",
       "*", true, IncludeRelationships.BOTH, "cmis:none", true, true, null);
    
    PropertiesData properties = myObject.getProperties();
    PropertyData<String> nameProperty = properties.getProperties().get(PropertyIds.NAME);
    String name = nameProperty.getFirstValue();
```

<a name="OpenCMISClientBindings-CustomAuthenticationProvider"></a>
## Custom Authentication Provider

OpenCMIS supports HTTP basic authentication for the AtomPub binding and
WS-Security (UsernameToken) for the Web Services binding out of the box.
Other authentication methods can be added by implementing a custom
authentication provider.

Such a provider must extend
`org.apache.chemistry.opencmis.client.provider.spi.AbstractAuthenticationProvider`
and overwrite the methods `getHTTPHeaders` and `getSOAPHeaders`. See
JavaDoc for details.

The session parameter `SessionParameter.AUTHENTICATION_PROVIDER_CLASS`
must be set to the fully qualified class name in order to active the
authentication provider before the session is created.

```java
    Map<String, String> parameters = new HashMap<String, String>();
    
    parameters.put(SessionParameter.AUTHENTICATION_PROVIDER_CLASS, "org.example.opencmis.MyAuthenticationProvider");
    parameters.put("org.example.opencmis.user", "cmisuser"); // MyAuthenticationProvider can get and evaluate this
    parameters.put("org.example.opencmis.secret", "b3BlbmNtaXMgdXNlcg==");
    
    parameters.put(SessionParameter.ATOMPUB_URL, url); // service document URL
    
    CmisBindingFactory factory = CmisBindingFactory.newInstance();
    CmisBinding provider = factory.createCmisAtomPubBinding(parameters);
```
