Title: OpenCMIS Modules
Breadcrumb: opencmis:modules

# OpenCMIS Modules

<a name="OpenCMISModules-OpenCMISModules"></a>

OpenCMIS is divided into four groups of modules:

* **chemistry-opencmis-commons**: Modules used by all other modules.
* **chemistry-opencmis-client**: CMIS client related modules.
* **chemistry-opencmis-server**: CMIS server framework related modules.
* **chemistry-opencmis-test**: Test modules that are not required at runtime.

<img src="opencmis-layers.png"/>


<a name="OpenCMISModules-ModuleDescription"></a>
## Module Description

<a name="OpenCMISModules-Commons"></a>
### Commons

<a name="OpenCMISModules-chemistry-opencmis-commons-api"></a>
#### chemistry-opencmis-commons-api
Interfaces, enums and exceptions used across all other modules.

<a name="OpenCMISModules-chemistry-opencmis-commons-impl"></a>
#### chemistry-opencmis-commons-impl
Implementations of the interface defined in chemistry-opencmis-commons-api.
It also generates and contains the JAXB classes.

<a name="OpenCMISModules-Client"></a>
### Client

<a name="OpenCMISModules-chemistry-opencmis-client-api"></a>
#### chemistry-opencmis-client-api
Client API used by applications. See [OpenCMIS Client API](client/dev-client-api.html)
 for details.

<a name="OpenCMISModules-chemistry-opencmis-client-impl"></a>
#### chemistry-opencmis-client-impl
Implementations of the client API.

<a name="OpenCMISModules-chemistry-opencmis-client-bindings"></a>
#### chemistry-opencmis-client-bindings
CMIS client AtomPub and Web Services binding implementation. See [OpenCMIS Client Bindings](client/dev-client-bindings.html)
 for details.

<a name="OpenCMISModules-Server"></a>
### Server

<a name="OpenCMISModules-chemistry-opencmis-server-bindings"></a>
#### chemistry-opencmis-server-bindings
CMIS server AtomPub and Web Services binding implementation. See [OpenCMIS Server Framework](dev-server.html)
 for details.

<a name="OpenCMISModules-chemistry-opencmis-server-support"></a>
#### chemistry-opencmis-server-support
Convenience classes for repository connectors. This module contains the
CMIS query parser.

<a name="OpenCMISModules-chemistry-opencmis-server-inmemory"></a>
#### chemistry-opencmis-server-inmemory
CMIS in-memory repository for test purposes. See [OpenCMIS InMemory Repository](repositories/dev-repositories-inmemory.html)
 for details.

<a name="OpenCMISModules-chemistry-opencmis-server-fileshare"></a>
#### chemistry-opencmis-server-fileshare
CMIS file system repository for test purposes. See [OpenCMIS FileShare Repository](repositories/dev-repositories-fileshare.html)
 for details.

<a name="OpenCMISModules-chemistry-opencmis-server-jcr"></a>
#### chemistry-opencmis-server-jcr
CMIS to JCR bridge. See [OpenCMIS JCR Repository](repositories/dev-repositories-jcr.html)
 for details.

<a name="OpenCMISModules-Test"></a>
### Test

<a name="OpenCMISModules-chemistry-opencmis-test-fit"></a>
#### chemistry-opencmis-test-fit
Integration tests covering the whole stack.

<a name="OpenCMISModules-chemistry-opencmis-test-browser"></a>
#### chemistry-opencmis-test-browser
Simple web based CMIS client. See [OpenCMIS Browser](tools/dev-tools-browser.html)
 for details.

<a name="OpenCMISModules-chemistry-opencmis-test-util"></a>
#### chemistry-opencmis-test-util
Utility classes for tests.

<a name="OpenCMISModules-chemistry-opencmis-test-tools"></a>
#### chemistry-opencmis-test-tools
Development tools.
 