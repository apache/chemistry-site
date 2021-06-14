Title:     OSGi Support for OpenCMIS

# OSGi Support for OpenCMIS

OpenCMIS builds its own OSGi bundle wrappers for using OpenCMIS client and server within an OSGi runtime. Both bundles can be found in following modules:

* **chemistry-opencmis-osgi**
* **chemistry-opencmis-osgi-client**
* **chemistry-opencmis-osgi-server**

The single bundles include all required libraries build by OpenCMIS while making use of the Bundle-ClassPath entry in MANIFEST-MF file. Basically the bundle contains all chemistry libraries required for client or server.
Other dependencies have to be deployed to OSGi runtime and are configured via the Import-Package entry in MANIFEST.MF.  Maven will help you for doing a deep analysis of all required secondary bundles:

    mvn dependency:tree

This results in the Maven output below to get a complete list of dependencies and their used versions. Most of this dependend components are already build as OSGi bundles. For those which do not build a bundle the Springsource Bundle Repository can be used to find appropriate bundle wrappers.

* [Spring Source Enterprise Bundle Repository][1]
* [Example code for OSGI usage](/java/examples/example-osgi.html) 

###### Maven.out:

    [INFO] ------------------------------------------------------------------------
    [INFO] Building OpenCMIS OSGi Client Wrapper 0.6.0-SNAPSHOT
    [INFO] ------------------------------------------------------------------------
    [INFO] 
    [INFO] --- maven-dependency-plugin:2.1:tree (default-cli) @ chemistry-opencmis-osgi-client ---
    [INFO] org.apache.chemistry.opencmis:chemistry-opencmis-osgi-client:bundle:0.6.0-SNAPSHOT
    [INFO] +- org.apache.chemistry.opencmis:chemistry-opencmis-commons-api:jar:0.6.0-SNAPSHOT:compile
    [INFO] +- org.apache.chemistry.opencmis:chemistry-opencmis-commons-impl:jar:0.6.0-SNAPSHOT:compile
    [INFO] |  \- com.sun.xml.ws:jaxws-rt:jar:2.1.7:compile
    [INFO] |     +- javax.xml.ws:jaxws-api:jar:2.1:compile
    [INFO] |     |  +- javax.xml.bind:jaxb-api:jar:2.1:compile
    [INFO] |     |  +- javax.xml.soap:saaj-api:jar:1.3:compile
    [INFO] |     |  +- javax.annotation:jsr250-api:jar:1.0:compile
    [INFO] |     |  \- javax.jws:jsr181-api:jar:1.0-MR1:compile
    [INFO] |     +- com.sun.xml.bind:jaxb-impl:jar:2.1.11:compile
    [INFO] |     +- com.sun.xml.messaging.saaj:saaj-impl:jar:1.3.3:compile
    [INFO] |     +- com.sun.xml.stream.buffer:streambuffer:jar:0.9:compile
    [INFO] |     |  \- javax.activation:activation:jar:1.1:compile
    [INFO] |     +- org.codehaus.woodstox:wstx-asl:jar:3.2.3:compile
    [INFO] |     |  \- stax:stax-api:jar:1.0.1:compile
    [INFO] |     +- org.jvnet.staxex:stax-ex:jar:1.2:compile
    [INFO] |     |  \- javax.xml.stream:stax-api:jar:1.0:compile
    [INFO] |     +- com.sun.org.apache.xml.internal:resolver:jar:20050927:compile
    [INFO] |     \- org.jvnet:mimepull:jar:1.3:compile
    [INFO] +- org.apache.chemistry.opencmis:chemistry-opencmis-client-api:jar:0.6.0-SNAPSHOT:compile
    [INFO] +- org.apache.chemistry.opencmis:chemistry-opencmis-client-impl:jar:0.6.0-SNAPSHOT:compile
    [INFO] |  \- org.apache.felix:org.osgi.core:jar:1.0.0:compile
    [INFO] +- org.apache.chemistry.opencmis:chemistry-opencmis-client-bindings:jar:0.6.0-SNAPSHOT:compile
    [INFO] +- commons-codec:commons-codec:jar:1.4:compile
    [INFO] +- commons-logging:commons-logging:jar:1.1.1:compile
    [INFO] +- log4j:log4j:jar:1.2.16:test
    [INFO] \- junit:junit:jar:4.7:test (scope not updated to compile)
    [INFO]                                                                         
    [INFO] ------------------------------------------------------------------------
    [INFO] Building OpenCMIS OSGi Server Wrapper 0.6.0-SNAPSHOT
    [INFO] ------------------------------------------------------------------------
    [INFO] 
    [INFO] --- maven-dependency-plugin:2.1:tree (default-cli) @ chemistry-opencmis-osgi-server ---
    [INFO] org.apache.chemistry.opencmis:chemistry-opencmis-osgi-server:bundle:0.6.0-SNAPSHOT
    [INFO] +- org.apache.chemistry.opencmis:chemistry-opencmis-commons-api:jar:0.6.0-SNAPSHOT:compile
    [INFO] +- org.apache.chemistry.opencmis:chemistry-opencmis-commons-impl:jar:0.6.0-SNAPSHOT:compile
    [INFO] |  \- com.sun.xml.ws:jaxws-rt:jar:2.1.7:compile
    [INFO] |     +- javax.xml.ws:jaxws-api:jar:2.1:compile
    [INFO] |     |  +- javax.xml.bind:jaxb-api:jar:2.1:compile
    [INFO] |     |  +- javax.xml.soap:saaj-api:jar:1.3:compile
    [INFO] |     |  +- javax.annotation:jsr250-api:jar:1.0:compile
    [INFO] |     |  \- javax.jws:jsr181-api:jar:1.0-MR1:compile
    [INFO] |     +- com.sun.xml.bind:jaxb-impl:jar:2.1.11:compile
    [INFO] |     +- com.sun.xml.messaging.saaj:saaj-impl:jar:1.3.3:compile
    [INFO] |     +- com.sun.xml.stream.buffer:streambuffer:jar:0.9:compile
    [INFO] |     |  \- javax.activation:activation:jar:1.1:compile
    [INFO] |     +- org.codehaus.woodstox:wstx-asl:jar:3.2.3:compile
    [INFO] |     |  \- stax:stax-api:jar:1.0.1:compile
    [INFO] |     +- org.jvnet.staxex:stax-ex:jar:1.2:compile
    [INFO] |     |  \- javax.xml.stream:stax-api:jar:1.0:compile
    [INFO] |     +- com.sun.org.apache.xml.internal:resolver:jar:20050927:compile
    [INFO] |     \- org.jvnet:mimepull:jar:1.3:compile
    [INFO] +- org.apache.chemistry.opencmis:chemistry-opencmis-server-support:jar:0.6.0-SNAPSHOT:compile
    [INFO] +- org.apache.chemistry.opencmis:chemistry-opencmis-server-bindings:jar:0.6.0-SNAPSHOT:compile
    [INFO] |  +- commons-fileupload:commons-fileupload:jar:1.2.1:compile
    [INFO] |  +- commons-io:commons-io:jar:2.0.1:compile
    [INFO] |  +- commons-lang:commons-lang:jar:2.6:compile
    [INFO] |  \- com.googlecode.json-simple:json-simple:jar:1.1:compile
    [INFO] +- org.apache.chemistry.opencmis:chemistry-opencmis-server-inmemory:jar:0.6.0-SNAPSHOT:compile
    [INFO] |  \- org.apache.chemistry.opencmis:chemistry-opencmis-client-bindings:jar:0.6.0-SNAPSHOT:compile
    [INFO] +- org.apache.chemistry.opencmis:chemistry-opencmis-test-util:jar:0.6.0-SNAPSHOT:compile
    [INFO] |  \- net.sf.jopt-simple:jopt-simple:jar:3.2:compile
    [INFO] +- org.antlr:antlr-runtime:jar:3.2:compile
    [INFO] |  \- org.antlr:stringtemplate:jar:3.2:compile
    [INFO] |     \- antlr:antlr:jar:2.7.7:compile
    [INFO] +- commons-codec:commons-codec:jar:1.4:compile
    [INFO] +- commons-logging:commons-logging:jar:1.1.1:compile
    [INFO] +- log4j:log4j:jar:1.2.16:test
    [INFO] \- junit:junit:jar:4.7:test (scope not updated to compile)

[1]: http://ebr.springsource.com/repository/app/ "Spring Source Enterprise Bundle Repository"

