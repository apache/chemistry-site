Title: Using Eclipse as Development Environment

# Using Eclipse

Please note that you can use maven to generate projects for Eclipse (and other
development tools) by using mvn `eclipse:eclipse`. For details see the maven
documentation. This mechanism also works for other IDEs than Eclipse.

## Debugging

### Debugging a client

Using the Eclipse debugger for a client application usually
is straightforward. Just create the launch configuration
(for example Java stand alone application) and run your 
project in the debugger.

### Debugging a server
Using the debugger for server development requires the 
appropriate setup. This is a bit more tricky than on the 
client side. There are several ways how to do this. Choose
whatever works best for your environment. The following 
sections use Apache Tomcat as servlet container for your
server development project. The same mechanism works for
other servlet containers and can easily be adapted
(e.g. Jetty).

#### Tomcat Remote Debugging
Build your server and create a .war archive. Deploy your
archive in Tomcat (e.g. copy it to the webapps directory of 
the Tomcat installation). Start Tomcat in debugging mode. See 
[here][1] for details. Create in Eclipse a launch configuration
for remote debugging connecting to your Tomcat.

There are other options as well. You can skip creating the 
.war file and configure Tomcat to use your build output 
directory as web application. See the Tomcat documentation
for more details.

#### Eclipse WDT
If you use the Eclipse J2EE distribution or have installed
the Web Development Tools (WDT) you can run/debug your 
server directly within your IDE. Unfortunately the 
integration with maven is not very convenient. You have
to patch the file `pom.xml` of your server project so 
that mvn eclipse:eclipse generates a web application. 

```xml
    <build>
      <plugins>
        . . . 
          <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-eclipse-plugin</artifactId>
			<version>2.8</version>
			<configuration>
			   <wtpversion>2.0</wtpversion>
			   <wtpContextName>inmemory</wtpContextName>
			   <linkedResources>
				 <linkedResource>
				   <name>src/main/webapp/WEB-INF/sun-jaxws.xml</name> 
				   <type>1</type> 
				   <location>
					 ${basedir}/chemistry-opencmis-server/chemistry-opencmis-server-bindings/src/main/webapp/WEB-INF/sun-jaxws.xml
				   </location>
				 </linkedResource>
				 <linkedResource>
				   <name>src/main/webapp/WEB-INF/web.xml</name> 
				   <type>1</type> 
				   <location>${basedir}/chemistry-opencmis-server/chemistry-opencmis-server-bindings/src/main/webapp/WEB-INF/web.xml</location>
				 </linkedResource>
				 <linkedResource>
				   <name>src/main/webapp/WEB-INF/wsdl</name> 
				   <type>2</type> 
				   <location>${basedir}/chemistry-opencmis-server/chemistry-opencmis-server-bindings/src/main/webapp/WEB-INF/wsdl</location>
				 </linkedResource>
			   </linkedResources>
			</configuration>            
          </plugin>
```

If you don't use maven you have to manually create a dynamic
web project in Eclipse. In this case `inmemory` is the
context root of your server. An address like
`http://localhost:8080/inmemory/atom/` should work. What
still needs to be done manually (even with maven) is to 
configure all the jars needed to run the server. Open the
project properties in Eclipse and select Deployment Assembly.
Add / Project and add the following projects:

  - chemistry-opencmis-commons-api
  - chemistry-opencmis-commons-impl
  - chemistry-opencmis-server-bindings
  - chemistry-opencmis-server-support

Then Add / Java Build Path Entries and add all the jars 
listed.

This setup allows you to add your server project to an existing
Server configuration in Eclipse. (If your Server tab is 
empty you first have to create one). In the Server Tab now
you can run or debug Tomcat with your server project as a 
deployed web application. 

#### The Local Binding

OpenCMIS allows to bypass all AtomPub or SOAP protocols
and directly connect from a client to server using Java 
classes within a single JVM and is called the Local
Binding. This can be convenient is some cases and makes
debugging very easy. See the example how to 
[create a session][2].

## Long Package Names
The opencmis project uses package names that sometimes
get inconvenient due to their length. Since Helios
Eclipse has a nice feature to abbreviate package names.
In Window/Preferences go to Java / Appearance and
select Abbreviate package names. Add a rule:

   `org.apache.chemistry.opencmis={cmis}`
   
This will display all your packages in a form like

   `{cmis}.commons.api`




  [1]: http://wiki.apache.org/tomcat/FAQ/Developing#Q1
  [2]: /java/examples/example-create-session.html