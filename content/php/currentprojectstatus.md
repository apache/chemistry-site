Title: CurrentProjectStatus

# Introduction
<a name="CurrentProjectStatus-Introduction"></a>

_This page needs to be updated/reworked.  For now look to the bottom for latest update._

I have taken a snapshot of the source as of Dec 1 and zipped it up for download.
This will enable me to start making changes.


<a name="CurrentProjectStatus-Details"></a>
## Details

Am now adding CMISService which will map more closely to the Services in the Domain model.
This is a work in progress.
Checked in code will usually be able to run without totally breaking.

Zipped downloads and Labeled revisions should generally run without breaking.

<a name="CurrentProjectStatus-Update2010-02-15"></a>
## Update 2010-02-15

<a name="CurrentProjectStatus-Addednewupdatewithsometestcode."></a>
**Added new update with some test code.**

Filled out quite a bit of the domain level routines, including:

  * getRepositories
  * getTypeDefinition
  * getChildren
  * getFolderParent
  * getObjectParents
  * getCheckedOutDocs
  * getObject
  * getObjectByPath
  * getProperties
  * getContentStream
  * createDocument
  * createFolder
  * updateProperties
  * moveObject
  * deleteObject
  * setContentStream
  * deleteContentStream

Many of these have been tested but not all -- some known issues:

  * Not handling optional inputs (filters etc....)
  * Some assumptions are being made regarding the form of URLs
  * May be making some assumptions about XML format w.r.t. Name spacing
  * Has only been tested against the Alfresco Repository


<a name="CurrentProjectStatus-Update2010-09-07"></a>
## Update 2010-09-07

<br/>

  * Fixed problem with handling of spaces -- no need to urlencode inputs
  * Added new functionality:
    * Get Type Children 
    * Get Type Descendants
    * Get Folder Tree
    * Get Descendants
  * Updated Documentation


## Update 2013-11-17

<br />

   * Added archive assets for easy deployment
   * Added a number of patches in preparation for a formal release
