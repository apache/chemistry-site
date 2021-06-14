Title: DotCMIS Session Parameters

# DotCMIS Session Parameters

Key|Constant|Description|Values|Required|Default
---|------|---------|-----|------|------
org.apache.chemistry.dotcmis.binding.spi.type|BindingType|Binding to use for the session |"atompub", "webservices", "custom"|yes
org.apache.chemistry.dotcmis.binding.spi.classname|BindingSpiClass|Binding implementation class|class name|Custom binding: yes<br/>other binding: no|Depends on BindingType
org.apache.chemistry.dotcmis.session.repository.id|RepositoryId|Repository id|repository id|CreateSession(): yes<br/>GetRepositories(): no
org.apache.chemistry.dotcmis.user|User|User name<br/>(used by the standard authentication provider)|string
org.apache.chemistry.dotcmis.password|Password|Password<br/>(used by the standard authentication provider)|string
org.apache.chemistry.dotcmis.binding.atompub.url|AtomPubUrl|AtomPub service document URL|URL|AtomPub binding: yes<br/>other bindings: no
org.apache.chemistry.dotcmis.binding.webservices.RepositoryService|WebServicesRepositoryService|Repository Service WSDL URL|WSDL URL|Web Services binding: yes<br/>other bindings: no
org.apache.chemistry.dotcmis.binding.webservices.NavigationService|WebServicesNavigationService|Navigation Service WSDL URL|WSDL URL|Web Services binding: yes<br/>other bindings: no
org.apache.chemistry.dotcmis.binding.webservices.ObjectService|WebServicesObjectService|Object Service WSDL URL|WSDL URL|Web Services binding: yes<br/>other bindings: no
org.apache.chemistry.dotcmis.binding.webservices.VersioningService|WebServicesVersioningService|Versioning Service WSDL URL|WSDL URL|Web Services binding: yes<br/>other bindings: no
org.apache.chemistry.dotcmis.binding.webservices.DiscoveryService|WebServicesDiscoveryService|Discovery Service WSDL URL|WSDL URL|Web Services binding: yes<br/>other bindings: no
org.apache.chemistry.dotcmis.binding.webservices.RelationshipService|WebServicesRelationshipService|Relationship Service WSDL URL|WSDL URL|Web Services binding: yes<br/>other bindings: no
org.apache.chemistry.dotcmis.binding.webservices.MultiFilingService|WebServicesMultifilingService|Multifiling Service WSDL URL|WSDL URL|Web Services binding: yes<br/>other bindings: no
org.apache.chemistry.dotcmis.binding.webservices.PolicyService|WebServicesPolicyService|Policy Service WSDL URL|WSDL URL|Web Services binding: yes<br/>other bindings: no
org.apache.chemistry.dotcmis.binding.webservices.ACLService|WebServicesAclService|ACL Service WSDL URL|WSDL URL|Web Services binding: yes<br/>other bindings: no
org.apache.chemistry.dotcmis.binding.webservices.wcfbinding|WebServicesWCFBinding|Name of the WCF binding to use|binding name|no
org.apache.chemistry.dotcmis.binding.webservices.opentimeout|WebServicesOpenTimeout|WCF binding open timeout|TimeSpan format|no|*CLR default*
org.apache.chemistry.dotcmis.binding.webservices.closetimeout|WebServicesCloseTimeout|WCF binding close timeout|TimeSpan format|no|*CLR default*
org.apache.chemistry.dotcmis.binding.webservices.sendtimeout|WebServicesSendTimeout|WCF binding send timeout|TimeSpan format|no|*CLR default*
org.apache.chemistry.dotcmis.binding.webservices.sendtimeout|WebServicesReceiveTimeout|WCF binding receive timeout|TimeSpan format|no|*CLR default*
org.apache.chemistry.dotcmis.binding.webservices.enableUnsecuredResponse|WebServicesEnableUnsecuredResponse|Indicates whether unsecured response is permitted (requires .NET 4.0)|"true", "false"|no|false
org.apache.chemistry.dotcmis.binding.auth.classname|AuthenticationProviderClass|Authentication Provider<br/>(DotCMIS provides two authentication providers out-of-the-box:<br/>basic authentication = DotCMIS.Binding.StandardAuthenticationProvider<br/>NTLM authentication (since 0.5) = DotCMIS.Binding.NtlmAuthenticationProvider)|class name|no|DotCMIS.Binding.StandardAuthenticationProvider
org.apache.chemistry.dotcmis.binding.compression|Compression|Switch to turn HTTP response compression on or off|"true", "false"|no|false
org.apache.chemistry.dotcmis.binding.connecttimeout|ConnectTimeout|HTTP connect timeout|time in milliseconds or -1 for infinite|no|*CLR default*
org.apache.chemistry.dotcmis.binding.readtimeout|ReadTimeout|HTTP read timeout|time in milliseconds or -1 for infinite|no|*CLR default*
org.apache.chemistry.dotcmis.cache.classname|CacheClass|Cache implementation|class name|no|DotCMIS.Client.Impl.Cache.CmisObjectCache
org.apache.chemistry.dotcmis.cache.objects.size|CacheSizeObjects|Object cache size|number of objects|no|1000
org.apache.chemistry.dotcmis.cache.objects.ttl|CacheTTLObjects|Object cache time-to-live|time in milliseconds|no|7200000 (2 hours)
org.apache.chemistry.dotcmis.cache.pathtoid.size|CacheSizePathToId|Path-to-id cache size|number of path to object links|no|1000
org.apache.chemistry.dotcmis.cache.pathtoid.ttl|CacheTTLPathToId|Path-to-id cache time-to-live|time in milliseconds|no|1800000 (30 minutes)
org.apache.chemistry.dotcmis.cache.path.omit|CachePathOmit|Turn off path-to-id cache|"true", "false"|no|false
org.apache.chemistry.dotcmis.binding.cache.repositories.size|CacheSizeRepositories|Repository info cache size|number of objects|no|10
org.apache.chemistry.dotcmis.binding.cache.types.size|CacheSizeTypes|Type definition cache size|number of objects|no|100 
org.apache.chemistry.dotcmis.binding.cache.links.size|CacheSizeLinks|AtomPub link cache size|number of objects|no|400
org.apache.chemistry.dotcmis.objectfactory.classname|ObjectFactoryClass|Object factory implementation |class name|no|DotCMIS.Client.Impl.ObjectFactory