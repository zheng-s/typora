配置繁重,注解是趋势,提高开发速度

![image](https://user-images.githubusercontent.com/65000660/172405589-8d7fdc7d-ef5e-46af-8e51-a6ab6abb5537.png)


### 原始注解(老注解)  :

Spring原始注解主要是替代<bean>的配置

例子:自己简单创建三层来演示



```java
package com.itheima.service.impl;

import com.itheima.dao.UserDao;
import com.itheima.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Component;

//<bean id="userService" class="com.itheima.service.impl.UserServiceImpl">
//       用来替换这句话用注解
@Component("userService")   //component不能表示这到底是哪个层的注解,所以Spring提供了三个衍生注解表示属于那个层
                            //所以可以改为@Service()
public class UserServiceImpl implements UserService {
//<property name="userDao" ref="userDao"></property><!--ref引用的是UserDao类的一个实例化对象userDao的名字-->
//    </bean>  注入
    @Autowired    //如果只有Auto五Qual,按照数据类型从Spring容器中进行匹配(UserDao类型的bean),也可一正常运行
                    //如果有多个UserDao类型的bean就不行了
    @Qualifier("userDao") //这是bean的id,在userService这个对象里注入userDao这个对象
    //@Resource("userDao")相当于@A加@Q
    private UserDao userDao;
    public void setUserDao(UserDao userDao) {  //注入,service依赖dao
        this.userDao = userDao;             //如果用的注解方法,set放啊可以不屑
    }

    public  void save(){
       userDao.save();
    }
}

```

```
//@Resource("userDao")相当于@A加@Q
```

```xml
 </bean>
<!-- <bean id="userDao" class="com.itheima.dao.impl.UserDaoImpl"></bean>
 <bean id="userService" class="com.itheima.service.impl.UserServiceImpl">
     <property name="userDao" ref="userDao"></property>&lt;!&ndash;ref引用的是UserDao类的一个实例化对象userDao的名字&ndash;&gt;
 </bean>-->

 <!--配置组件扫描,告诉Spring去找注解,我这不写配置文件了-->
 <context:component-scan base-package="com.itheima"/>
```



```java
 /**
  * 现在来尝试注入普通数据类型
  */

 @Value("shenshen")
private String driver;  //这个应用简单,不能体现出来它的优势



 @Value("${jdbc.driver}") //这是才发挥它的作用
 private  String ddd;
```



```xml
jdbc.driver=com.mysql.jdbc.Driver
```



输出com.mysql.jdbc.Driver

### 新注解:

![image](https://user-images.githubusercontent.com/65000660/172405743-f806cbb0-ae33-44fd-99da-00cf7253e889.png)

![image](https://user-images.githubusercontent.com/65000660/172405766-760fe627-c45b-46d3-8680-b5ecb577ee7d.png)



```java
package com.itheima.config;
/**
 * 这是你数据源相关的配置
 */

import com.mchange.v2.c3p0.ComboPooledDataSource;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;

import javax.sql.DataSource;
import java.beans.PropertyVetoException;

//关于Spring配置的类,代替配置文件
@Configuration//标志改类是Spring的核心配置类,用类的方式去代替文件,用注解的方式取代替标签,
@ComponentScan("com.itheima")// <context:component-scan base-package="com.itheima"/>
@PropertySource("classpath:jdbc.properties")// <context:property-placeholder location="classpath:jdbc.properties"/>
public class DataSourceConfigguration {
    @Value("${jdbc.driver}")
    private  String driver;
    @Value("${jdbc.url}")
    private  String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;



    @Bean("dataSource")//Spring 会将当前方法的返回值以执行名称加入到Spring容器中,就相当于xml配置文件
     public DataSource getDataSource() throws Exception {
         ComboPooledDataSource dataSource=new ComboPooledDataSource();
         //设置基本的连接参数
        //<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
       // <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        //<property name="jdbcUrl" value="jdbc:mysql://localhost:3306/test"></property>
        //<property name="user" value="root"></property>
        //<property name="password" value="sz152261"></property>
        //<!--web配置文件这个是非自定义的bean原始注解好像不行,新注解-->
    //</bean>
         dataSource.setDriverClass(driver);
         dataSource.setJdbcUrl(url);
         dataSource.setUser(username);
         dataSource.setPassword(password); //但是这里要解耦合
         return dataSource;
     }
}


```





```java
package com.itheima.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

/**
 * 这是Spring核心配置文件,得把数据源配置文件引入过来
 */
@Configuration//z标志该类是Spring配置类
@ComponentScan("com.itheima")
//<import resouce=""/>
@Import({DataSourceConfigguration.class})
public class SpringConfiguration {

}
```

然后在这里实现:

```java
package com.itheima.web;

import com.itheima.config.SpringConfiguration;
import com.itheima.service.UserService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

//充当web层
public class UserController {
    public static void main(String[] args) {
        //ApplicationContext app=new ClassPathXmlApplicationContext("applicationContext.xml");
        //在Java中，用spring的框架，我们都会通过ApplicationContext接口提供应用程序配置。
        // 我们常常需要在代码中获取当前的ApplicationContext。
        // 有时会需要通过ApplicationContext获取各种Bean,
        // 但有时候为了行文方便，我们也将ApplicationContext称为Spring容器。
        ApplicationContext app=new AnnotationConfigApplicationContext(SpringConfiguration.class);
        UserService userService=app.getBean(UserService.class);//配置文件的bean这中此类型独一份
        userService.save();
    }
}
```

在此就可以用全注解开发从而不需要xml配置文件了,
