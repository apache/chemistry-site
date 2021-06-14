Title: OpenCMIS Client API

# OpenCMIS Client API
<a name="OpenCMISClientAPI-OpenCMISClientAPI"></a>

## Overview
The OpenCMIS client layer provides an object oriented interface for easy
consumption of the underlying CMIS related layers. In addition to the CMIS
specification the OpenCMIS client layer introduces a session concept which
easily enables applications to get control on the client side cache
behavior.

The client layer consists of a client interface, common interfaces and a
runtime implementation. The runtime maps the client interface to the
bindings layer and implements the session cache. All parts are exposed by
following packages:

Package                                      | Artifact                       | Description
---------------------------------------------|--------------------------------|----------------------------------------------------------------------------------------------
org.apache.chemistry.opencmis.client.api     | chemistry-opencmis-client-api  | Main interfaces of the client API
org.apache.chemistry.opencmis.commons.api    | chemistry-opencmis-commons-api | Interfaces and classes shared by client and client bindings API
org.apache.chemistry.opencmis.client.runtime | chemistry-opencmis-client-impl | Implementation classes of client API including a default implementation of the SessionFactory


The following UML diagram illustrates the main interfaces of the client API:

![OpenCMIS client class diagram][1]

* **SessionFactory** This interface provides the entry point into the client
API and is responsible to create a session object. Additionally it gives
access to all repository info exposed by a CMIS client binding. The runtime
provides a default implementation for the SessionFactory interface.
* **Session** This is the main interface an application has to work with. A
session object is related to a CMIS service client binding and is attached
to exact one repository. All data that is received through the session
interface can be cached in the session object in dependency of the concrete
implementation which is behind.
* **Repository** Wrapper interface for the CMIS RepositoryInfo service.
* **CmisObject** The CmisObject interface represents the CMIS domain object.
* **ObjectType** This interface is base for all CMIS domain types like
FolderType, DocumentType, PolicyType and RelationshipType. The derived
interfaces are not shown in the diagram.
* **Folder** This interface represents the CMIS folder object.
* **Document** This interface represents the CMIS document object.
* **ContentStream** this interface wraps the content stream of a CMIS
document.
* **Policy** This interface represents the CMIS policy object.
* **Relationship** This interface represents the CMIS relationship object.

<a name="OpenCMISClientAPI-Sessions"></a>
## Sessions

OpenCMIS' central entry point to a CMIS repository is a session. A session
controls settings and caches that used across multiple calls and provides
access to all CMIS operations and objects.
In order to create a session, the SessionFactory needs a set parameters
(see [OpenCMIS Session Parameters](../dev-session-parameters.html)
).


  [1]: opencmis-client-api-class-diagram2.png