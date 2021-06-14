Title: Using Maven

# Using Maven

You can use OpenCMIS with [Maven](http://maven.apache.org/).
Check the latest version available by [searching the Apache repository](https://repository.apache.org/index.html#nexus-search;quick~opencmis).
Replace the version number x.y.z with the version of the release you have in use (e.g. 0.5.0)


## Client

Use this fragment to add the OpenCMIS client jars and all dependencies. 

```xml
     <dependency>
        <groupId>org.apache.chemistry.opencmis</groupId>
        <artifactId>chemistry-opencmis-client-impl</artifactId>
        <version>x.y.z</version>
     </dependency>
```

## Server

Use this fragment to add the OpenCMIS server framework and all dependencies.

```xml
     <dependency>
        <groupId>org.apache.chemistry.opencmis</groupId>
        <artifactId>chemistry-opencmis-server-support</artifactId>
        <version>x.y.z</version>
     </dependency>
```
