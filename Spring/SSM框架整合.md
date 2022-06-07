

 [SSM整合.pdf](E:\BaiduNetdiskDownload\笔记\第十七天资料\讲义\SSM整合.pdf) 

SSM:主要是spring与mybatis的整合,springmvc主要是与spring整合过了,本来就是spring的成员

### 1,原始整合方式

#### 数据库的数据(表)
![image](https://user-images.githubusercontent.com/65000660/172406148-d292fce9-1b0c-4992-89af-442d8866b466.png)

#### \2. 创建Maven工程

![image](https://user-images.githubusercontent.com/65000660/172406099-fd63965a-cc8f-49ff-9736-8a834d355318.png)




#### 3.导入Maven坐标

#### \4. 编写实体类

```java
package com.sz.domain;
//创建实体,在页面展示.可以不重写toString方法
public class Account {
    private  Integer id;
    private String name;
    private double money;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getMoney() {
        return money;
    }

    public void setMoney(double money) {
        this.money = money;
    }
}
```

#### \5. 编写Mapper接口

```java
package com.sz.mapper;

import com.sz.domain.Account;

import java.util.List;

public interface AccountMapper {
    public void save(Account account);
    public List<Account> findAll();
}
```

#### \6. 编写Service接口

#### \7. 编写Service接口实现

#### \8. 编写Controller

```java
package com.sz.controller;

import com.sz.domain.Account;
import org.springframework.web.servlet.ModelAndView;

public class AccountController {
    //保存
    public String save(Account account){
        return null;
    }

    //查询,查询到的数据封装到ModelAndView
    public ModelAndView findAll(){
        return null;
    }
}
```

#### \9. 编写添加页面

```jsp
<%--
  Created by IntelliJ IDEA.
  User: lenovo
  Date: 2022/5/7
  Time: 17:34
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h1>添加账户信息的表单</h1>
    <form name="accountForm" action="${pageContext.request.contexPath}/account/save" method="post">
        账户名称:<input type="text" name="name"><br>
        账户金额:<input type="text" name="money"><br>
       <input type="submit" value="保存"><br>

    </form>
</body>
</html>
```

#### \10. 编写列表页面

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h1>展示账户数据列表</h1>
    <table>
        <tr>
            <th>账户id</th>
            <th>账户名称</th>
            <th>账户金额</th>
        </tr>
        <tr>
            <td>1</td>
            <td>zhangsan</td><%--这个地方改为for循环来展示查询列表结果的数据--%>
            <td>5000</td>
        </tr>
    </table>
</body>
</html>
```

#### \11. 编写相应配置文件:

spring.xml是spring与springmvc的..     mybatis映射文件与核心文件   spring与springmvc需要借助web.xml与web项目结合,spring需要监听器,springmvc需要一个前端控制器,

![image](https://user-images.githubusercontent.com/65000660/172406273-c7b48d8b-fb59-4796-82b7-f7bc37fff9f5.png)



##### 第一,mapper.xml的配置:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.sz.mapper.AccountMapper">
    <insert id="save" parameterType="account">
        insert into account values(#{id},#{name},#{money})
    </insert>
    
    <select id="findAll" resultType="account">
        select * from account
    </select>

</mapper>
```

account在sqlMapConfig,xml里定义了别名

##### 核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--加载properties文件-->
    <properties resource="jdbc.properties"></properties>

    <!--定义别名-->
<typeAliases>
    <typeAlias type="com.sz.domain.Account" alias="account"></typeAlias>
</typeAliases>

    <!--环境-->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"></transactionManager><!--事务管理器-->
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"></property>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="jdbc.username"/>
                <property name="password" value="jdbc.password"/>
            </dataSource><!--数据源-->
        </environment>
    </environments>

    <!--加载映射-->
    <mappers>
       <!-- <mapper resource="com/sz/mapper/AccountMapper.xml"></mapper>-->
        <package name="com.sz.mapper"/>
    </mappers>

</configuration>
```

##### spring相关的配置文件applicationContext.xml与spring-mvc.xml:

```xml
但是controller是web层的,由Spring-mvc管理-->
```

```xml
<!--组件扫描 扫描service和mapper--><!--但是controller是web层的,由Spring-mvc管理-->
<context:component-scan base-package="com.sz">
    <!--配出controller-->
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/mvc
http://www.springframework.org/schema/mvc/spring-mvc.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd">

    <!--组件扫描  主要扫描controller-->
    <context:component-scan base-package="com.sz.controller">

    </context:component-scan>
    <!--配置mvc注解驱动-->
    <mvc:annotation-driven></mvc:annotation-driven>
    <!--内部资源视图解析器-->
    <bean id="resourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-NF/pages/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>
    <!--开发静态资源访问权限-->
    <mvc:default-servlet-handler></mvc:default-servlet-handler>


</beans>
```

##### web.xml:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://java.sun.com/xml/ns/javaee"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">

    <!--spring 监听器-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!--springmvc的前端控制器-->
    <servlet>
        <servlet-name>DispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!--乱码过滤器-->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

</web-app>
```

```xml
<url-pattern>/</url-pattern><!--任何访问资源都进web层的框架-->
```



#### \12. 测试添加账户

#### \13. 测试账户列表





### 弊端主要体现在Service层:

```java
InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
SqlSession sqlSession = sqlSessionFactory.openSession();
AccountMapper mapper = sqlSession.getMapper(AccountMapper.class);
List<Account> accountList = mapper.findAll();

sqlSession.close();
return accountList;
```



每次在执行这个业务方法时候都会加载这个文件并且每次都建立一个工厂

让Spring帮我产生AccountMapper





![image](https://user-images.githubusercontent.com/65000660/172406432-34d6a143-7619-44fe-8a13-dfaea102a85f.png)





```
<!--加载propeties文件-->


<!--配置数据源信息-->


<!--配置sessionFactory-->


<!--扫描mapper所在的包 为mapper创建实现类-->



<!--声明式事务控制-->
<!--平台事务管理器-->
```

mybatis-spring这个包提供了一个工厂的实现类

##### 在applicationContext.xml里写入一下配置文件:将SqlSessionFactory配置到Spring容器中

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd
http://www.springframework.org/schema/aop
http://www.springframework.org/schema/aop/spring-aop.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd">

    <!--组件扫描 扫描service和mapper--><!--但是controller是web层的,由Spring-mvc管理-->
    <context:component-scan base-package="com.sz">
        <!--配出controller-->
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

    <!--加载propeties文件-->
    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>

    <!--配置数据源信息-->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"></property>
        <property name="jdbcUrl" value="${jdbc.url}"></property>
        <property name="user" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>

    <!--配置sessionFactory--><!--为了省略每次逗得产生对数据库操作的代理对象mapper-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"></property>
        <!--加载mybatis的核心文件-->
        <property name="configLocation" value="sqlMapConfig.xml"></property>
    </bean>

    <!--扫描mapper所在的包 为mapper创建实现类-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.sz.mapper"></property>
    </bean>


    <!--声明式事务控制-->
    <!--平台事务管理器-->


    <!--配置事务增强-->


    <!--事务的aop织入-->

</beans>
```

##### 



##### 关于事务:

```xml
<!--平台事务管理器-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"></property>
</bean>

<!--配置事务增强-->
<tx:advice id="txAdvice">
    <tx:attributes>
        <tx:method name="*"/>
    </tx:attributes>
</tx:advice>

<!--事务的aop织入-->
<aop:config>
    <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.sz.service.impl.*.*(..))"></aop:advisor>
</aop:config>
```

