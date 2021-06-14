Title: PowerShell Examples

# PowerShell Example

The DotCMIS DLL can be used in PowerShell scripts. Here is a simple example.


    # load DotCMIS DLL
    [Reflection.Assembly]::LoadFile("X:\path\to\DotCMIS.dll")
    
    
    # -----------------------------------------------------------------
    
    # helper functions
    function New-GenericDictionary([type] $keyType, [type] $valueType) {  
       $base = [System.Collections.Generic.Dictionary``2]  
       $ct = $base.MakeGenericType(($keyType, $valueType))  
       New-Object $ct
    }
    
    function New-ContentStream([string] $file, [string] $mimetype) {
       $fileinfo = ([System.IO.FileInfo]$file)
       
       $contentStream = New-Object "DotCMIS.Data.Impl.ContentStream"
       $contentStream.Filename = $fileinfo.Name
       $contentStream.Length = $fileinfo.Length
       $contentStream.MimeType = $mimetype
       $contentStream.Stream = $fileinfo.OpenRead()
       
       $contentStream
    }
    
    function Download-ContentStream([DotCMIS.Client.IDocument] $document, [string] $file) {
       $contentStream = $document.GetContentStream()   
       $fileStream = [System.IO.File]::OpenWrite($file)
    
       $buffer = New-Object byte[] 4096  
       do {  
          $b = $contentStream.Stream.Read($buffer, 0, 4096)  
          $fileStream.Write($buffer, 0, $b)  
       }  
       while ($b -ne 0)
       
       $fileStream.Close()
       $contentStream.Stream.Close()
    }
    
    
    # -----------------------------------------------------------------
    
    # create session
    $sp = New-GenericDictionary string string
    $sp["org.apache.chemistry.dotcmis.binding.spi.type"] = "atompub"
    $sp["org.apache.chemistry.dotcmis.binding.atompub.url"] = "http://localhost:8080/alfresco/service/cmis"
    $sp["org.apache.chemistry.dotcmis.user"] = "admin"
    $sp["org.apache.chemistry.dotcmis.password"] = "admin"
    
    $factory = [DotCMIS.Client.Impl.SessionFactory]::NewInstance()
    $session = $factory.GetRepositories($sp)[0].CreateSession()
    
    
    # print the repository infos
    $session.RepositoryInfo.Id
    $session.RepositoryInfo.Name
    $session.RepositoryInfo.Vendor
    $session.RepositoryInfo.ProductName
    $session.RepositoryInfo.ProductVersion
    
    
    # get root folder
    $root = $session.GetRootFolder()
    
    
    # print root folder children
    $children = $root.GetChildren()
    foreach ($object in $children) {
       $object.Name + "     (" + $object.ObjectType.Id + ")" 
    }
    
    
    # run a quick query
    $queryresult = $session.Query("SELECT * FROM cmis:document", $false)
    foreach ($object in $queryresult) {
       foreach ($item in $object.Properties) {
          $item.QueryName + ": " + $item.FirstValue
       }
       "----------------------------------"
    }
    
    
    # create a folder
    $folderProperties = New-GenericDictionary string object
    $folderProperties["cmis:name"] = "myNewFolder"
    $folderProperties["cmis:objectTypeId"] = "cmis:folder"
    
    $folder = $root.CreateFolder($folderProperties)
    
    
    # create a document 
    $documentProperties = New-GenericDictionary string object
    $documentProperties["cmis:name"] = "myNewDocument"
    $documentProperties["cmis:objectTypeId"] = "cmis:document"
    
    $source = $home + "\source.txt"
    $mimetype = "text/plain"
    $contentStream = New-ContentStream $source $mimetype
    
    $doc = $folder.CreateDocument($documentProperties, $contentStream, $null)
    
    
    # download a document
    $target = $home + "\target.txt"
    Download-ContentStream $doc $target
    
    
    # clean up
    $doc.Delete($true)
    $folder.Delete($true)
