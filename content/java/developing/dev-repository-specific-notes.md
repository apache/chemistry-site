Title: Repository specific notes

#  Repository specific notes


## Alfresco 3.4

In order to access Alfresco aspects, you have to set up the [Alfresco OpenCMIS Extension](http://apache-extras.org/p/alfresco-opencmis-extension).


## SharePoint 2010
    
While connecting via AtomPub is straight forward, connecting via Web Services is a bit tricky.
(See also Microsofts [CMIS documentation](http://msdn.microsoft.com/en-us/library/ff934619.aspx) for details.)
    
### AtomPub
    
The service document URL is `http://<host>/_vti_bin/cmis/rest/<SPList>?getrepositoryinfo`.


### Web Services
    
1. Download the WSDL with a web browser and store it on your local disk. The WSDL URL is `http://<host>/_vti_bin/cmissoapwsdl.aspx?wsdl`.
1. Provide `file://`... URLs to the downloaded WSDL for all OpenCMIS WSDL session parameters.


### Authentication

If NTLM is enabled on SharePoint, you have to activate the OpenCMIS NTLM authentication provider.

```java
    parameters.put(SessionParameter.AUTHENTICATION_PROVIDER_CLASS, CmisBindingFactory.NTLM_AUTHENTICATION_PROVIDER);
```

(The NTLM authentication provider uses [java.net.Authenticator](http://download-llnw.oracle.com/javase/6/docs/api/java/net/Authenticator.html)
under the hood. If this interferes with your environment, you are on your own. Sorry!)

