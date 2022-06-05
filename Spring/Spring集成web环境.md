1.1 ApplicationContext应用上下文获取方式 应用上下文对象是通过new ClasspathXmlApplicationContext(spring配置文件) 方式获取的，但是每次从 容器中获得Bean时都要编写new ClasspathXmlApplicationContext(spring配置文件) ，这样的弊端是配置 文件加载多次，应用上下文对象创建多次。 在Web项目中，可以使用ServletContextListener监听Web应用的启动，我们可以在Web应用启动时，就加 载Spring的配置文件，创建应用上下文对象ApplicationContext，在将其存储到最大的域servletContext域 中，这样就可以在任意位置从域中获得应用上下文ApplicationContext对象了





上面的分析不用手动实现，Spring提供了一个监听器ContextLoaderListener就是对上述功能的封装，该监 听器内部加载Spring配置文件，创建应用上下文对象，并存储到ServletContext域中，提供了一个客户端工 具WebApplicationContextUtils供使用者获得应用上下文对象。 所以我们需要做的只有两件事： ① 在web.xml中配置ContextLoaderListener监听器（导入spring-web坐标） ② 使用WebApplicationContextUtils获得应用上下文对象ApplicationContex





1.3 导入Spring集成web的坐标 org.springframework spring-web 5.0.5.RELEASE



1.4 配置ContextLoaderListener监听器  contextConfigLocation classpath:applicationContext.xml   org.springframework.web.context.ContextLoaderListener



1.5 通过工具获得应用上下文对象 ApplicationContext applicationContext =  WebApplicationContextUtils.getWebApplicationContext(servletContext); Object obj = applicationContext.getBean("id")



1.5 通过工具获得应用上下文对象 ApplicationContext applicationContext =  WebApplicationContextUtils.getWebApplicationContext(servletContext); Object obj = applicationContext.getBean("id")