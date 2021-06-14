Title: Connecting to a repository

# Connecting to a repository

In order to connect to a CMIS repository you have to [create a session](../examples/example-create-session.html).

A few repositories need [special treatment](../developing/dev-repository-specific-notes.html).
    
    
## Using Cookies
    
Some repositories are sending HTTP cookies to maintain state (although CMIS is stateless) or to accelerate authentication for subsequent calls. OpenCMIS
ignores these cookies by default. 

### OpenCMIS 0.4.0 and earlier

The following code snippet activates cookies for your application and OpenCMIS.
See [this page](http://java.sun.com/docs/books/tutorial/networking/cookies/cookiemanager.html) for details. Note, that all OpenCMIS sessions share the same set of cookies.

```java
    CookieManager cm = new CookieManager(null, CookiePolicy.ACCEPT_ALL);
    CookieHandler.setDefault(cm);
```

If each session has to manage a separate set of cookies, a [custom authentication provider](../developing/client/dev-client-bindings.html#OpenCMISClientBindings-CustomAuthenticationProvider) is required. 

### OpenCMIS 0.5.0 and later

OpenCMIS has per-session cookie support. It can be activated by setting the [session parameter COOKIES](../developing/dev-session-parameters.html) to "true".
