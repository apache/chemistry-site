Title: OpenCMIS Fileshare Repository

# OpenCMIS Fileshare Repository

**This repository is not intended for production use!**

The FileShare repository uses the file system as its back-end store and provides read/write access to content and metadata. In other words, it puts a CMIS interface on top of your file system.

The repository is restricted by the capabilities of the file system and therefore does not support relationships, policies, document versions, multi-filing, un-filing and query.

By default it provides a repository "test" that uses your home directory as the repository root. It requires authentication. A user "test" with the password "test" is pre-configured.

1. Follow this guide: [Build OpenCMIS](../../how-to/how-to-build.html).
1. A ready-to-use WAR file should now exist in `/chemistry-opencmis-server/chemistry-opencmis-server-fileshare/target`.
1. Deploy the WAR file to your favorite servlet engine.
1. AtomPub endpoint: `http://<host>:<port>/<context>/atom` <br/>
   Web Services endpoint: `http://<host>:<port>/<context>/services/RepositoryService`

## Download the FileShare Repository

You can download the latest release from the [download page](/java/download.html).

## Configure the Repository

The configuration file in the WAR file is
`/WEB-INF/classes/repository.properties`.

```properties
    # Don't touch this line
    class=org.apache.chemistry.opencmis.fileshare.FileShareServiceFactory
    
    # Login configuration
    #  login.<no> = <user>:<password>
    login.1 = test:test
    login.2 = cmisuser:password
    login.3 = reader:reader
    
    # Type defintions (see example-type.xml)
    #  type.<no> = <absolute path to type definition XML file>
    type.1 = /home/cmistest/type1.xml
    type.2 = /home/cmistest/type2.xml
    
    # Repository configuration
    #  repository.<repositoryId> = <absolute path to repository root folder>
    #  repository.<repositoryId>.readwrite = <comma separated list of login names>
    #  repository.<repositoryId>.readonly = <comma separated list of login names>
    repository.test = /home/cmistest/myreproot 
    repository.test.readwrite = test, cmisuser
    repository.test.readonly = reader
```
