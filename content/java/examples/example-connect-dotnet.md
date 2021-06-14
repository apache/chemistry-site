Title: Connecting from a .NET client

# Connecting from a .NET client via the Web Services binding
    
This is a very simple C# example that demonstrates how to connect to an OpenCMIS server via the Web Services binding. Please note that .NET only allows UsernameTokens over HTTPS.
(See also [DotCMIS](../../dotnet/dotcmis.html)).

```C#
    using System;
    using System.ServiceModel;
    using OpenCMISClient.OpenCMISServer;
    using System.Net;

    namespace OpenCMISClient
    {
        class CMISClientDemo
        {
            public void DoStuff()
            {
	        try
                {
                    // uncomment the next line if you are using a self signed SSL certificate 
                    // ServicePointManager.ServerCertificateValidationCallback = delegate { return true; };

                    // get hold of the services
                    RepositoryServicePortClient repService = GetRepositoryService("https://localhost:8443/opencmis/services/RepositoryService?wsdl", "test", "test");
                    NavigationServicePortClient navService = GetNavigationService("https://localhost:8443/opencmis/services/NavigationService?wsdl", "test", "test");
                    ObjectServicePortClient objService = GetObjectService("https://localhost:8443/opencmis/services/ObjectService?wsdl", "test", "test");
                    
                    // get the list of repositories
                    cmisRepositoryEntryType[](.html) repositoryEntries = repService.getRepositories(null);
                    foreach (cmisRepositoryEntryType repositoryEntry in repositoryEntries)
                    {
                        Console.WriteLine("Repository: " + repositoryEntry.repositoryName + " (" + repositoryEntry.repositoryId + ")");
                        
                        // get repository info
                        cmisRepositoryInfoType repositoryInfo = repService.getRepositoryInfo(repositoryEntry.repositoryId, null);
                        Console.WriteLine("  Info:");
                        Console.WriteLine("    Description: " + repositoryInfo.repositoryDescription);
                        Console.WriteLine("    Product:	" + repositoryInfo.vendorName + " / " + repositoryInfo.productName + " " + repositoryInfo.productVersion);
                        
                        // get all base types of the repository
                        cmisTypeDefinitionListType typeList = repService.getTypeChildren(repositoryInfo.repositoryId, null, true, null, null, null);
                        Console.WriteLine("  Types:");
                        foreach (cmisTypeDefinitionType type in typeList.types)
                        {
                            Console.WriteLine("    " + type.displayName + " (" + type.id + ")");
                        }

                        // get all root folder children
                        cmisObjectInFolderListType children = navService.getChildren(repositoryInfo.repositoryId, repositoryInfo.rootFolderId, null, null, true, null, null, false, null, null, null);
                        Console.WriteLine("  Root folder:");
                        foreach (cmisObjectInFolderType objInFolder in children.objects)
                        {
                            cmisObjectType obj = objInFolder.@object;
                            String objId = GetIdPropertyValue(obj.properties, "cmis:objectId");
                            Console.WriteLine("    Name: " + GetStringPropertyValue(obj.properties, "cmis:name") + " (" + objId + ")");
                            Console.WriteLine("	 Type:		" + GetIdPropertyValue(obj.properties, "cmis:baseTypeId"));
                            Console.WriteLine("	 Created by:	" + GetStringPropertyValue(obj.properties, "cmis:createdBy"));
                            Console.WriteLine("	 Creation date: " + GetDateTimePropertyValue(obj.properties, "cmis:creationDate"));

                            // if it is a document, get the size and the content
                            String baseType = GetIdPropertyValue(obj.properties, "cmis:baseTypeId");
                            if ("cmis:document".Equals(baseType))
                            {
                                // get the size
                                Int64? size = GetIntegerPropertyValue(obj.properties, "cmis:contentStreamLength");
                                Console.WriteLine("      Size:	    " + size);
                                
                                // get the content
                                cmisContentStreamType content = objService.getContentStream(repositoryInfo.repositoryId, objId, null, null, null, null);
                                Console.WriteLine("      MIME type:     " + content.mimeType);
                                
                                // get the "stream"
                                byte[]bytes = content.stream; // really streaming requires some more work
                                Console.WriteLine("      Stream:	    " + (bytes.Length == size ? "ok" : "mismatch"));
                            }
                        }

                    }
                }
                catch (FaultException<cmisFaultType> fe)
                {
                    Console.WriteLine("CMIS Exception: " + fe.Detail.message);
                    Console.WriteLine("Type: " + fe.Detail.type);
                    Console.WriteLine("Code: " + fe.Detail.code);
                }
                catch (Exception e)
                {
                    Console.WriteLine("Exception: " + e.Message);
                    Console.WriteLine(e.StackTrace);
                }
                
                Console.ReadKey();
            }

            public RepositoryServicePortClient GetRepositoryService(String wsdlUrl, String user, String password)
            {
                BasicHttpBinding binding = new BasicHttpBinding();
                binding.MessageEncoding = WSMessageEncoding.Mtom;
                binding.Security.Mode = BasicHttpSecurityMode.TransportWithMessageCredential;
                
                RepositoryServicePortClient service = new RepositoryServicePortClient(binding, new EndpointAddress(wsdlUrl));
                
                service.ClientCredentials.UserName.UserName = user;
                service.ClientCredentials.UserName.Password = password;
                
                return service;
            }

            public NavigationServicePortClient GetNavigationService(String wsdlUrl, String user, String password)
            {
                BasicHttpBinding binding = new BasicHttpBinding();
                binding.MessageEncoding = WSMessageEncoding.Mtom;
                binding.Security.Mode = BasicHttpSecurityMode.TransportWithMessageCredential;
                
                NavigationServicePortClient service = new
                NavigationServicePortClient(binding, new EndpointAddress(wsdlUrl));
                
                service.ClientCredentials.UserName.UserName = user;
                service.ClientCredentials.UserName.Password = password;
                
                return service;
            }
                
            public ObjectServicePortClient GetObjectService(String wsdlUrl, String user, String password)
            {
                BasicHttpBinding binding = new BasicHttpBinding();
                binding.MessageEncoding = WSMessageEncoding.Mtom;
                binding.Security.Mode = BasicHttpSecurityMode.TransportWithMessageCredential;
                binding.TransferMode = TransferMode.Streamed;
                
                ObjectServicePortClient service = new ObjectServicePortClient(binding, new EndpointAddress(wsdlUrl));
                
                service.ClientCredentials.UserName.UserName = user;
                service.ClientCredentials.UserName.Password = password;
                
                return service;
            }
                
            public String GetStringPropertyValue(cmisPropertiesType properties, String id)
            {
                String result = null;
                
                foreach (cmisProperty property in properties.Items)
                {
                    if (property.propertyDefinitionId.Equals(id))
                    {
                        if (property is cmisPropertyString)
                        {
                            result = ((cmisPropertyString)property).value[0](0.html);
                        }
                        break;
                    }
                }
                
                return result;
            }
                
            public String GetIdPropertyValue(cmisPropertiesType properties, String id)
            {
                String result = null;
                
                foreach (cmisProperty property in properties.Items)
                {
                    if (property.propertyDefinitionId.Equals(id))
                    {
                        if (property is cmisPropertyId)
                        {
                            result = ((cmisPropertyId)property).value[0](0.html);
                        }
                        break;
                    }
                }
                
                return result;
            }
                
            public Int64? GetIntegerPropertyValue(cmisPropertiesType properties, String id)
            {
                Int64? result = null;
                
                foreach (cmisProperty property in properties.Items)
                {
                    if (property.propertyDefinitionId.Equals(id))
                    {
                        if (property is cmisPropertyInteger)
                        {
                            result = Int64.Parse(((cmisPropertyInteger)property).value[0](0.html));
                        }
                        break;
                    }
                }

                return result;
            }

            public DateTime? GetDateTimePropertyValue(cmisPropertiesType properties, String id)
            {
                DateTime? result = null;
                
                foreach (cmisProperty property in properties.Items)
                {
                    if (property.propertyDefinitionId.Equals(id))
                    {
                        if (property is cmisPropertyDateTime)
                        {
                            result = ((cmisPropertyDateTime)property).value[0](0.html);
                        }
                        break;
                    }
                }
                
                return result;
            }
        }
    }
```