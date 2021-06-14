Title: Understanding the client side cache

# Understanding the client side cache

Client side caching is turned on by default. That is, `getObject()` will
first look into the session cache if the object already exists there. If
this is the case, it returns the object without talking to the repository.
So it might return stale objects.  

There are multiple ways to deal with that:

* Refresh the object data that is returned from `getObject()`.

  &nbsp;

  ```java
    CmisObject object = session.getObject(id);
    object.refresh(); // contacts the repository and refreshes the object
    object.refreshIfOld(60 * 1000); // ... or refreshes the object only if the data is older than a minute
  ```

* Turn off the session cache completely.

  &nbsp;

  ```java
    session.getDefaultContext().setCacheEnabled(false);
  ```

* Turn off caching for this `getObject()` call.

  &nbsp;

  ```java
    OperationContext oc = session.createOperationContext();
    oc.setCacheEnabled(false);
    
    CmisObject object = session.getObject(id, oc);
  ```

* Clear the session cache (not recommended).

  &nbsp;

  ```java
    session.clear();
  ```

