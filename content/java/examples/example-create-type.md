Title:     Create Type

# Creating a type

Java command-line example using the CMIS 1.1 type mutability feature.

```java
    package typeMutability.enabled;
    
    import java.io.FileInputStream;
    import java.io.FileNotFoundException;
    import java.io.IOException;
    import java.math.BigInteger;
    import java.util.ArrayList;
    import java.util.HashMap;
    import java.util.LinkedHashMap;
    import java.util.LinkedList;
    import java.util.List;
    import java.util.Map;
    import java.util.Map.Entry;
    import java.util.Properties;
    
    import org.apache.chemistry.opencmis.client.api.ObjectType;
    import org.apache.chemistry.opencmis.client.api.Repository;
    import org.apache.chemistry.opencmis.client.api.Session;
    import org.apache.chemistry.opencmis.client.api.SessionFactory;
    import org.apache.chemistry.opencmis.client.runtime.SessionFactoryImpl;
    import org.apache.chemistry.opencmis.commons.SessionParameter;
    import org.apache.chemistry.opencmis.commons.data.CmisExtensionElement;
    import org.apache.chemistry.opencmis.commons.data.RepositoryInfo;
    import org.apache.chemistry.opencmis.commons.definitions.Choice;
    import org.apache.chemistry.opencmis.commons.definitions.DocumentTypeDefinition;
    import org.apache.chemistry.opencmis.commons.definitions.PropertyBooleanDefinition;
    import org.apache.chemistry.opencmis.commons.definitions.PropertyDefinition;
    import org.apache.chemistry.opencmis.commons.definitions.PropertyIdDefinition;
    import org.apache.chemistry.opencmis.commons.definitions.PropertyIntegerDefinition;
    import org.apache.chemistry.opencmis.commons.definitions.PropertyStringDefinition;
    import org.apache.chemistry.opencmis.commons.definitions.TypeDefinition;
    import org.apache.chemistry.opencmis.commons.definitions.TypeMutability;
    import org.apache.chemistry.opencmis.commons.enums.BaseTypeId;
    import org.apache.chemistry.opencmis.commons.enums.Cardinality;
    import org.apache.chemistry.opencmis.commons.enums.ContentStreamAllowed;
    import org.apache.chemistry.opencmis.commons.enums.PropertyType;
    import org.apache.chemistry.opencmis.commons.enums.Updatability;
    import org.apache.chemistry.opencmis.commons.exceptions.CmisRuntimeException;
    
    import typeMutability.util.TestStringChoice;
    
    /* 
     * Licensed under the Apache License, Version 2.0 (the "License");
     * you may not use this file except in compliance with the License.
     * You may obtain a copy of the License at
     * 
     *   http://www.apache.org/licenses/LICENSE-2.0
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     *
     *
     * This code is based on the Apache Chemistry OpenCMIS FileShare project
     * <http://chemistry.apache.org/java/developing/repositories/dev-repositories-fileshare.html>.
     *
     * This code is part of a training exercise and not intended for production use!
     *
     */
    
    /**
     * Running this test requires an input file containing the connection 
     * properties for your CMIS server. 
     *
     * The default/static location is /temp/cmis.properties
     * and is hard coded in the main() method. 
     * See the loadConnectionProperties() call if you 
     * need to change this location or make it dynamic. 
     * 
     * Example format for a typical cmis.properties file:
     * You can also refer to the properties section in the 
     * Workbench page here:
     * http://chemistry.apache.org/java/developing/tools/dev-tools-workbench.html
     * 
     * ******************************************
     * SAMPLE cmis.properties *******************
     * ******************************************
     * 
     *     org.apache.chemistry.opencmis.binding.spi.type=browser
     *     org.apache.chemistry.opencmis
     *     org.apache.chemistry.opencmis.binding.browser.url=http://localhost:8080/openfncmis/browser
     *     org.apache.chemistry.opencmis.user=username_here
     *     org.apache.chemistry.opencmis.password=password_here
     *     org.apache.chemistry.opencmis.binding.compression=true
     *     org.apache.chemistry.opencmis.binding.cookies=true
     * 
     */
    
    /*
     * This class serves as a simple example of how to create a new TypeDefinintion
     * in CMIS using the OpenCMIS client. (all in one self contained file) This code
     * was tested with the FileNet CMIS 1.1 tech preview but can be modified to work
     * with any CMIS client that supports type mutability.
     * 
     * Code will create a new subclass of cmis:document with 4 properties 
     * String, Int, Boolean and Id, and two choice list values on the String prop
     */
    public class CreateTestCommandLine {
    
        // Modify these values to conform to your local repository requirements
        //
        // property id's and names here 
        private static final String property_stringPropertyId = "StringPropertyDefinition";
        private static final String property_intPropertyId = "IntPropertyDefinition";
        private static final String property_boolPropertyId = "BoolPropertyDefinition";
        private static final String property_idPropertyId = "IdPropertyDefinition";
    
        //
        // type related
        // the id of the new type's parent
        private static final String type_idOfParentType = "cmis:document";
        // Id of the new subclass we will be creating
        private static final String type_idOfNewClass = "cmis_newDocSubclass1";
        // other information about type
        private static final String type_description = "Test document type definition";
        private static final String type_displayName = "TestTypeDefinition";
        private static final String type_localName = "some test local name";
        private static final String type_localNamespace = "some test local name space";
    
        // globals
        private static Session session = null;
    
        public static void main(String[] args) {
    
            System.out.println("An example of type creation with CMIS TypeMutability from OpenCMIS.");
    
            // Assume /cmis.properties is the name of the input file containing all
            // of the session parameters (test only)
            Map<String, String> parameters = loadConnectionProperties("/temp/cmis.properties");
    
            session = getSession(parameters);
    
            // Look at repository info - demonstrates a valid connection
            RepositoryInfo repositoryInfo = session.getRepositoryInfo();
            System.out.println("Connected to repository.  Supports CMIS version:"
                    + repositoryInfo.getCmisVersionSupported());
    
            
            // Check here to verify the types we will be creating are permissible 
            // for this repository.
            Boolean canCreateStringProperty = false;
            Boolean canCreateIdProperty = false;
            Boolean canCreateBoolProperty = false;
            Boolean canCreateIntProperty = false;
            for (PropertyType propCreatable : repositoryInfo.getCapabilities().getCreatablePropertyTypes().canCreate()) {
    
                if (propCreatable.equals(PropertyType.STRING)) {
                    canCreateStringProperty = true;
                } else if (propCreatable.equals(PropertyType.INTEGER)) {
                    canCreateIntProperty = true;
                } else if (propCreatable.equals(PropertyType.ID)) {
                    canCreateIdProperty = true;
                } else if (propCreatable.equals(PropertyType.BOOLEAN)) {
                    canCreateBoolProperty = true;
                }
               
                System.out.println("Repository can create property of : " + propCreatable.toString());
            }
           
            assert canCreateStringProperty: "String is not one of the createable properties.";
            assert canCreateIdProperty: "Id is not one of the createable properties.";
            assert canCreateBoolProperty: "Boolean is not one of the createable properties.";
            assert canCreateIntProperty: "Integer is not one of the createable properties.";
    
            // Create new type with string property
            // and verify it exists in type collection
            ObjectType newType = createNewType();
    
            // clean up the type if permitted
            // TODO - check to see if delete is permitted first...
            session.deleteType(newType.getId());
    
            // TODO - verify delete here for completeness.
            System.out.println("Cleanup completed.");
        }
    
        /**
         * Create the new type with with our 4 test property types.
         * 
         * @return
         */
        public static ObjectType createNewType() {
            // assuming the following default (change if your repository
            // does not support these settings
            Boolean isCreatable = true;
            Boolean includedInSupertypeQuery = true;
            Boolean queryable = true;
            ContentStreamAllowed contentStreamAllowed = ContentStreamAllowed.ALLOWED;
            Boolean versionable = false;
    
            // build property definitions - string, int, boolean and id
            Map<String, PropertyDefinition<?>> propertyDefinitions = new LinkedHashMap<String, PropertyDefinition<?>>();
            TestStringPropertyDefinition spd = createStringPropertyDefinition();
            TestIntegerPropertyDefinition ipd = createIntPropertyDefinition();
            TestBooleanPropertyDefinition bpd = createBooleanPropertyDefinition();
            TestIdPropertyDefinition idpd = createIDPropertyDefinition();
    
            propertyDefinitions.put(spd.getId(), spd);
            propertyDefinitions.put(ipd.getId(), ipd);
            propertyDefinitions.put(bpd.getId(), bpd);
            propertyDefinitions.put(idpd.getId(), idpd);
    
            TestDocumentTypeDefinition typeToCreate = new TestDocumentTypeDefinition(type_idOfNewClass, type_description,
                    type_displayName, type_localName, type_localNamespace, type_idOfParentType, isCreatable,
                    includedInSupertypeQuery, queryable, contentStreamAllowed, versionable, propertyDefinitions);
    
            TypeDefinition createdType = null;
            try {
                createdType = session.createType(typeToCreate);
                System.out.println("Type created: " + createdType.toString());
            } catch (Exception e) {
                assert false: "An exception was thrown when trying to create a new type definition. Message: " + e.getMessage();
            }
    
            ObjectType retrievedType = null;
            try {
                retrievedType = session.getTypeDefinition(createdType.getId());
                assert retrievedType != null: "Unable to retrieve new type. ";
            } catch (Exception e) {
                assert false: "Got exception. Cannot get the type definition from the repository. Message: " + e.getMessage();
            }
    
            return retrievedType;
        }
    
        public static Session getSession(Map<String, String> parameters) {
            Session session = null;
    
            SessionFactory factory = SessionFactoryImpl.newInstance();
            if (parameters.containsKey(SessionParameter.REPOSITORY_ID)) {
                session = factory.createSession(parameters);
            } else {
                // Create session for the first repository.
                List<Repository> repositories = factory.getRepositories(parameters);
                session = repositories.get(0).createSession();
            }
    
            // reset op context to default
            session.setDefaultContext(session.createOperationContext());
    
            return session;
        }
    
        /*
         * Load our connection properties from a local file
         * 
         * Note file should use the same format as the expert settings for Workbench
         */
        public static final Map<String, String> loadConnectionProperties(String configResource) {
            Properties testConfig = new Properties();
            if (configResource == null) {
                throw new CmisRuntimeException("Filename with connection parameters was not supplied.");
            }
    
            FileInputStream inStream;
            try {
                inStream = new FileInputStream(configResource);
                testConfig.load(inStream);
                inStream.close();
            } catch (FileNotFoundException e1) {
                throw new CmisRuntimeException("Test properties file '" + configResource + "' was not found at:"
                        + configResource);
            } catch (IOException e) {
                throw new CmisRuntimeException("Exception loading test properties file " + configResource, e);
            }
    
            Map<String, String> map = new HashMap<String, String>();
            for (Entry<?, ?> entry : testConfig.entrySet()) {
                System.out.println("Found key: " + entry.getKey() + " Value:" + entry.getValue());
                map.put((String) entry.getKey(), ((String) entry.getValue()).trim());
            }
            return map;
        }
    
        /**
         * Create a single string property definition with a choice list
         */
        private static TestStringPropertyDefinition createStringPropertyDefinition() {
        
            Cardinality cardinality = Cardinality.SINGLE;
            String description = "String property definition";
            String displayName = "StringPropertyDefinition";
            String localName = "StringPropertyDefinition";
            String localNameSpace = "StringPropertyDefinition";    
            Updatability updatability = Updatability.READWRITE;
            Boolean orderable = false;
            Boolean queryable = false;
            ArrayList<String> defaults = new ArrayList<String>();
            defaults.add("test");
    
            List<String> vals1 = new LinkedList<String>();
            vals1.add("val1");
    
            List<String> vals2 = new LinkedList<String>();
            vals2.add("val2");
    
            TestStringChoice strChoice1 = new TestStringChoice("choice1", vals1, null);
            TestStringChoice strChoice2 = new TestStringChoice("choice2", vals2, null);
            List<Choice<String>> choiceList = new LinkedList<Choice<String>>();
            choiceList.add (strChoice1);
            choiceList.add (strChoice2);
    
            TestStringPropertyDefinition spd = new TestStringPropertyDefinition(property_stringPropertyId, cardinality, 
                    description, displayName, localName, localNameSpace, updatability, orderable, queryable, 
                    defaults, choiceList, null);
    
            return spd;      
        }
    
        private static TestIntegerPropertyDefinition createIntPropertyDefinition() {
            Cardinality cardinality = Cardinality.MULTI;
            String description = "Int property definition";
            String displayName = "IntPropertyDefinition";
            String localName = "IntPropertyDefinition";
            String localNameSpace = "IntPropertyDefinition";
            Updatability updatability = Updatability.READWRITE;
            Boolean orderable = false;
            Boolean queryable = false;
            ArrayList<BigInteger> defaults = new ArrayList<BigInteger>();
            // defaults.add(new BigInteger("101"));
            BigInteger minVal = new BigInteger("100");
            BigInteger maxVal = new BigInteger("1000");
    
            TestIntegerPropertyDefinition ipd = new TestIntegerPropertyDefinition(property_intPropertyId, cardinality,
                    description, displayName, localName, localNameSpace, updatability, orderable, queryable, defaults,
                    minVal, maxVal, null);
            return ipd;
        }
    
        private static TestBooleanPropertyDefinition createBooleanPropertyDefinition() {
    
            Cardinality cardinality = Cardinality.SINGLE;
            String description = "Boolean property definition";
            String displayName = "BooleanPropertyDefinition";
            String localName = "BooleanPropertyDefinition";
            String localNameSpace = "BooleanPropertyDefinition";
            Updatability updatability = Updatability.ONCREATE;
            Boolean orderable = false;
            Boolean queryable = false;
            ArrayList<Boolean> defaults = new ArrayList<Boolean>();
            defaults.add(false);
    
            TestBooleanPropertyDefinition spd = new TestBooleanPropertyDefinition(property_boolPropertyId, cardinality,
                    description, displayName, localName, localNameSpace, updatability, orderable, queryable, defaults);
            return spd;
        }
    
        private static TestIdPropertyDefinition createIDPropertyDefinition() {
            Cardinality cardinality = Cardinality.SINGLE;
            String description = "ID property definition";
            String displayName = "IDPropertyDefinition";
            String localName = "IDPropertyDefinition";
            String localNameSpace = "IDPropertyDefinition";
            Updatability updatability = Updatability.READWRITE;
            Boolean orderable = false;
            Boolean queryable = false;
    
            TestIdPropertyDefinition idpd = new TestIdPropertyDefinition(property_idPropertyId, cardinality, description,
                    displayName, localName, localNameSpace, updatability, orderable, queryable, null);
            return idpd;
        }
    
        /**
         * **************************************************************************
         * Inner classes follow
         * **************************************************************************
         * 
         * All of the abstract base classes (for properties and types) 
         * that are used in this example are defined here
         * along with their subclasses for each type that we support in this example.
         * 
         * These classes can be further extended and reused for additional type 
         * mutability operations. 
         * 
         * These were made inner classes so the entire example would be contained
         * in a single Java file. (no design reason) 
         * 
         */
    
        private static class TestStringPropertyDefinition extends TestPropertyDefinition<String> implements
                PropertyStringDefinition {
    
            BigInteger maxLength = null;
    
            public TestStringPropertyDefinition(String idAndQueryName, Cardinality cardinality, String description,
                    String displayName, String localName, String localNameSpace, Updatability updatability,
                    Boolean orderable, Boolean queryable, List<String> defaultValue, List<Choice<String>> choiceList,
                    BigInteger maxLength) {
                super(idAndQueryName, cardinality, description, displayName, localName, localNameSpace, updatability,
                        orderable, queryable, defaultValue, choiceList);
    
                this.maxLength = maxLength;
            }
    
            @Override
            public PropertyType getPropertyType() {
                return PropertyType.STRING;
            }
    
            @Override
            public BigInteger getMaxLength() {
                return maxLength;
            }
    
        }
    
        private static class TestIntegerPropertyDefinition extends TestPropertyDefinition<BigInteger> implements
                PropertyIntegerDefinition {
    
            private BigInteger minVal = null;
            private BigInteger maxVal = null;
    
            public TestIntegerPropertyDefinition(String idAndQueryName, Cardinality cardinality, String description,
                    String displayName, String localName, String localNameSpace, Updatability updatability,
                    Boolean orderable, Boolean queryable, List<BigInteger> defaultValue, BigInteger minVal,
                    BigInteger maxVal, List<Choice<BigInteger>> choiceList) {
                super(idAndQueryName, cardinality, description, displayName, localName, localNameSpace, updatability,
                        orderable, queryable, defaultValue, choiceList);
    
                this.minVal = minVal;
                this.maxVal = maxVal;
            }
    
            @Override
            public PropertyType getPropertyType() {
                return PropertyType.INTEGER;
            }
    
            @Override
            public BigInteger getMaxValue() {
                return this.maxVal;
            }
    
            @Override
            public BigInteger getMinValue() {
                return this.minVal;
            }
        }
    
        private static class TestBooleanPropertyDefinition extends TestPropertyDefinition<Boolean> implements
                PropertyBooleanDefinition {
    
            public TestBooleanPropertyDefinition(String idAndQueryName, Cardinality cardinality, String description,
                    String displayName, String localName, String localNameSpace, Updatability updatability,
                    Boolean orderable, Boolean queryable, List<Boolean> defaultValue) {
    
                super(idAndQueryName, cardinality, description, displayName, localName, localNameSpace, updatability,
                        orderable, queryable, defaultValue, null);
            }
    
            @Override
            public PropertyType getPropertyType() {
                return PropertyType.BOOLEAN;
            }
        }
    
        private static class TestIdPropertyDefinition extends TestPropertyDefinition<String> implements
                PropertyIdDefinition {
    
            public TestIdPropertyDefinition(String idAndQueryName, Cardinality cardinality, String description,
                    String displayName, String localName, String localNameSpace, Updatability updatability,
                    Boolean orderable, Boolean queryable, List<String> defaultValue) {
                super(idAndQueryName, cardinality, description, displayName, localName, localNameSpace, updatability,
                        orderable, queryable, defaultValue, null);
            }
    
            @Override
            public PropertyType getPropertyType() {
                return PropertyType.ID;
            }
    
        }
    
        /** 
         * Base class for all property definition types
         * 
         * See TestStringPropertyDefinition for example of how to subclass this.
         *
         * @param <T>
         */
        abstract private static class TestPropertyDefinition<T> implements PropertyDefinition<T> {
    
            private String idAndQueryName = null;
            private Cardinality cardinality = null;
            private String description = null;
            private String displayName = null;
            private String localName = null;
            private String localNameSpace = null;
            private Updatability updatability = null;
            private Boolean orderable = null;
            private Boolean queryable = null;
    
            private List<T> defaultValue = null;
            private List<Choice<T>> choiceList = null;
    
            public TestPropertyDefinition(String idAndQueryName, Cardinality cardinality, String description,
                    String displayName, String localName, String localNameSpace, Updatability updatability,
                    Boolean orderable, Boolean queryable, List<T> defaultValue, List<Choice<T>> choiceList) {
                super();
                this.idAndQueryName = idAndQueryName;
                this.cardinality = cardinality;
                this.description = description;
                this.displayName = displayName;
                this.localName = localName;
                this.localNameSpace = localNameSpace;
                this.updatability = updatability;
                this.orderable = orderable;
                this.queryable = queryable;
                this.defaultValue = defaultValue;
                this.choiceList = choiceList;
            }
    
            @Override
            public String getId() {
                return idAndQueryName;
            }
    
            @Override
            public Cardinality getCardinality() {
                return cardinality;
            }
    
            @Override
            public String getDescription() {
                return description;
            }
    
            @Override
            public String getDisplayName() {
                return displayName;
            }
    
            @Override
            public String getLocalName() {
                return localName;
            }
    
            @Override
            public String getLocalNamespace() {
                return localNameSpace;
            }
    
            @Override
            abstract public PropertyType getPropertyType();
    
            @Override
            public String getQueryName() {
                return idAndQueryName;
            }
    
            @Override
            public Updatability getUpdatability() {
                return updatability;
            }
    
            @Override
            public Boolean isOrderable() {
                return orderable;
            }
    
            @Override
            public Boolean isQueryable() {
                return queryable;
            }
    
            // methods with static content
    
            @Override
            public List<Choice<T>> getChoices() {
                return this.choiceList;
            }
    
            @Override
            public List<T> getDefaultValue() {
                return this.defaultValue;
            }
    
            
            /**
             * TODO For these remaining attributes you will want to set them 
             * accordingly.  They are all set to static values only 
             * because this is sample code. 
             */
            @Override
            public Boolean isInherited() {
                return false;
            }
    
            @Override
            public Boolean isOpenChoice() {
                return false;
            }
    
            @Override
            public Boolean isRequired() {
                return false;
            }
    
            @Override
            public List<CmisExtensionElement> getExtensions() {
                return null;
            }
    
            @Override
            public void setExtensions(List<CmisExtensionElement> arg0) {
            }
    
        }
    
        /**
         * Base class for all typeDefinitions.  
         * See TestDocumentTypeDefinition for an example of how to subclass this for document.
         *
         */
        private static abstract class TestTypeDefinition implements TypeDefinition {
    
            private String description = null;
            private String displayName = null;
            private String idAndQueryName = null;
            private String localName = null;
            private String localNamespace = null;
            private String parentTypeId = null;
            private Boolean isCreatable = null;
            private Boolean includedInSupertypeQuery = null;
            private Boolean queryable = null;
            private Map<String, PropertyDefinition<?>> propertyDefinitions = new HashMap<String, PropertyDefinition<?>>();
    
            public TestTypeDefinition(String idAndQueryName, String description, String displayName, String localName,
                    String localNamespace, String parentTypeId, Boolean isCreatable, Boolean includedInSupertypeQuery,
                    Boolean queryable, Map<String, PropertyDefinition<?>> propertyDefinitions) {
    
                this.description = description;
                this.displayName = displayName;
                this.idAndQueryName = idAndQueryName;
                this.localName = localName;
                this.localNamespace = localNamespace;
                this.parentTypeId = parentTypeId;
                this.isCreatable = isCreatable;
                this.includedInSupertypeQuery = includedInSupertypeQuery;
                this.queryable = queryable;
    
                if (propertyDefinitions != null) {
                    this.propertyDefinitions = propertyDefinitions;
                }
            }
    
            @Override
            abstract public BaseTypeId getBaseTypeId();
    
            @Override
            public String getDescription() {
                return description;
            }
    
            @Override
            public String getDisplayName() {
                return displayName;
            }
    
            @Override
            public String getId() {
                return idAndQueryName;
            }
    
            @Override
            public String getLocalName() {
                return localName;
            }
    
            @Override
            public String getLocalNamespace() {
                return localNamespace;
            }
    
            @Override
            public String getParentTypeId() {
                return parentTypeId;
            }
    
            @Override
            public Map<String, PropertyDefinition<?>> getPropertyDefinitions() {
                return propertyDefinitions;
            }
    
            @Override
            public String getQueryName() {
                return idAndQueryName;
            }
    
            @Override
            public Boolean isCreatable() {
                return isCreatable;
            }
    
            @Override
            public Boolean isIncludedInSupertypeQuery() {
                return includedInSupertypeQuery;
            }
    
            @Override
            public Boolean isQueryable() {
                return queryable;
            }
    
            
            /**
             * TODO For these remaining attributes you will want to set them 
             * accordingly.  They are all set to static values only 
             * because this is sample code. 
             */
    
            @Override
            public TypeMutability getTypeMutability() {
                return new TestTypeMutability();
            }
    
            @Override
            public Boolean isControllableAcl() {
                return true;
            }
    
            @Override
            public Boolean isControllablePolicy() {
                return false;
            }
    
            @Override
            public Boolean isFileable() {
                return true;
            }
    
            @Override
            public Boolean isFulltextIndexed() {
                return false;
            }
    
            @Override
            public List<CmisExtensionElement> getExtensions() {
                return null;
            }
    
            @Override
            public void setExtensions(List<CmisExtensionElement> extension) {
            }
    
        }
    
        private static class TestDocumentTypeDefinition extends TestTypeDefinition implements DocumentTypeDefinition {
    
            private ContentStreamAllowed contentStreamAllowed = null;
            private Boolean versionable = null;
    
            public TestDocumentTypeDefinition(String idAndQueryName, String description, String displayName,
                    String localName, String localNamespace, String parentTypeId, Boolean isCreatable,
                    Boolean includedInSupertypeQuery, Boolean queryable, ContentStreamAllowed contentStreamAllowed,
                    Boolean versionable, Map<String, PropertyDefinition<?>> propertyDefinitions) {
    
                super(idAndQueryName, description, displayName, localName, localNamespace, parentTypeId, isCreatable,
                        includedInSupertypeQuery, queryable, propertyDefinitions);
    
                this.contentStreamAllowed = contentStreamAllowed;
                this.versionable = versionable;
            }
    
            @Override
            public BaseTypeId getBaseTypeId() {
                return BaseTypeId.CMIS_DOCUMENT;
            }
    
            @Override
            public ContentStreamAllowed getContentStreamAllowed() {
                return contentStreamAllowed;
            }
    
            @Override
            public Boolean isVersionable() {
                return versionable;
            }
    
        }
    
        public static class TestTypeMutability implements TypeMutability {
           
            /**
             * TODO:
             * Change these values based on your repository and requirements
             */
            
            @Override
            public List<CmisExtensionElement> getExtensions() {
                return null;
            }
    
            @Override
            public void setExtensions(List<CmisExtensionElement> arg0) {
            }
            
            @Override
            public Boolean canCreate() {
                return true;
            }
    
            @Override
            public Boolean canDelete() {
                return true;
            }
    
            @Override
            public Boolean canUpdate() {
                return true;
            }
        }
    }
```