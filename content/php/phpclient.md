Title: phpclient

# Apache Chemistry CMIS PHP Client
<a name="phpclient-CMISPHPClient"></a>
Apache Chemistry CMIS PHP Client is a [CMIS](http://docs.oasis-open.org/cmis/CMIS/v1.0/cmis-spec-v1.0.html) client library for PHP.


This code is available at: [https://svn.apache.org/repos/asf/chemistry/phpclient/](https://svn.apache.org/repos/asf/chemistry/phpclient/).

To Run this example execute the following:

    php -f cmis_ls.php <rest-endpoint> <username> <password> <folderpath> [debug option 1](2.html)

Notes:

* The if the folder path has spaces in it you must URL encode the folder
path i.e. /Data Dictionary \--> /Data+Dictionary
* The debug option can be omitted. If it is 1 the program will display all
of the arrays associated with the objects that
* are returned. If the debug option is 2 or more, then the XML returned
will also be displayed
* This will not work on Pre CMIS-1.0 repositories
* There is virtually no error checking.
* Your version of php must support DOMDocument and curl


Example Runs:

    php -f cmis_ls.php http://cmis.alfresco.com/service/api/cmis admin admin /
    php -f cmis_ls.php http://cmis.alfresco.com/service/api/cmis admin admin /Data+Dictionary 1

The `cmis_repository_wrapper.php` library provided the following functionality:

* Encapsulates access to a CMIS 1.0 compliant repository
* Provides utilities for getting information out of:
* Workspace (the repositoryInfo)
* Object Entries
* Non-Hierarchical Object feeds

More information can be found on the following pages:

* [Current Project Status](currentprojectstatus.html)
* [Function Coverage](phpfunctioncoverage.html)
* [Test Suite Description](testsuitedescription.html)
* [API Docs](docs/atom/index.html)
