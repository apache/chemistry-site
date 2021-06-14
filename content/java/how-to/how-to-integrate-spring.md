Title: Using Spring Framework (Server)

# Using Spring Framework (Server)

By default, the OpenCMIS services factory is set up by a context listner
configured in the web.xml. If you want or need Spring to set up the
services factory, remove the context listener from the `web.xml` and use a
bean like this instead:

```java
    public class CmisLifecycleBean implements ServletContextAware,
    InitializingBean, DisposableBean
    {
        private ServletContext servletContext;
        private CmisServiceFactory factory;
    
        @Override
        public void setServletContext(ServletContext servletContext)
        {
            this.servletContext = servletContext;
        }

        public void setCmisServiceFactory(CmisServiceFactory factory)
        {
            this.factory = factory;
        }

        @Override
        public void afterPropertiesSet() throws Exception
        {
            if (factory != null)
            {
                factory.init(new HashMap<String, String>());
                servletContext.setAttribute(CmisRepositoryContextListener.SERVICES_FACTORY, factory);
            }
        }

        @Override
        public void destroy() throws Exception
        {
            if (factory != null)
            {
                factory.destroy();
            }
        }
    }
```

The Spring configuration could look like this:
    
```xml
    <bean id="CmisLifecycleBean" class="org.example.mycmisservice.CmisLifecycleBean">
        <property name="cmisServiceFactory" ref="CmisServiceFactory" />
    </bean>
    
    <bean id="CmisServiceFactory" class="org.example.mycmisservice.MyCmisServiceFactory">
    </bean>
```
