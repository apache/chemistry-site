Title: TestSuiteDescription

# Introduction
<a name="TestSuiteDescription-Introduction"></a>

The Test Suite tests some of the basic CMIS functionality.  This will be
used to validate this client library against a wide variety of CMIS
compliant repositories.


<a name="TestSuiteDescription-Details"></a>
## Details

This Test Suite tests the following functionality:

* Getting the Type information of an object
* Creating a folder
* Creating a document
* Listing the files and folders in a folder
* Getting the contents of a created document
* Moving a Document
* Deleting a document


The user must provide the following information:

* The URL of the server
* The Authentication credentials
* The name of an existing folder
* The name of a new folder that will be created as part of the tests
* A flag that tells how much debug information to print out

Currently the test suite does not have an "expects" capability allowing it
the validate the test results.	That will be added as one of the TODOs.

<a name="TestSuiteDescription-RunningtheTestSuite"></a>
## Running the Test Suite

`php -f cmis_test_suite.php` _url_to_cmis_compliant_repository_ _username_
_password_ _existing_folder_ _new_folder_name_ _debug_flag_


* _url_to_cmis_compliant_repository_ : The URL For the ATOM-PUB/REST
binding of a CMIS compliant repository
* _username_ : username
* _password_ : password
* _existing_folder_ : An existing folder in the repository.  A folder will
be created in this folder to contain any documents created as part of this
test
* _new_folder_name_
* _debug_flag_ : if non-blank and non-zero, extra debug information will
be printed

Currently the output is a bit of a fire hose.  This will be cleaned up.
