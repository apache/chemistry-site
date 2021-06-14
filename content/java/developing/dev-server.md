Title: OpenCMIS Server Framework

# OpenCMIS Server Framework

The OpenCMIS Server Framework provides a server implementation of both CMIS
bindings, AtomPub and Web Services, and maps them to Java interfaces.
Requests and data from CMIS clients are converted and pushed to a
repository connector. The connector then translates the CMIS calls into native
repository calls.


<a name="OpenCMISServerFramework-RepositoryConnectorDevelopment"></a>
## Repository Connector Development

This is a brief description of the interfaces and classes a repository
connector has to extend and implement. For interface details see the
OpenCMIS Server Framework SPI JavaDoc.


<a name="OpenCMISServerFramework-FrameworkEntryPoint"></a>
### Framework Entry Point

A repository connector has to extend the `AbstractServiceFactory` class.
This class manages the objects that implement the CMIS service interface.
There is only one active instance of this factory class per servlet
context. The class name has to be set in the configuration file
`/WEB-INF/classes/repository.properties`.


    # set fully qualified class name
    class=org.repository.ServicesFactory


The configuration file may contain more key-value pairs. They are passed to
the `init` method of the `AbstractServiceFactory` object when the
servlet context starts up.

For each request the `getService` method is called by the framework and a
`CallContext` object is passed. This `CallContext` object contains
data about the request, such as the used binding, the repository id,
username and password. The `getService` method must return an object that
implements the `CmisService` interface.
This object is used by framework only for this one request and only in this thread. 
Therefore, the `CmisService` object need not to be thread-safe and and can hold request specific data.
When the framework is ready to send a response and does not need the `CmisService` object anymore, 
it calls the `close` method. The repository connector can override this method to do repository specific clean ups
(e.g. close a repository session, commit or rollback database transactions, remove temporary files, update caches, etc.).

It is up to the repository connector how these service objects are created
and maintained. It is possible to create such an object for each request or
keep an instance per thread in a `ThreadLocal` or manage service objects in a
pool. If you reuse a service object make sure that it doesn't hold any
state from previous requests.


<a name="OpenCMISServerFramework-ServiceInterface"></a>
### Service Interface

The `CmisService` interface contains all operations of the CMIS
specification and a few more. Most methods are named after the operations
described in the CMIS specification. There are a few exceptions to that
rule because the AtomPub binding doesn't always allow a one-to-one mapping.
Those divergences are explained in the JavaDoc.

The methods take the same parameters as described in the CMIS
specification. There are also a few exceptions that are explained in the
JavaDoc.

It is recommended to extend the `AbstractCmisService` class instead of
implementing the `CmisService` interface directly.
`AbstractCmisService` contains several convenience methods and covers all
AtomPub specifics in a generic way.


<a name="OpenCMISServerFramework-AtomPubSpecifics"></a>
### AtomPub Specifics

The AtomPub binding needs more object data than many of the operations
return. Therefore a repository connector has to provide `ObjectInfo`
objects through the `getObjectInfo` method. `AbstractCmisService`
provides a generic implementation of `getObjectInfo`. If you don't notice
any performance issue with the AtomPub binding, you don't have to bother
with `ObjectInfo` objects.

If the generic assembly of `ObjectInfo` objects raises a problem, a
repository connector can build them itself. `AbstractCmisService`
provides a `addObjectInfo` method that takes and manages `ObjectInfo`
objects. Which objects are required for which operation is documented in
the JavaDoc.


<a name="OpenCMISServerFramework-AuthenticationFramework"></a>
### Authentication Framework

Authentication information is transported to the service implementation via
the `CallContext` object. The `CallContext` is basically a Map and can
contain any kind of data. The OpenCMIS server fills it by default with a
username and a password from either HTTP basic authentication for the
AtomPub binding or WS-Security (UsernameToken) for the Web Services
binding.

Other authentication methods can be plugged in if needed. Here is how this
works for the two CMIS bindings.

<a name="OpenCMISServerFramework-AtomPubauthentication"></a>
#### AtomPub authentication

For the AtomPub binding a new class implementing the interface
`org.apache.chemistry.opencmis.server.impl.atompub.CallContextHandler`
has to be created. It gets the `HttpServletRequest` object of the current
request and returns key-value pairs that are added to the `CallContext`.
See the JavaDoc for details.

The new `CallContext` handler can be activated by changing the servlet init parameter `callContextHandler`
in `/WEB-INF/web.xml`.

```xml
    <init-param>
      <param-name>callContextHandler</param-name>
      <param-value>org.example.opencmis.MyCallContextHandler</param-value>
    </init-param>
```


<a name="OpenCMISServerFramework-WebServicesauthentication"></a>
#### Web Services authentication

For the Web Services binding a new `SOAPHandler` class has to be created
and registered in `/WEB-INF/sun-jaxws.xml`.

The `handleMessage` method should look like this:

```java
    public boolean handleMessage(SOAPMessageContext context) {
      Boolean outboundProperty = (Boolean)
            context.get(MessageContext.MESSAGE_OUTBOUND_PROPERTY);
      if (outboundProperty.booleanValue()) {
        // we are only looking at inbound messages
        return true;
      }
    
      // do whatever you have to do here
      String user = ...
      String secret = ...
  
      // set up key-value pairs for the CallContext
      Map<String, String> callContextMap = new HashMap<String, String>();
      callContextMap.put("org.example.opencmis.user", user);
      callContextMap.put("org.example.opencmis.secret", secret);
    
      // add key-value pairs the SOAP message context
      context.put(AbstractService.CALL_CONTEXT_MAP, callContextMap);
      context.setScope(AbstractService.CALL_CONTEXT_MAP, Scope.APPLICATION);
    
      return true;
    }
```

<a name="OpenCMISServerFramework-RepositoryConnectorDeployment"></a>
## Repository Connector Deployment

The OpenCMIS build process creates a WAR file in
`/chemistry-opencmis-server/chemistry-opencmis-server/target`. This WAR
file should be used as a template. It can be deployed as it is but doesn't
do anything.

In order to use your connector, copy your compiled connector code into this
WAR file and overwrite `/WEB-INF/classes/repository.properties`.

Have a look at the [OpenCMIS FileShare Repository](repositories/dev-repositories-fileshare.html)
 test repository code and `pom.xml`. It's a simple example of a
repository connector.
