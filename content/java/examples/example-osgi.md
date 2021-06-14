Title:     Using OSGI with OpenCMIS

# Using OSGI with OpenCMIS

## Client Factory with OSGi

These examples show the first steps that are required in client applications: How to create a session and connect to a repository using OSGi factory service.

```java
    // OSGi factory service
    
    BundleContext bundleContext = ...;  // retriev bundle context from OSGi runtime
    ServiceReference serviceReference = bundleContext.getServiceReference(SessionFactory.class.getName());
    SessionFactory factory = (SessionFactory) bundleContext.getService(serviceReference); 
    Map<String, String> parameter = new HashMap<String, String>();
    
    // fill in session parameter
    parameter.put(...);
    
    // create session
    Session session = factory.createSession(parameter);
```

* [Creating a session](example-create-session.html)
* [OSGi support for OpenCMIS](/java/developing/dev-osgi.html)

