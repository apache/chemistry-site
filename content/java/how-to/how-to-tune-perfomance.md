Title: Tuning the performance

# Tuning the client performance

The client performance relies on two aspects: **Caching** and the **requested data**.

The OpenCMIS Session object does a lot of caching for you and tries to avoid calls to the repository wherever
possible. In order to be effective, you have to reuse the Session object whenever possible. Creating a new Session
object for each call decreases the performance drastically. Session objects and CMIS objects are thread-safe and
can/should be reused across threads.

[OperationContext](../developing/dev-operation-context.html) objects control which data (properties, renditions, relationships, allowable actions, ACLs,
policies, etc.) is requested from the repository. The more data is requested, the longer it takes to transmit and
process it. By default, OpenCMIS fetches more data than most applications need. Tuning the OperationContext,
and especially the property filter, can result in a significant performance improvement.


## Understanding the client side cache

Client side caching is turned on by default. That is, `getObject()` will first look into the session cache if the object already exists there. 
If this is the case, it returns the object without talking to the repository. So it might return stale objects.  

There are multiple ways to deal with that:

* Refresh the object data that is returned from `getObject()`:

  &nbsp;

  ```java
    CmisObject object = session.getObject(id);
    object.refresh(); // contacts the repository and refreshes the object
    object.refreshIfOld(60 * 1000); // ... or refreshes the object only if the data is older than a minute
  ```

* Turn off the session cache completely:

  &nbsp;

  ```java
    session.getDefaultContext().setCacheEnabled(false);
  ```

* Turn off caching for this `getObject()` call:

  &nbsp;

  ```java
    OperationContext oc = session.createOperationContext();
    oc.setCacheEnabled(false);
    
    CmisObject object = session.getObject(id, oc);
  ```

* Clear the session cache (not recommended!):

  &nbsp;

  ```java
    session.clear();
  ```
