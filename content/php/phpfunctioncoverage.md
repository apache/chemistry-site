Title: PHPFunctionCoverage

# Table of Functions and Their Status (PHP Client)
<a name="PHPFunctionCoverage-TableofFunctionsandTheirStatus(PHPClient)"></a>

The Tables below list the methods on all of the services and whether or not
they are implemented via the API calls and their return values.

This CMIS client leverages the REST Binding and turns the ATOM-PUB
structures that are returned into PHP Data Structures that organize the
information in a way that matches the Domain Model of the CMIS Spec.

<a name="PHPFunctionCoverage-RepositoryServices"></a>
## Repository Services

<table>
<tr><th> Function </th><th> Status </th><th> Return Type </th></tr>
<tr><td> Get Repositories </td><td> <i>N/A</i> </td><td> <i>N/A<i/> </td></tr>
<tr><td> Get Repository Info </td><td> Yes </td><td> Repository Definition </td></tr>
<tr><td> Get Type Children </td><td> Yes </td><td> List of Types </td></tr>
<tr><td> Get Type Descendants </td><td> Yes </td><td> Tree of Types </td></tr>
<tr><td> Get Type Definition </td><td> Yes </td><td> Type Definition </td></tr>
</table>

<a name="PHPFunctionCoverage-NavigationServices"></a>
## Navigation Services

<table>
<tr><th> Function </th><th> Status </th><th> Return Type </th></tr>
<tr><td> Get Folder Tree </td><td> Yes </td><td> Tree of Folders </td></tr>
<tr><td> Get Descendants </td><td> Yes </td><td> Tree of Folders and Documents </td></tr>
<tr><td> Get Children </td><td> Yes </td><td> List of Objects </td></tr>
<tr><td> Get Folder Parent </td><td> Yes </td><td> Folder Object </td></tr>
<tr><td> Get Object Parents </td><td> Yes </td><td> List Folder Objects </td></tr>
<tr><td> Get Checkedout Docs </td><td> Yes </td><td> List of Document Objects </td></tr>
</table>

<a name="PHPFunctionCoverage-DiscoveryServices"></a>
## Discovery Services

<table>
<tr><th> Function </th><th> Status </th><th> Return Type </th></tr>
<tr><td> Query </td><td> Yes </td><td> List Folder Objects </td></tr>
<tr><td> Get Content Changes </td><td> No </td><td> ??? </td></tr>
</table>

<a name="PHPFunctionCoverage-ObjectServices"></a>
## Object Services

<table>
<tr><th> Function </th><th> Status </th><th> Return Type </th></tr>
<tr><td> Get Object </td><td> Yes </td><td> CMIS Object </td></tr>
<tr><td> Get Object By Path </td><td> Yes </td><td> CMIS Object </td></tr>
<tr><td> Get Properties </td><td> Yes </td><td> CMIS Object </td></tr>
<tr><td> Get Allowable Actions </td><td> No </td><td> ??? </td></tr>
<tr><td> Get Renditions </td><td> Yes </td><td> CMIS Object </td></tr>
<tr><td> Get Content Stream </td><td> Yes </td><td> Content Stream </td></tr>
<tr><td> Create Document </td><td> Yes </td><td> Object ID (CMIS Object Returned) </td></tr>
<tr><td> Create Document From Source </td><td> <i>N/A</i> </td><td> <i>N/A</i> </td></tr>
<tr><td> Create Folder </td><td> Yes </td><td> Object ID (CMIS Object Returned) </td></tr>
<tr><td> Create Relationship </td><td> No </td><td> Object ID (CMIS Object Returned) </td></tr>
<tr><td> Create Policy </td><td> No </td><td> Object ID (CMIS Object Returned) </td></tr>
<tr><td> Update Properties </td><td> Yes </td><td> Object ID+Change Token (CMIS Object Returned) </td></tr>
<tr><td> Move Object </td><td> Yes </td><td> Object Id (CMIS Object Returned) </td></tr>
<tr><td> Delete Object </td><td> Yes </td><td> None </td></tr>
<tr><td> Delete Tree </td><td> No </td><td> List of Object IDs (Objects that could not be deleted) (CMIS Objects Returned?) </td></tr>
<tr><td> Set Content Stream </td><td> Yes </td><td> Object ID+Change Token (CMIS Object Returned) </td></tr>
<tr><td> Delete Content Stream </td><td> Yes </td><td> Object ID+Change Token (CMIS Object Returned) </td></tr>
</table>

<a name="PHPFunctionCoverage-VersioningServices"></a>
# Versioning Services

<table>
<tr><th> Function </th><th> Status </th><th> Return Type </th></tr>
<tr><td> Check Out </td><td> No </td><td> ??? </td></tr>
<tr><td> Check In </td><td> No </td><td> ??? </td></tr>
<tr><td> Cancel Check Out </td><td> No </td><td> ??? </td></tr>
<tr><td> Get Properties Of Latest Version </td><td> <i>Incomplete - Do Not Use</i> </td><td> ??? </td></tr>
<tr><td> Get Object Of Latest Version </td><td> <i>Incomplete - Do Not Use</i> </td><td> ??? </td></tr>
<tr><td> Get All Versions </td><td> No </td><td> ??? </td></tr>
<tr><td> Delete All Versions </td><td> No </td><td> ??? </td></tr>
</table>

