Title: CMIS Workbench

# CMIS Workbench

CMIS Workbench is CMIS desktop client for developers. It's a repository
browser and an interactive testbed for the OpenCMIS client API.

<a name="CMISWorkbench-BuildtheCMISWorkbench"></a>
## Build the CMIS Workbench

1. [Build the OpenCMIS Client Libraries](../../how-to/how-to-build.html).
1. CMIS Workbench zip and tar.gz files should now exist in `/target`.
1. Unpack one of them into an empty directory.
1. Run `workbench.sh` (UNIX) or `workbench.bat` (Windows)


<a name="CMISWorkbench-DownloadtheCMISWorkbench"></a>
## Download the CMIS Workbench

You can download the latest release from the [download page](/java/download.html).



## CMIS Workbench Introduction video

<iframe width="560" height="349" src="https://www.youtube.com/embed/akvCDVh03qs?rel=0" frameborder="0" allowfullscreen></iframe>


<a name="CMISWorkbench-PropertiesReference"></a>
## Properties Reference

The CMIS Workbench can be configured through system properties or
additional properties in the expert login dialog.

<a name="CMISWorkbench-Logindialog"></a>
### Login dialog

System Property               | Function
------------------------------|---------------------------------------------------
cmis.workbench.url            | preset URL
cmis.workbench.user           | preset user name
cmis.workbench.password       | preset password
cmis.workbench.binding        | preset binding (atompub/webservices)
cmis.workbench.authentication | preset authentication method (none/standard/ntlm)
cmis.workbench.compression    | preset compression (on/off)

<a name="CMISWorkbench-Folderoperationcontext"></a>
### Folder operation context

System Property                                | Function
-----------------------------------------------|-----------
cmis.workbench.folder.filter                   | property filter (comma separated list of query names)
cmis.workbench.folder.includeAcls              | fetch ACLs (true/false)
cmis.workbench.folder.includeAllowableActions  | fetch allowable actions (true/false)
cmis.workbench.folder.includePolicies          | fetch polices (true/false)
cmis.workbench.folder.includeRelationships     | fetch relationships (true/false)
cmis.workbench.folder.renditionFilter          | rendition filter (comma separated list of rendition kinds and MIME types)
cmis.workbench.folder.orderBy                  |  order of the children (comma separated list of query names)
cmis.workbench.folder.maxItemsPerPage          | maximum number of children that should be fetched in one call



<a name="CMISWorkbench-Objectoperationcontext"></a>
### Object operation context

System Property                                | Function
-----------------------------------------------|-----------
cmis.workbench.object.filter                   |  property filter (comma separated list of query names)
cmis.workbench.object.includeAcls              | fetch ACLs (true/false)
cmis.workbench.object.includeAllowableActions  | fetch allowable actions (true/false)
cmis.workbench.object.includePolicies          | fetch polices (true/false)
cmis.workbench.object.includeRelationships     | fetch relationships (true/false)
cmis.workbench.object.renditionFilter          | rendition filter (comma separated list of rendition kinds and MIME types)


<a name="CMISWorkbench-Others"></a>
### Others

System Property                             | Function
--------------------------------------------|-------------------------------------------
cmis.workbench.acceptSelfSignedCertificates | disable SSL certificate check (true/false)
