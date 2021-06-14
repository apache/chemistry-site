Title: URLs for AtomPub

# URLs for AtomPub
Often it is useful to understand the URL patterns that OpenCMIS uses in
the AtomPub binding. This can be helpful when analyzing log files or if
you want to debug sepcific requests.

##  Syntax

	URL ::= http://<HOST>:<PORT>/<SERVLET-PATH>/<REPOSITORY-ID>/<RESOURCE>?<PARAM=VALUE>&<PARAM=VALUE>&....

	RESOURCE ::= RESOURCE_CHILDREN |
		RESOURCE_CHILDREN |
		RESOURCE_DESCENDANTS |
		RESOURCE_FOLDERTREE |
		RESOURCE_TYPE |
		RESOURCE_TYPES |
		RESOURCE_TYPESDESC |
		RESOURCE_ENTRY |
		RESOURCE_PARENTS |
		RESOURCE_VERSIONS |
		RESOURCE_ALLOWABLEACIONS |
		RESOURCE_ACL |
		RESOURCE_POLICIES |
		RESOURCE_RELATIONSHIPS |
		RESOURCE_OBJECTBYID |
		RESOURCE_OBJECTBYPATH |
		RESOURCE_QUERY |
		RESOURCE_CHECKEDOUT |
		RESOURCE_UNFILED |
		RESOURCE_CHANGES |
		RESOURCE_CONTENT

	PARAM ::= PARAM_ACL |
		PARAM_ALLOWABLE_ACTIONS |
		PARAM_ALL_VERSIONS |
		PARAM_CHANGE_LOG_TOKEN |
		PARAM_CHANGE_TOKEN |
		PARAM_CHECKIN_COMMENT |
		PARAM_CHECK_IN |
		PARAM_CHILD_TYPES |
		PARAM_CONTINUE_ON_FAILURE |
		PARAM_DEPTH |
		PARAM_FILTER |
		PARAM_FOLDER_ID |
		PARAM_ID |
		PARAM_MAJOR |
		PARAM_MAX_ITEMS |
		PARAM_ONLY_BASIC_PERMISSIONS |
		PARAM_ORDER_BY |
		PARAM_OVERWRITE_FLAG |
		PARAM_PATH |
		PARAM_PATH_SEGMENT |
		PARAM_POLICY_ID |
		PARAM_POLICY_IDS |
		PARAM_PROPERTIES |
		PARAM_PROPERTY_DEFINITIONS |
		PARAM_RELATIONSHIPS |
		PARAM_RELATIONSHIP_DIRECTION |
		PARAM_RELATIVE_PATH_SEGMENT |
		PARAM_REMOVE_FROM |
		PARAM_RENDITION_FILTER |
		PARAM_REPOSITORY_ID |
		PARAM_RETURN_VERSION |
		PARAM_ROPERTY_DEFINITIONS |
		PARAM_SKIP_COUNT |
		PARAM_SOURCE_FOLDER_ID |
		PARAM_STREAM_ID |
		PARAM_SUB_RELATIONSHIP_TYPES |
		PARAM_TYPE_ID |
		PARAM_UNFILE_OBJECTS |
		PARAM_VERSIONIG_STATE |
		PARAM_Q |
		PARAM_SEARCH_ALL_VERSIONS |
		PARAM_ACL_PROPAGATION |



	RESOURCE_CHILDREN ::= "children";
	RESOURCE_DESCENDANTS ::= "descendants";
	RESOURCE_FOLDERTREE ::= "foldertree";
	RESOURCE_TYPE ::= "type";
	RESOURCE_TYPES ::= "types";
	RESOURCE_TYPESDESC ::= "typedesc";
	RESOURCE_ENTRY ::= "entry";
	RESOURCE_PARENTS ::= "parents";
	RESOURCE_VERSIONS ::= "versions";
	RESOURCE_ALLOWABLEACIONS ::= "allowableactions";
	RESOURCE_ACL ::= "acl";
	RESOURCE_POLICIES ::= "policies";
	RESOURCE_RELATIONSHIPS ::= "relationships";
	RESOURCE_OBJECTBYID ::= "id";
	RESOURCE_OBJECTBYPATH ::= "path";
	RESOURCE_QUERY ::= "query";
	RESOURCE_CHECKEDOUT ::= "checkedout";
	RESOURCE_UNFILED ::= "unfiled";
	RESOURCE_CHANGES ::= "changes";
	RESOURCE_CONTENT ::= "content";


		// parameter
	PARAM_ACL ::= "includeACL";
	PARAM_ALLOWABLE_ACTIONS ::= "includeAllowableActions";
	PARAM_ALL_VERSIONS ::= "allVersions";
	PARAM_CHANGE_LOG_TOKEN ::= "changeLogToken";
	PARAM_CHANGE_TOKEN ::= "changeToken";
	PARAM_CHECKIN_COMMENT ::= "checkinComment";
	PARAM_CHECK_IN ::= "checkin";
	PARAM_CHILD_TYPES ::= "childTypes";
	PARAM_CONTINUE_ON_FAILURE ::= "continueOnFailure";
	PARAM_DEPTH ::= "depth";
	PARAM_FILTER ::= "filter";
	PARAM_FOLDER_ID ::= "folderId";
	PARAM_ID ::= "id";
	PARAM_MAJOR ::= "major";
	PARAM_MAX_ITEMS ::= "maxItems";
	PARAM_ONLY_BASIC_PERMISSIONS ::= "onlyBasicPermissions";
	PARAM_ORDER_BY ::= "orderBy";
	PARAM_OVERWRITE_FLAG ::= "overwriteFlag";
	PARAM_PATH ::= "path";
	PARAM_PATH_SEGMENT ::= "includePathSegment";
	PARAM_POLICY_ID ::= "policyId";
	PARAM_POLICY_IDS ::= "includePolicyIds";
	PARAM_PROPERTIES ::= "includeProperties";
	PARAM_PROPERTY_DEFINITIONS ::= "includePropertyDefinitions";
	PARAM_RELATIONSHIPS ::= "includeRelationships";
	PARAM_RELATIONSHIP_DIRECTION ::= "relationshipDirection";
	PARAM_RELATIVE_PATH_SEGMENT ::= "includeRelativePathSegment";
	PARAM_REMOVE_FROM ::= "removeFrom";
	PARAM_RENDITION_FILTER ::= "renditionFilter";
	PARAM_REPOSITORY_ID ::= "repositoryId";
	PARAM_RETURN_VERSION ::= "returnVersion";
	PARAM_ROPERTY_DEFINITIONS ::= "includePropertyDefinitions";
	PARAM_SKIP_COUNT ::= "skipCount";
	PARAM_SOURCE_FOLDER_ID ::= "sourceFolderId";
	PARAM_STREAM_ID ::= "streamId";
	PARAM_SUB_RELATIONSHIP_TYPES ::= "includeSubRelationshipTypes";
	PARAM_TYPE_ID ::= "typeId";
	PARAM_UNFILE_OBJECTS ::= "unfileObjects";
	PARAM_VERSIONIG_STATE ::= "versioningState";
	PARAM_Q ::= "q";
	PARAM_SEARCH_ALL_VERSIONS ::= "searchAllVersions";
	PARAM_ACL_PROPAGATION ::= "ACLPropagation";

	HOST ::= <String>
	PORT ::= <Integer>
	SERVLET-PATH ::= <String>
	REPOSITORY-ID ::= <String>
	VALUE ::= <String>

## Examples:

getChildren feed of folder with id=2FF200 in repository A1
http://localhost:8080/opencmis/atom/A1/children?id=2FF2000

 


