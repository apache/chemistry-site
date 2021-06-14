Title: OpenCMIS Session Parameters

# OpenCMIS Session Parameters
<a name="OpenCMISSessionParameters-OpenCMISSessionParameters"></a>

Key|Constant|Description|Values|Required|Default
---|------|---------|-----|------|------
org.apache.chemistry.opencmis.binding.spi.type|BINDING_TYPE|Binding to use for the session |"atompub", "webservices", "local", "custom"|yes
org.apache.chemistry.opencmis.binding.spi.classname|BINDING_SPI_CLASS|Binding implementation class|class name|Custom binding: yes<br/>other binding: no|Depends on BINDING_TYPE
org.apache.chemistry.opencmis.session.repository.id|REPOSITORY_ID|Repository id|repository id|createSession(): yes<br/>getRepositories(): no
org.apache.chemistry.opencmis.user|USER|User name<br/>(used by the standard authentication provider)|string
org.apache.chemistry.opencmis.password|PASSWORD|Password<br/>(used by the standard authentication provider)|string
org.apache.chemistry.opencmis.locale.iso639|LOCALE_ISO639_LANGUAGE|Language code sent to server|ISO 639 code|no
org.apache.chemistry.opencmis.locale.iso3166|LOCALE_ISO3166_COUNTRY|Country code sent to server if language code is set|ISO 3166 code|no
org.apache.chemistry.opencmis.binding.atompub.url|ATOMPUB_URL|AtomPub service document URL|URL|AtomPub binding: yes<br/>other bindings: no
org.apache.chemistry.opencmis.binding.webservices.RepositoryService|WEBSERVICES_REPOSITORY_SERVICE|Repository Service WSDL URL|WSDL URL|Web Services binding: yes<br/>other bindings: no
org.apache.chemistry.opencmis.binding.webservices.NavigationService|WEBSERVICES_NAVIGATION_SERVICE|Navigation Service WSDL URL|WSDL URL|Web Services binding: yes<br/>other bindings: no
org.apache.chemistry.opencmis.binding.webservices.ObjectService|WEBSERVICES_OBJECT_SERVICE|Object Service WSDL URL|WSDL URL|Web Services binding: yes<br/>other bindings: no
org.apache.chemistry.opencmis.binding.webservices.VersioningService|WEBSERVICES_VERSIONING_SERVICE|Versioning Service WSDL URL|WSDL URL|Web Services binding: yes<br/>other bindings: no
org.apache.chemistry.opencmis.binding.webservices.DiscoveryService|WEBSERVICES_DISCOVERY_SERVICE|Discovery Service WSDL URL|WSDL URL|Web Services binding: yes<br/>other bindings: no
org.apache.chemistry.opencmis.binding.webservices.RelationshipService|WEBSERVICES_RELATIONSHIP_SERVICE|Relationship Service WSDL URL|WSDL URL|Web Services binding: yes<br/>other bindings: no
org.apache.chemistry.opencmis.binding.webservices.MultiFilingService|WEBSERVICES_MULTIFILING_SERVICE|Multifiling Service WSDL URL|WSDL URL|Web Services binding: yes<br/>other bindings: no
org.apache.chemistry.opencmis.binding.webservices.PolicyService|WEBSERVICES_POLICY_SERVICE|Policy Service WSDL URL|WSDL URL|Web Services binding: yes<br/>other bindings: no
org.apache.chemistry.opencmis.binding.webservices.ACLService|WEBSERVICES_ACL_SERVICE|ACL Service WSDL URL|WSDL URL|Web Services binding: yes<br/>other bindings: no
org.apache.chemistry.opencmis.binding.webservices.memoryThreshold|WEBSERVICES_MEMORY_THRESHOLD|Documents smaller than the threshold are kept in main memory, larger documents are written to a temporary file|size in bytes|no|4194304 (4MB)
org.apache.chemistry.opencmis.binding.local.classname|LOCAL_FACTORY|Class name of the local service factory (if client and server reside in the same JVM)|class name|Local binding: yes<br/>other bindings: no
org.apache.chemistry.opencmis.binding.auth.classname|AUTHENTICATION_PROVIDER_CLASS|Authentication Provider|class name|no|org.apache.chemistry.opencmis.client.bindings.spi.StandardAuthenticationProvider
org.apache.chemistry.opencmis.binding.auth.http.basic|AUTH_HTTP_BASIC|Switch to turn HTTP basic authentication on or off|"true", "false"|no|Depends on BINDING_TYPE
org.apache.chemistry.opencmis.binding.auth.soap.usernametoken|AUTH_SOAP_USERNAMETOKEN|Switch to turn UsernameTokens on or off|"true", "false"|no|Depends on BINDING_TYPE
org.apache.chemistry.opencmis.binding.compression|COMPRESSION|Switch to turn HTTP response compression on or off|"true", "false"|no|false
org.apache.chemistry.opencmis.binding.clientcompression|CLIENT_COMPRESSION|Switch to turn HTTP request compression on or off|"true", "false"|no|false
org.apache.chemistry.opencmis.binding.cookies|COOKIES|Switch to turn cookie support on or off|"true", "false"|no|false
org.apache.chemistry.opencmis.binding.connecttimeout|CONNECT_TIMEOUT|HTTP connect timeout|time in milliseconds|no|*JVM default*
org.apache.chemistry.opencmis.binding.readtimeout|READ_TIMEOUT|HTTP read timeout|time in milliseconds|no|*JVM default*
org.apache.chemistry.opencmis.binding.proxyuser|PROXY_USER|proxy user<br/>(used by the standard authentication provider)|string|no
org.apache.chemistry.opencmis.binding.proxypassword|PROXY_PASSWORD|proxy password<br/>(used by the standard authentication provider)|string|no
org.apache.chemistry.opencmis.cache.classname|CACHE_CLASS|Cache implementation|class name|no|org.apache.chemistry.opencmis.client.runtime.cache.CacheImpl
org.apache.chemistry.opencmis.cache.objects.size|CACHE_SIZE_OBJECTS|Object cache size|number of objects|no|1000
org.apache.chemistry.opencmis.cache.objects.ttl|CACHE_TTL_OBJECTS|Object cache time-to-live|time in milliseconds|no|7200000 (2 hours)
org.apache.chemistry.opencmis.cache.pathtoid.size|CACHE_SIZE_PATHTOID|Path-to-id cache size|number of path to object links|no|1000
org.apache.chemistry.opencmis.cache.pathtoid.ttl|CACHE_TTL_PATHTOID|Path-to-id cache time-to-live|time in milliseconds|no|1800000 (30 minutes)
org.apache.chemistry.opencmis.cache.path.omit|CACHE_PATH_OMIT|Turn off path-to-id cache|"true", "false"|no|false
org.apache.chemistry.opencmis.binding.cache.repositories.size|CACHE_SIZE_REPOSITORIES|Repository info cache size|number of objects|no|10
org.apache.chemistry.opencmis.binding.cache.types.size|CACHE_SIZE_TYPES|Type definition cache size|number of objects|no|100 
org.apache.chemistry.opencmis.binding.cache.links.size|CACHE_SIZE_LINKS|AtomPub link cache size|number of objects|no|400
org.apache.chemistry.opencmis.objectfactory.classname|OBJECT_FACTORY_CLASS|Object factory implementation |class name|no|org.apache.chemistry.opencmis.client.runtime.repository.ObjectFactoryImpl