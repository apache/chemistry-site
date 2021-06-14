Title: Debugging OpenCMIS

# Debugging

This section gives variuos hints for debugging OpenCMIS. Debugging on the 
client side entirely depends on your application. So there is not much to
be described here. Debuggin in the server however is often a bit more tricky.
OpenCMIS will usually be run as a server and require a web application 
container like Tomcat. There are several possible techniques you can use
to debug server code. Some of them may depend on your choice of a 
development environment.

## Tips for Server Debugging

### Use the client bindings instead of the API for test code

The client bindings layer allow to give you finer control about the requests
sent to the server. The bindings layer is less convenient to use than the
client API, but it does less caching and no efforts to reduce round-trips. 
This is often helpful when debugging server code.

### Use the AtomPub Binding for debugging

The AtomPub binding allows it for simple requests to use a browser as client.
For advanced scenarios (e.g. requiring http PUT, DELETE requestss) a browser 
won't be sufficient but command line tools can help (sse next section). The 
WS binding in contrast ussually requires writing code to submit requests.

### Use command line tools to trigger single requests

Sometimes it is helpful to easily send a single request to the server repeatedly 
when trying to figure out why something does not work as expected. Often it is 
not necessary to write specific client code fur such situations. There exist
command line tools allowing to send http requests like wget or curl. Creating 
a script using such tools is often faster and easier then writing code. You need 
to understand the opencmis URL syntax to do this and it won't work for the
WS (SOAP) binding.

Here is an example:

> curl --user joe:secret --dump-header header.txt --output item.xml http://localhost:8080/inmemory/atom/A1/entry?filter=cmis:name&id=133

This will get an item with id=133 using a property filter and store the output 
in a file `item.xml`. In addtion all http header will be stored in `header.txt`

> curl --raw --header "Content-Type: image/jpeg" -d @photo.jpg --output response.xml --url http://localhost:8080/opencmis/atom/A1/content?id=133

This will upload file `photo.jpg` as content stream to an item with document id 133.

Usually you will create batch files or shell scripts to avoid a lot of typing. 

Windows users please note that you have to escape the & character in a URL with 
a ^(circumflex) in Windows batch files! 

### Consider the local binding

OpenCMIS allows running client and server code in a single JVM process and bypassing
the AtomPub or SOAP binding layer. Therefore a local binding can be used. This can help
to track down errors where it is not certain if the problem is caused from the bindings
or in the server logic. It is also useful for unit tests. The local binding is created
like this:

```java
	import org.apache.chemistry.opencmis.commons.SessionParameter;
	import org.apache.chemistry.opencmis.commons.impl.dataobjects.BindingsObjectFactoryImpl;
	...
	Map<String, String> parameters = new HashMap<String, String>();
	parameters.put("...", SessionParameter...);
	CmisBindingFactory factory = CmisBindingFactory.newInstance();
	CmisBinding binding = factory.createCmisLocalBinding(parameters);
	fRepSvc = binding.getRepositoryService();
	...
```

For an example you can refer to the unit test code of the in-memory server.

