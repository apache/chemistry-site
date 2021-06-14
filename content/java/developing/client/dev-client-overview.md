Title: CMIS Client Overview

# OpenCMIS Client Development

The OpenCMIS client API provides an easy way to connect to CMIS repositories. This is a short step-by-step guide.

## Prerequisite

1. Read the second chapter of the [CMIS specification](http://docs.oasis-open.org/cmis/CMIS/v1.0/cmis-spec-v1.0.html). You can skip the other chapters but you need to understand the CMIS domain model to in order to use OpenCMIS.

1. Set up your repository. Some repositories provide CMIS out-of-the-box. Other repositories require an update, a module, a service pack, or something like that.

1. Select your binding. The CMIS specification defines two bindings, AtomPub and Web Services. Check which binding your repository supports. If the repository supports both, go for the AtomPub binding. In most cases it is the better choice. But don't worry; you can switch the binding at any time. The OpenCMIS API is not binding specific.

1. Check which authentication method the repository provides. Refer to the authentication section for more information. 

1. Make sure your repository speaks HTTPS. You probably don't want to expose your password and your documents to everyone on the network.

1. Download the [OpenCMIS client library](../../download.html) and the [CMIS Workbench](../../download.html).

1. Choose an API. OpenCMIS provides two CMIS client APIs that are called [Client API](dev-client-api.html) and [Client Bindings API](dev-client-bindings.html). The Client API is a high-level, object orientated API and suitable for most use cases. It sits on top of the Client Bindings API. The Client Bindings API reflects the CMIS domain model. It allows fine-grained control, which makes the interfaces a bit clunky. This guide explains the Client API and [this page](../dev-compare-client-api-binding.html) compares both.


## The Theory

CMIS is stateless. That has a few implications.
It scales well since each call can go to a different cluster node. But it also implies that each call has to contain authentication information. It also makes the client responsible for the information it pulls from the repository. And this has a direct effect on the application performance. Keep in mind that CMIS sends and receives XML over HTTP. Avoiding such a call can save valuable time.

OpenCMIS introduces a session concept on top of CMIS and provides a set of caches that help reducing the number of calls and the amount of transmitted data. 

The central object of the client API is the Session object. It manages the authentication, all caches and provides the entry point to all CMIS operations. A Session object is bound to a user and therefore there is a set of separate caches per user. The repository might send different data for different users, for example to obey permissions or provide localized display names and property values. Therefore it is not possible to manage a shared cache. (The Client Bindings API is more flexible in this respect but also requires more effort and care.)

In order to be effective, this Session object has to be reused as much as possible! Don't throw it away. Keep it and reuse it! OpenCMIS is thread-safe. The Session object can and should be reused across thread boundaries.


## First Steps

The OpenCMIS client package contains all jars you need to connect to a repository. Make sure they are all in your classpath.

Before you start implementing have a look at the OpenCMIS JavaDoc. Start with the Session interface, the Document interface, and the Folder interface. Together with your CMIS domain model knowledge (you remember the second chapter of the CMIS specification) you should now have a picture of what is available. 

To connect to a repository you have to create a Session object. See [this page](../../examples/example-create-session.html) for code examples. There are a few required entries in the session parameters map. OpenCMIS has to know which binding you want to use and where to find the CMIS endpoint. Most repositories also need a username and a password to identify the user. (See the authentication section for more details.)

The repository id parameter tells OpenCMIS which repository at this CMIS endpoint it should talk to. How do you get this repository id is repository specific. The SessionFactory can also fetch a list of all available repositories. 

There are also a number of optional [session parameters](../dev-session-parameters.html) that control the cache behavior, HTTP settings, etc. Leave them alone as long as you don't want or have to optimize your setup.

The Session object gives you access to all CMIS features.  There are a few common and repeating patterns. Have a look at the [examples sections](../../examples/index.html).

When you feel comfortable with the API, familiarize yourself with the [OpenCMIS caches](../dev-client-cache.html) and the [OperationContext](../dev-operation-context.html). They can improve the performance considerably.


## Authentication

The CMIS specification recommends HTTP basic authentication for AtomPub and WS-Security UsernameToken for Web Services. Most repositories support that. If can't find any information in the repository documentation, assume that those are enabled.

OpenCMIS also supports [NTLM](../dev-session-parameters.html) but you should avoid it if you can. It generates some side effects in the JVM and has streaming issues. 

If the repository need requires a different authentication mechanism, you have to implement your own [authentication provider](dev-client-bindings.html).


## CMIS Workbench

The CMIS Workbench is desktop client build on top of OpenCMIS. It lets you see the repository through the eyes of CMIS and OpenCMIS. That can be handy during development.

You should also play with console that is built into the CMIS Workbench. It runs code snippets and lets you experiment with the OpenCMIS client API without setting up a full-blown Java project.


