Title: How To Build A Server

**This guide is outdated!** There is a more up-to-date 
[OpenCMIS Server Development Guide on GitHub](https://github.com/cmisdocs/ServerDevelopmentGuide).


[TOC]

# How to create a CMIS server using OpenCMIS
<a name="HowToBuildAServer-HowtocreateaCMISserverusingOpenCMIS"></a>

This document contains a step-by-step introduction how you can use opencmis
to build an CMIS server. The document is divided into the following
sections:

Getting started. (download, setup Eclipse, etc.)

1. Implementing the services
1. The ServiceWrapper
1. Differences between SOAP and AtomPub (ObjectHolder)
1. Running the server
1. Handling Authentication
1. Testing the server

<a name="HowToBuildAServer-Gettingstarted:"></a>
## Getting started:

The following step-by-step guide is a sample how to create a web
application acting as CMIS server. It will support both bindings web
services and AtomPub.

The following section describes how to initially setup a project to compile
a CMIS server. Please note that CMIS comes with two built-in servers. The
fileshare and the in-memory server. It is a good hint for all upcoming
questions to look at these implementations as example code containing
working implementations.

<a name="HowToBuildAServer-Usingmaven"></a>
### Using maven

Using maven is the easiest way to get started. OpenCMIS itself is using
maven and you can get easily your setup and dependencies using maven. This
requires that you have a working maven environment. In case you don't yet
have one you can find instructions [here](http://maven.apache.org/guides/getting-started/maven-in-five-minutes.html).

The first step is to create your initial pom.xml file to build your project
and to setup the dependencies. The following steps require that you have
build opencmis and installed it to your local maven repository (`mvn
install`). See [here](how-to-build.html)
 for detailed instructions how to do this.

You can create your initial setup using maven itself (adjust package name,
version number and so on according to your needs):


    mvn archetype:generate -DgroupId=org.mycmis -DartifactId= mycmissrv \
    -DpackageName=local.mycmis -Dversion=0.1


Then select option 18 `maven-archetype-webapp` and confirm settings.

You will find a project setup then consisting of a directory `mycmissvr`
and subdirectories for source code and test code and the Java packages. We
need to adapt the `.pom` file so that maven creates a web application file
that can be deployed in a servlet container (like Apache Tomcat or Jetty).
We also need to setup the dependencies to opencmis and we need to instruct
maven generating an overlay with the existing server code in opencmis.
Please remove therefore the entire generated `src/webapp` directory and all
included files (`*.jsp`, `web.xml`).
The final `pom.xml` will look like this:

```xml
    <project xmlns="http://maven.apache.org/POM/4.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
      <modelVersion>4.0.0</modelVersion>
      <groupId>org.mycmis</groupId>
      <artifactId>mycmissrv</artifactId>
      <packaging>war</packaging>
      <version>0.1</version>
      <name>mycmissrv Maven Webapp</name>
      <url>http://maven.apache.org</url>
    
      <properties>
        <opencmisVersion>0.1.0-incubating-SNAPSHOT</opencmisVersion>
        <opencmisGroupId>org.apache.chemistry.opencmis</opencmisGroupId>
      </properties>
    
      <build>
        <finalName>mycmissrv</finalName>
        <plugins>
          <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-war-plugin</artifactId>
            <configuration>
              <overlays>
                <overlay>
                </overlay>
                <overlay>
                  <groupId>${opencmisGroupId}</groupId>
                  <artifactId>chemistry-opencmis-server-bindings</artifactId>
                </overlay>
              </overlays>
            </configuration>
          </plugin>
          <plugin>
            <artifactId>maven-compiler-plugin</artifactId>
            <configuration>
              <target>1.5</target>
              <source>1.5</source>
              <encoding>UTF-8</encoding>
            </configuration>
          </plugin>
        </plugins>
      </build>
  
      <dependencies>
        <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>3.8.1</version>
          <scope>test</scope>
        </dependency>
        <dependency>
          <groupId>${opencmisGroupId}</groupId>
          <artifactId>chemistry-opencmis-commons-api</artifactId>
          <version>${opencmisVersion}</version>
        </dependency>
        <dependency>
          <groupId>${opencmisGroupId}</groupId>
          <artifactId>chemistry-opencmis-commons-impl</artifactId>
          <version>${opencmisVersion}</version>
        </dependency>
        <dependency>
          <groupId>${opencmisGroupId}</groupId>
          <artifactId>chemistry-opencmis-test-util</artifactId>
          <version>${opencmisVersion}</version>
        </dependency>
        <dependency>
          <groupId>${opencmisGroupId}</groupId>
          <artifactId>chemistry-opencmis-server-bindings</artifactId>
          <version>${opencmisVersion}</version>
          <type>war</type>
       </dependency>
        <dependency>
          <groupId>${opencmisGroupId}</groupId>
          <artifactId>chemistry-opencmis-server-support</artifactId>
          <version>${opencmisVersion}</version>
        </dependency>
        <dependency>
          <groupId>org.antlr</groupId>
          <artifactId>antlr-runtime</artifactId>
          <version>3.1.3</version>
        </dependency>  
      </dependencies>
    </project>
```

You will get your `web.xml` and other required files for your web
application from opencmis which you may adjust according to your needs. You
may run `mvn eclipse:eclipse` to generate the required `.projects` and
`.classpath` files for Eclipse that you just need to import in your
Eclipse workspace.


### Without maven
    
If you do not want to use maven to build your project downlod from the downloads
page the package "OpenCMIS Server Framework". Unzip the file and copy
the contents to your project. It includes all required jars, web.xml and
other supporting files. It follow the structure of a web application (.war).
The web application can be deployed but does not have any functionality.

Edit the file `repository.properties` as a first step (see below) and take
care that it is in your classpath.

You also have an initial `web.xml` and the required support files for the
web services binding. Unless you have specific requirements you do not need
to modify them.
    
## Implementing the services
    
The first step is to tell opencmis where it can find your classes that
implement the CMIS logic. For this purpose a file named
repository.properties must be in the classpath. It must contain a property
named class that refers to your service factory:

    class=local.mycmis.ServiceFactory


The ServiceFactory class should extend the `AbstractServiceFactory` class
from org.apache.chemistry.opencmis.commons.impl.server. You have to
implement the interface CmisServiceFactory:

```java
    public void init(Map<String, String> parameters) {
    }
    
    public void destroy() {
    }
    
    public abstract CmisService getService(CallContext context);
```

The `init()` method is used to perform initialization and passes a set of
configuration parameters from repository.properties:

```java
    @Override
    public void init(Map<String, String> parameters) {
    }
```

Typically you will create an instance of your service implementation.
    
There is also wrapper classes is explained in the next section.
    
The next step is that you need to implement the services according to the
CMIS spec. We will do this here for one example call. The important piece
is that your class implements `CmisService` which is central access point
to all incoming calls. Here is an implementation of an example call
`getRepositoryInfo()`:

```java
    public class MyCmisServiceImpl extends AbstractCmisService {
    
        public RepositoryInfo getRepositoryInfo(CallContext context,
          String repositoryId, ExtensionsData extension) {
    
        	RepositoryInfoImpl repoInfo = new RepositoryInfoImpl();
        	repoInfo.setRepositoryId(repositoryId);
        	repoInfo.setRepositoryName("My CMIS Repository");
        	repoInfo.setRepositoryDescription("Sample Repository");
        	repoInfo.setCmisVersionSupported("1.0");
        	repoInfo.setRepositoryCapabilities(null);
        	repoInfo.setRootFolder("MyRootFolderId");
        	repoInfo.setPrincipalAnonymous("anonymous");
        	repoInfo.setPrincipalAnyone("anyone");
        	repoInfo.setThinClientUri(null);
        	repoInfo.setChangesIncomplete(Boolean.TRUE);
        	repoInfo.setChangesOnType(null);
        	repoInfo.setLatestChangeLogToken(null);
        	repoInfo.setVendorName("ACME");
        	repoInfo.setProductName("ACME CMIS Connector");
        	repoInfo.setProductVersion("0.1");
    
        	// set capabilities
        	RepositoryCapabilitiesImpl caps = new RepositoryCapabilitiesImpl();
        	caps.setAllVersionsSearchable(false);
        	caps.setCapabilityAcl(CapabilityAcl.NONE);
        	caps.setCapabilityChanges(CapabilityChanges.PROPERTIES);
    
        	caps.setCapabilityContentStreamUpdates(CapabilityContentStreamUpdates.PWCONLY);
        	caps.setCapabilityJoin(CapabilityJoin.NONE);
        	caps.setCapabilityQuery(CapabilityQuery.METADATAONLY);
        	caps.setCapabilityRendition(CapabilityRenditions.NONE);
        	caps.setIsPwcSearchable(false);
        	caps.setIsPwcUpdatable(true);
        	caps.setSupportsGetDescendants(true);
        	caps.setSupportsGetFolderTree(true);
        	caps.setSupportsMultifiling(true);
        	caps.setSupportsUnfiling(true);
        	caps.setSupportsVersionSpecificFiling(false);
        	repoInfo.setRepositoryCapabilities(caps);
    
        	AclCapabilitiesDataImpl aclCaps = new AclCapabilitiesDataImpl();
        	aclCaps.setAclPropagation(AclPropagation.REPOSITORYDETERMINED);
        	aclCaps.setPermissionDefinitionData(null);
        	aclCaps.setPermissionMappingData(null);
        	repoInfo.setAclCapabilities(aclCaps);
    
        	return repoInfo;
          }
         // ...
    }
```    

Now you can start implementing all the required methods with their methods.

<a name="HowToBuildAServer-TheServiceWrapper"></a>
### The ServiceWrapper

For the CmisService interface exists a corresponding abstract class. This
can be used as a starting point for your implementation. This is optional
but gives you the benefit of providing many common null pointer checks and
default values for optional parameters. The abstract class also provides a
default implementation for generating the ObjectInfo needed for the AtomPub
binding. This wrapper can reduce the code required in your service
implementation. You should always start using the wrapper class and
directly implement the CmisService interface only when necessary. It is
strongly recommended however to provide a better implementation for create
the ObjectInfo however (see next section for details).

<a name="HowToBuildAServer-DifferencesbetweentheCMISbindings"></a>
## Differences between the CMIS bindings

OpenCMIS supports both the AtomPub and the web services binding interface.
It hides all the details how to handle the protocol but there are some
subtle differences that need to be reflected in the Java interfaces. This
chapter explains where the differences are and why they are needed.

<a name="HowToBuildAServer-TheObjectInfointerface"></a>
### The ObjectInfo interface

The methods in the interfaces `CmisService` are following the data model
in the CMIS specification. The specification defines the following
services:

* AclService
* DiscoveryService
* ObjectService
* MultifFlingService
* NavigationService
* PolicyService
* RelationshipService
* RepositoryService
* Versioning Service

For each of these services exists a corresponding interface in opencmis.
CmisService is an interface that unifies all interfaces in one interface.
The methods in this interface correspond to the methods in the
specification. For the web service binding this is a one-to-one mapping.
For the AtomPub binding this information is not sufficient in all cases.
Take the creation of a document as an example. The web service returns in
this case a single string value containing the id of the created document.
A response in the AtomPub binding however consists of an AtomPub entry
element which is an XML fragment (for an example look at
`DocumentEntry.xml` in the examples directory of the CMIS specification).
You will notice that generating this XML requires much more information
than the create...() methods in the specification provide as return value.
Your implementation needs to provide all the information so that opencmis
can generate the complete response. For this purpose the `ObjectInfo` was
introduced. `getObjectInfo` is the method of the `CmisService` interface
that provides this information. The `AbstractCmisService` contains a
member `objectInfoMap` that you can fill with this information. If you
ignore this the class `AbstractCmisService` contains a default
implementation which works but is very ineffecient. You should use this
only as a starting point. Beware that a service can be called from multiple
threads, so have to take care to handle threading issues properly.

If the method you are implementing returns a list of objects and not only a
single value (for example methods like `getChildren()`, `getDescendants()`
in the navigation service) you need to provide an `ObjectInfo` for each
element in the collection. `getObjectInfo` therefore returns a map with
the object id for each object as key and the corresponding `ObjectInfo`
as value. You can use the method `addObjectInfo` to add an element to the
map. 



<a name="HowToBuildAServer-Thecreate()methods"></a>
### The create() methods

The web service binding has separate calls for each object type to be
created: policies, relationships, documents and folders:

* createDocument
* createFolder
* createRelationship
* createPolicy

In the AtomPub binding there is one general `create()` method that is
used for all object types. The CmisObjectService interface therefore
contains 5 `create()` methods, the four specific ones are used for the
web service bindings and the general one for the AtomPub binding.


<a name="HowToBuildAServer-ApplyingACLs"></a>
### Applying ACLs

The class `CmisAclService` has two methods `applyAcl()`. One uses two
`AccessControlList`s with one `Acl` to add &nbsp;and one to remove. The other
method contains only one `Acl` to be set on the target object. The web
service binding uses the method with two parameters, the AtomPub binding
uses the method with one parameter.

<a name="HowToBuildAServer-RunningTheServer"></a>
## Running The Server

When your services are implemented (or parts of them) you can try to run
the server in a servlet container like Tomcat. If you have used maven to
setup your project you can run

    mvn package


to generate a war file that you will find in the target directory. If you
have not used maven, create the war with the mechanisms of your build
environment. Check the war contains a `web.xml`, the wsdl files in
`WEB-INF` and all the required jars from opencmis and dependent libraries
in `WEB-INF/lib`. For jetty maven contains a build-in integration that
you may use if you like using `mvn jetty:run`. Adjust your `pom.xml` in
this case accordingly.

Deploy your application and start testing.

<a name="HowToBuildAServer-HandlingAuthentication"></a>
## Handling Authentication

Opencmis does not provide or expect any specific mechanism how
authentication is handled. It provides some basic mechanisms hot to extract
user name and password depending on the protocol. For AtomPub binding http
basic authentication is supported, for web services WS-Security 1.1 for
Username Token Profile 1.1 is supported.

If you want to use servlet filters dealing with authentication, just
add them to your `web.xml` file. You also can handle authentication inside of
your code. In this case derive a class from `BasicAuthCallContextHandler`
and implement method `getCallContextMap()`. There you have access to user
name and password as provided by the user. By default opencmis does not
enforce authentication, so initially the map will be null. If you raise a
`CmisPermissionDeniedException` the exception is caught by the server
implementation and a `401` http return code is sent as response to the
browser. This usually opens a dialog for user name and password then.

```java
    public class MyContextHandler
        extends BasicAuthCallContextHandler
    {
      public Map<String, String> getCallContextMap(HttpServletRequest request){

        // call superclass to get user and password via basic authentication
        Map<String, String>  ctxMap = super.getCallContextMap(request);

        if (null == ctxMap)
          // no user name, password given yet say: we need authentication:
          throw new CmisPermissionDeniedException("Authentication required");
    
        // call your authentication

        MyAuthentication.login(
	        ctxMap.get(CallContext.USERNAME),
	        ctxMap.get(CallContext.PASSWORD));

        return ctxMap;
      }
    }
```
    
Beyond this it is up to you how to implement authentication. Creating
tokens for example, using cookies, etc. is not covered by opencmis, but you
can add it in your code.


## Configuring your server
    
Opencmis reads a file `repository.properties` on startup. By default you only
have to configure the class of your service factory (see above). You can
add additional properties in this file. These configuration parameters are
then passed to the `init()` method of your `ServiceFactory` as key
value pairs in a hashmap.

<DIV class="codeHeader">repository.properties</DIV>

    class=local.mycmis.ServiceFactory
    myparam=my-configuration-value


<DIV class="codeHeader">local.mycmis.ServiceFactory.java</DIV>

```java
    // ...
    @Override
    public void init(Map<String, String> parameters) {
      String myParamValue = parameters.get("myparam");
             // use myParamValue
    }
    // ...
```


## Testing the server
    
There are various ways how you can test your implementation. You may add
unit tests that directly call your service implementations as a first step.
Opencmis also contains some basic tests that perform client-server
communication. You can choose the protocol binding and whether read-only or
read-write tests are performed. The amount of functionality tested depends
on the capabilities returned in your getRepositoryInfo return value. You
can run them using junit:
    
Examples:
    
Test class:

    org.apache.chemistry.opencmis.client.bindings.atompub.SimpleReadOnlyTests


Test Parameters (passed as JVM args):

    -Dopencmis.test=true
    -Dopencmis.test.username=myuser
    -Dopencmis.test.password=mypasswd
    -Dopencmis.test.repository=my_repository_id&nbsp;
    -Dopencmis.test.atompub.url=[http://localhost:8080/opencmis/atom]


The following test classes exist:

    org.apache.opencmis.client.bindings.atompub.SimpleReadOnlyTests
    org.apache.opencmis.client.bindings.atompub.SimpleReadWriteTests
    org.apache.opencmis.client.bindings.webservices.SimpleReadOnlyTests
    org.apache.opencmis.client.bindings.webservices.SimpleReadWriteTests


For web services you need an additional parameter for the service endpoint:

    -Dopencmis.test.webservices.url=[http://localhost:8080/opencmis/services/]


### Test using curl
    
Simple tests (which might be useful at the beginning) can also be done
using tools like curl or wget (AtomPub binding only). A simple example for
a `.BAT` file (Windows) would look like this:

```bat
    rem set PATH=...\libcurl-7.19.7;...\OpenSSL\bin;%PATH%
    set CURL=curl.exe
    set VIEWER="C:\Program Files\Mozilla Firefox\firefox.exe"
    set USER=XXX
    set PWD=YYY
    set URLPREFIX=[http://localhost:8080/opencmis/atom]
    SET OUTFILE=atom.xml
    
    %CURL% \--user %USER%:%PWD% \--dump-header header.txt \--output %OUTFILE% %URLPREFIX%
    IF ERRORLEVEL 0 %VIEWER% %OUTFILE%
```

Another example how to get a document with id MyDocument:

    %CURL% \--user %USER%:%PWD% \--dump-header header.txt \--output %OUTFILE% %URLPREFIX%/A1/entry?id=%%2FMyDocument


You also can use this to create documents:

    %CURL% \--user %USER%:%PWD% \--header "Content-Type: application/atom+xml;type=entry" \-d @post-item.xml \--output %OUTFILE%&nbsp; \--url %URLPREFIX%/A1/children?id=/


The file `post-item.xml` must contain a valid Atom entry then:

```xml
    <?xml version="1.0" encoding="utf-8"?>
    <entry xmlns="http://www.w3.org/2005/Atom"
        xmlns:cmis="http://docs.oasis-open.org/ns/cmis/core/200908/"
        xmlns:cmisra="http://docs.oasis-open.org/ns/cmis/restatom/200908/"
        xmlns:app="http://www.w3.org/2007/app">
      <id>http://vmelcmis1:8080/cmis/atom/cd-library/main/Default-I10915</id>
      <author>
        <name>Admin</name>
      </author>
      <updated>2009-03-01T00:00:00.000Z</updated>
      <title type="text">Content from Curl</title>
      <cmisra:object>
        <cmis:properties>
          <cmis:propertyId propertyDefinitionId="cmis:name">
    	<cmis:value>Content from Curl</cmis:value>
          </cmis:propertyId>
          <cmis:propertyId propertyDefinitionId="cmis:objectTypeId">
    	<cmis:value>cmis:document</cmis:value>
          </cmis:propertyId>
        </cmis:properties>
      </cmisra:object>
    
      <cmisra:content type="text/plain">
      <cmisra:base64>
    RmF1c3Q6DQoNCk1laW4gc2No9m5lcyBGcuR1bGVpbiwgZGFyZiBpY2ggd2FnZW4sDQpNZWlu
    ZW4gQXJtIHVuZCBHZWxlaXQgSWhyIGFuenV0cmFnZW4/DQoNCk1hcmdhcmV0ZToNCg0KQmlu
    IHdlZGVyIEZy5HVsZWluLCB3ZWRlciBzY2j2biwNCkthbm4gdW5nZWxlaXRldCBuYWNoIEhh
    dXNlIGdlaG4uDQoNCihTaWUgbWFjaHQgc2ljaCBsb3MgdW5kIGFiLikNCg0KRmF1c3Q6DQoN
    CkJlaW0gSGltbWVsLCBkaWVzZXMgS2luZCBpc3Qgc2No9m4hDQpTbyBldHdhcyBoYWIgaWNo
    IG5pZSBnZXNlaG4uDQpTaWUgaXN0IHNvIHNpdHQtIHVuZCB0dWdlbmRyZWljaCwNClVuZCBl
    dHdhcyBzY2huaXBwaXNjaCBkb2NoIHp1Z2xlaWNoLg0KRGVyIExpcHBlIFJvdCwgZGVyIFdh
    bmdlIExpY2h0LA0KRGllIFRhZ2UgZGVyIFdlbHQgdmVyZ2XfIGljaCdzIG5pY2h0IQ0KV2ll
    IHNpZSBkaWUgQXVnZW4gbmllZGVyc2NobORndCwNCkhhdCB0aWVmIHNpY2ggaW4gbWVpbiBI
    ZXJ6IGdlcHLkZ3Q7DQpXaWUgc2llIGt1cnogYW5nZWJ1bmRlbiB3YXIsDQpEYXMgaXN0IG51
    biB6dW0gRW50evxja2VuIGdhciE=
      </cmisra:base64>
      </cmisra:content>
    </entry>
```