<a name="PHPFunctionCoverage-RelationshipServices"></a>
## Relationship Services

<table>
<tr><th> Function </th><th> Status </th><th> Return Type </th></tr>
<tr><td> Get Object Relationships </td><td> No </td><td> ??? </td></tr>
</table>

<a name="PHPFunctionCoverage-Multi-FilingServices"></a>
## Multi-Filing Services

<table>
<tr><th> Function </th><th> Status </th><th> Return Type </th></tr>
<tr><td> Add Object To Folder </td><td> No </td><td> ??? </td></tr>
<tr><td> Remove Object From Folder </td><td> No </td><td> ??? </td></tr>
</table>

<a name="PHPFunctionCoverage-PolicyServices"></a>
## Policy Services

<table>
<tr><th> Function </th><th> Status </th><th> Return Type </th></tr>
<tr><td> Apply Policy </td><td> No </td><td> ??? </td></tr>
<tr><td> Remove Policy </td><td> No </td><td> ??? </td></tr>
<tr><td> Get Applied Policies </td><td> No </td><td> ??? </td></tr>
</table>

<a name="PHPFunctionCoverage-ACLServices"></a>
## ACL Services

<table>
<tr><th> Function </th><th> Status </th><th> Return Type </th></tr>
<tr><td> Get ACL </td><td> No </td><td> ??? </td></tr>
<tr><td> Apply ACL </td><td> No </td><td> ??? </td></tr>
</table>

<a name="PHPFunctionCoverage-DocumentationonVariousReturnTypes"></a>
# Documentation on Various Return Types

<table>
<tr>
 <th> Return Type </th>
 <th> Atom Pub Type </th>
 <th> Description of PHP Structure </th>
 <th> Comments </th>
</tr>
<tr>
 <td> Repository Definition </td>
 <td> Workspace </td>
 <td> An object with 5 arrays
  <ol>
   <li> Links (used by the client to navigate the repository) </li>
   <li> URI Templates (used by the client to navigate the repository </li>
   <li> Collections (used by the client to navigate the repository) </li>
   <li> Capabilities </li>
   <li> Repository Information </li>
  </ol>
 </td>
 <td> </td>
</tr> 
<tr>  
 <td> CMIS Object </td>
 <td> Entry </td>
 <td> An object with 2 arrays and 2 scalars:
  <ol>
   <li> Links  (used by the client to navigate the repository) </li>
   <li> Properties </li>
   <li> UUID </li>
   <li> ID (Object ID) </li>
  </ol>
 </td>
 <td> CMIS Object can refer to:
  <ul>
   <li> Document </li>
   <li> Folder </li>
   <li> Policy </li>
   <li> Relationship </li>
   <li> Object ID </li>
   <li> Object ID+Change Token </li>
  </ul>
 </td>
</tr>
<tr>
 <td> List of CMIS Objects </td>
 <td> Feed </td>
 <td> PHP object with 2 arrays of Entry objects:
  <ul>
   <li> objectsById - an associative array of the Entries </li>
   <li> objectList - an array of references to the objets in the objectsById array </li>
  </ul>
 </td>
 <td> Objects in the feed may not be fully populated </td>
</tr>
<tr>
 <td> Tree of CMIS Objects </td>
 <td> Feed with CMIS Hierarchy Extensions </td>
 <td> Array similar to above.
    Hierarchy is achieved by adding a "children" object to each Entry that has children.
    The "Children" object contains the same structure as the Feed (2 arrays) </td>
 <td> Objects in the feed may not be fully populated </td>
</tr>
<tr>
 <td> Type Definition </td>
 <td> Entry </td>
 <td> An Object with 3 arrays and 1 scalar:
  <ol>
   <li>Links  (used by the client to navigate the repository)</li>
   <li>Properties</li>
   <li>Attributes</li>
   <li>ID (Object Type ID)</li>
  </ol>
 </td>
 <td>
  The Type Definition data structure needs work for completion.
  Currently it has enough to support the needs of the Object Services
 </td>
</tr>
<tr>
 <td> List of Type Definitions </td>
 <td> Feed with CMIS Hierarchy Extensions </td>
 <td> PHP object with 2 arrays of Entry objects:
  <ul>  
   <li> objectsById - an associative array of the Entries </li>
   <li> objectList - an array of references to the objets in the objectsById array </li>
  </ul>
 </td>
 <td> Objects in the feed may not be fully populated </td>
</tr>
<tr>
 <td> Tree of Type Definitions </td>
 <td> Feed with CMIS Hierarchy Extensions </td>
 <td> Array similar to above. Hierarchy is achieved by adding a "children" object to each Entry that has children.
      The "Children" object contains the same structure as the Feed (2 arrays) </td>
 <td> Objects in the feed may not be fully populated </td>
</tr>
<tr>
 <td> Content Stream </td>
 <td> Content </td>
 <td> Content </td>
 <td> </td>
</tr>
</table>
<br/>
