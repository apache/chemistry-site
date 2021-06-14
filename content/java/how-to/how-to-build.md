Title: OpenCMIS how-to-build

# How to build OpenCMIS

OpenCMIS releases are available [here](../download.html).
If you want to build the latest and greatest instead, follow these simple steps:

* Make sure you have JDK 1.6 or higher, Maven 3 and a Subversion client installed.
* Fetch the source code via Subversion from here: [https://svn.apache.org/repos/asf/chemistry/opencmis/trunk](https://svn.apache.org/repos/asf/chemistry/opencmis/trunk)
* And finally run:

&nbsp;

    mvn clean install -Dmaven.test.skip=true

* Only in case you're the one who builds a new release (which also produces the commodity packages (ZIPs and Tarballs), source and javadoc JARs, and is then published), run:

&nbsp;

    mvn clean install -Papache-release

To produce also commodity packages (ZIPs and Tarballs), source and javadoc JARs 

## The Client Libraries

After the build, the OpenCMIS client libraries (with all dependencies) reside in the
`/chemistry-opencmis-client/chemistry-opencmis-client-impl/target`
directory. The zip file contains all libraries necessary to build a CMIS client.

## The Server Framework

Please refer to the [Server Framework](../developing/dev-server.html) page for more information where to find it and how to use it.


## JavaDocs

### Releases Javadocs

You can access OpenCMIS releases Javadocs in the following ways:

 1. Every release publishes an online Javadoc version
 2. Javadocs are included in the `chemistry-opencmis-docs.zip` and `chemistry-opencmis-docs.tar.gz`
packages for offline browsing


To browse online Javadocs for latest/older releases check the [home page][1]'s Download section.


### Latest Javadocs 

You can access OpenCMIS latest (trunk) Javadocs in the following ways:

 1. Build them from trunk using Maven
 
* Checkout the [project trunk][2] 
* Run `mvn site` or `mvn javadoc:aggregate`
* Open `./target/site/apidocs/index.html`

 2.Access the latest CI Javadoc 
 
* (**TODO**) <del>Latest Javadocs for current SNAPSHOT version (trunk) are produced by our build server 
and available [here][3]</del>



  [1]: http://chemistry.apache.org/java/opencmis.html
  [2]: http://svn.apache.org/repos/asf/chemistry/opencmis/trunk/
  [3]: http://chemistry.apache.org/java/maven/apidocs/index.html