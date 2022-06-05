### 1.即连接池(数据源)的作用

之前都是Spring的配置文件配置Bean(我们之前自定义的UserService)

如果是非自定义的对象:

​     1 数据源是为了提高程序性能出现的

​     2实现实例化数据源对象,初始化部分连接对象

​     3使用连接资源时从数据源获取

​     4用完毕后将连接资源归还给数据源

常见的DBCP,C3p0,Druid等

​	

### 2,数据源的开发步骤

1,导入数据源的坐标和数据库驱动坐标

2,创建数据源对象

3,设置数据源的基本连接

四个步骤:1,驱动  2,数据库地址 3,用户名  4, 密码   ,,初始化最大连接个数 

4,使用数据源获取连接资源和归还连接资源(close)





手动创建:

```java
package com.itehima.test;


import com.alibaba.druid.pool.DruidDataSource;
import com.alibaba.druid.pool.DruidPooledConnection;
import com.mchange.v2.c3p0.ComboPooledDataSource;
import com.mysql.jdbc.Driver;
import org.junit.Test;

import java.beans.PropertyVetoException;
import java.sql.Connection;

public class DataSourceTest {

    @Test  //测试手动创建c3p0测试源
    public void test1() throws Exception {
        ComboPooledDataSource dataSource=new ComboPooledDataSource();
        //设置基本的连接参数
        dataSource.setDriverClass("com.mysql.jdbc.Driver");
        dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/test");
        dataSource.setUser("root");
        dataSource.setPassword("sz152261");

        //获取连接对象
        Connection connection = dataSource.getConnection();

        System.out.println(connection);

        connection.close();
    }

    @Test//测试手动创建durid数据原
    public void test2() throws Exception{
        DruidDataSource dataSource=new DruidDataSource();
        //设置基本连接参数
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/test");
        dataSource.setUsername("root");
        dataSource.setPassword("sz152261");

        DruidPooledConnection connection = dataSource.getConnection();
        System.out.println(connection);
        connection.close();

    }
}
```

```java
//这些参数耦合死了,与程序深深绑定不好
```

.properties名字的配置文件键值对形式,比xml解析更快,用配置文件来解耦合

```java
public void test3() throws Exception {
    //读取配置文件
    ResourceBundle resourceBundle=ResourceBundle.getBundle("jdbc");//本身在来加载路径下面,jdbc.properties值用前面就行
    //ResourceBundle工具类不需要new
    String driver = resourceBundle.getString("jdbc.driver");
    String url = resourceBundle.getString("jdbc.url");
    String username = resourceBundle.getString("jdbc.username");
    String password = resourceBundle.getString("jdbc.password");


    //创建数据源对象,设置连接参数
    ComboPooledDataSource dataSource=new ComboPooledDataSource();
    dataSource.setDriverClass(driver);
    dataSource.setJdbcUrl(url);
    dataSource.setUser(username);
    dataSource.setPassword(password);

    //创建连接对象
    Connection connection = dataSource.getConnection();
    System.out.println(connection);
    connection.close();

}
```

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/test
jdbc.username=root
jdbc.password=sz152261
```

jdbc.properties





### 3.以上都是手动,可以让Spring来配置数据源:

```java
//创建数据源对象,设置连接参数
ComboPooledDataSource dataSource=new ComboPooledDataSource();
dataSource.setDriverClass(driver);
dataSource.setJdbcUrl(url);
dataSource.setUser(username);
dataSource.setPassword(password);//这其实都是ComboPooledDataSource这个类的set方法
```

这一部分用set方法注入依赖创建Bean对象正好



#### Spring环境搭建的步骤:

1,导坐标

2,创建业务层,dao层等等,这一步由于导入datasource的java包不用自己创建

3,创建配置文件applicationContext.xml

```xml
class="com.mchange.v2.c3p0.ComboPooledDataSource"
```

class表示Bean的id指向的类,在此处就是你配置的数据源类的位置





```java
public  void test4() throws Exception{
    ApplicationContext app=new ClassPathXmlApplicationContext("applicationContext.xml");
    DataSource dataSource =(DataSource)app.getBean("dataSource");//根据Bean的id来获取
    //app.getBean("DataSource.class");//根据Spring容器要创建的类型,必须是唯一的
    Connection connection = dataSource.getConnection();
    System.out.println(connection);
    connection.close();

}
```

这个代码与Spring的配置文件,这里配置文件的注入是普通的数据类型String类型:即

<property name="driverClass" value="com.mysql.jdbc.Driver"></property

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/test"></property>
        <property name="user" value="root"></property>
        <property name="password" value="sz152261"></property>

    </bean>

</beans>
```

