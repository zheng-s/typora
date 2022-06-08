1,导入坐标

2,创建bean

3,创建配置文件applicationContext.xml

4,在配置文件中进行bean的配置

5,通过Spring的客户端创建ApplicationContext对象getBean(id)

### spring配置文件

#### 1,Bean标签的范围配置

Bean标签的范围配置scope    :  

  选singleton,bean新建一个对象是同一个(Bean的实例化个数是一个) **,(实例化时机,当Spring核心文件Xml**

**被加载时就会创建一个Bean实例)创建的时候是当加载配置文件,创建Spring容器时,bean就被创建**

```java
ApplicationContext app=new ClassPathXmlApplicationContext("applicationContext.xml");
        Userdao usedao1=(Userdao)app.getBean("userDao");
        Userdao usedao2=(Userdao)app.getBean("userDao");
        System.out.println(usedao1);
        System.out.println(usedao2);

```

```
com.sz.dao.impl.UserDaoImpl@77caeb3e
com.sz.dao.impl.UserDaoImpl@77caeb3e

```

   应用卸载,容器销毁时对象消失





选prototype:容器存在多个对象,地址不一样..bean的创建时机是在每次getBean时创建,实例化时机是在getBean时

对象长时间不用,被java垃圾回收

#### 2,Bean标签的生命周期的配置

什么时候创建,什么时候销毁,与配置相关的有连个属性

inti_method方法:指定类的初始化方法

destory-method方法:指定类的销毁方法名称

得告诉Spring,我这有个初始化与销毁方法,一会执行一下,在applicationContext.xml

首先配置一下

```java
<bean id="userDao" class="com.sz.dao.impl.UserDaoImpl"></bean>
```

```java
<bean id="userDao" class="com.sz.dao.impl.UserDaoImpl" init-method="init" destroy-method="destory"></bean>
```

#### 3,Bean实例化得三种方式

无参构造方法实例化

以上得都是无参构造:

工厂静态方法实例化

工厂实例方法实例化

#### 4,Bean得依赖注入分析

刚才得代码是让Spring产生dao层得对象,业务层,web层也是有对应代码
![image](https://user-images.githubusercontent.com/65000660/172512705-e0389c0c-dfdf-4d6c-a351-b812588d4a8d.png)


#### 5,依赖注入的概念:

它是Spring框架核心IOC的具体实现,UserService需要dao,在编写程序时,把对象的创建交给了Spring,IOC解耦只是

降低他们依赖关系,但不会消除.即做的框架把持久层对象传入业务层,而不用我们自己获取(持久层有点像数据访问层)

本来service依赖dao,我们把他们依赖关系配置到Spring容器中来降低依赖关系

怎么把dao注入daoservice中?

注入方式:构造方法,set方法

```java
private Userdao userdao;

public void setUserdao(Userdao userdao) {  //注入
    this.userdao = userdao;
}
```

```
<bean id="userService" class="com.sz.dao.service.impl.UserServiceImpl" >
    <property name="userdao" ref="userDao"></property>
</bean>
```

有参构造方法

构造注入:name表示构造方法形参列表参数名   ref都是Bean的id

set注入:name表示set方法后缀名称改首字母小写





三大代码块,表示UserDao的实现类(dao层),UserService(业务层)的实现类,UserController的测试类(web层)

```java
package com.sz.dao.impl;

import com.sz.dao.Userdao;

public class UserDaoImpl implements Userdao {
    public UserDaoImpl() {
        System.out.println("UserDaoImpl创建");   //打印一次,无参构造方法就调用一次,调用一次对象就创建一次
    }
public void init(){
    System.out.println("初始化方法");
}
public void destory(){
        System.out.println("销毁方法");
    }


    public void save(){


        System.out.println("save running----");
    }
}
```

```java
package com.sz.dao.service.impl;
/**
 * 业务层模拟
 */

import com.sz.dao.Userdao;
import com.sz.dao.service.UserService;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class UserServiceImpl implements UserService {

   private Userdao userdao;

    public UserServiceImpl(Userdao userdao) {    //有参构造依赖注入,同时一般都在给个无参构造
        this.userdao = userdao;

    }

    public UserServiceImpl() {     //再告诉Spring你通过哪种方式把依赖注入进去的
    }
    //    public void setUserdao(Userdao userdao) {  //注入
//        this.userdao = userdao;
//    }

    public void save() {          //,UserService要调UserDao,刚才UserDao的对象已经让Spring帮我产生了
//        ClassPathXmlApplicationContext app=new ClassPathXmlApplicationContext("applicationContext.xml");
//        Userdao usedao1=(Userdao)app.getBean("userDao");
        userdao.save();
    }
}
```

```java
package com.sz.dao.web;

import com.sz.dao.service.UserService;
import com.sz.dao.service.impl.UserServiceImpl;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 充当web层,假的,用来测试
 */
public class UserController {
    public static void main(String[] args) {
        //他要获得Service,通过new方法
        //UserService userService=new UserServiceImpl();
        //userService.save(); //为什莫不从容器中获取,因为还没到Spring里配置

        ApplicationContext app=new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService= (UserService) app.getBean("userService");
        userService.save();
    }
}
```

配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--    <bean id="userDao" class="com.sz.dao.impl.UserDaoImpl" init-method="init" destroy-method="destory"></bean>-->
    <bean id="userDao" class="com.sz.dao.impl.UserDaoImpl" ></bean>
<!--    <bean id="userService" class="com.sz.dao.service.impl.UserServiceImpl" >-->
<!--        <property name="userdao" ref="userDao"></property>-->
<!--    </bean>-->
    <bean id="userService" class="com.sz.dao.service.impl.UserServiceImpl" >
        <constructor-arg name="userdao" ref="userDao"></constructor-arg>
    </bean>  //这是set注入方法的配置
</beans>
```

#### 6,Bean的依赖注入的数据类型

上面的操作,都是注入引用的Bean,除了对象能引入(用标签里的ref),普通数据类型,集合都可以在容器中进行注入

下面来正常类型



```java
package com.sz.dao.doma;
//这个是key-value的value值user对象
public class User {
    private String name;
    private String addr;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAddr() {
        return addr;
    }

    public void setAddr(String addr) {
        this.addr = addr;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", addr='" + addr + '\'' +
                '}';
    }
}

```





```java
package com.sz.dao.impl;

import com.sz.dao.Userdao;
import com.sz.dao.doma.User;

import java.util.List;
import java.util.Map;
import java.util.Properties;

public class UserDaoImpl implements Userdao {
    //注入集合(这不是注入,在service的里边才有注入,注入了dao类,dao类又包含了这些新加的属性)
    private List<String> strList;
    private Map<String, User> userMap;
    private Properties properties;

    public void setStrList(List<String> strList) {
        this.strList = strList;
    }

    public void setUserMap(Map<String, User> userMap) {
        this.userMap = userMap;
    }

    public void setProperties(Properties properties) {
        this.properties = properties;
    }

    //注入基本数据类型
    private String username;
    private int age;

    public void setUsername(String username) {            //这是set方法用来注入
        this.username = username;
    }

    public void setAge(int age) {
        this.age = age;
    }
    /*这是注入对象的引用类型
    public UserDaoImpl() {
        System.out.println("UserDaoImpl创建");   //打印一次,无参构造方法就调用一次,调用一次对象就创建一次
    }
    public void init(){
    System.out.println("初始化方法");
    }
    public void destory(){
        System.out.println("销毁方法");
    }
   */

    public void save(){

//        System.out.println(username+"========"+age);
        System.out.println(strList);
        System.out.println(userMap);
        System.out.println(properties);
        System.out.println("save running----");
    }
}
```

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--    <bean id="userDao" class="com.sz.dao.impl.UserDaoImpl" init-method="init" destroy-method="destory"></bean>-->
<!--    <bean id="userDao" class="com.sz.dao.impl.UserDaoImpl" >-->
<!--        <property name="username" value="sz"/>-->
<!--        <property name="age" value="18">-->
<!--        </property>-->
<!--    </bean>-->
    <bean id="userDao" class="com.sz.dao.impl.UserDaoImpl" >
        <property name="strList" >
            <list>
                <value>aaa</value>
                <value>bbb</value>
                <value>ccc</value>
            </list>
        </property>
        <property name="userMap" >
            <map>
                <entry key="user1" value-ref="user1"></entry>
                <entry key="user2" value-ref="user2"></entry>
            </map>
        </property>
        <property name="properties">
            <props>
                <prop key="p1">ppp1</prop>
                <prop key="p2">ppp2</prop>
                <prop key="p3">ppp3</prop>
            </props>
        </property>
    </bean>
<!--    <bean id="userService" class="com.sz.dao.service.impl.UserServiceImpl" >-->
<!--        <property name="userdao" ref="userDao"></property>-->
<!--    </bean>-->
    <bean id="userService" class="com.sz.dao.service.impl.UserServiceImpl" >
        <constructor-arg name="userdao" ref="userDao"></constructor-arg>
    </bean>
    <bean id="user1" class="com.sz.dao.doma.User" >
        <property name="name" value="tom"/>
        <property name="addr" value="beijing"/>
    </bean>
    <bean id="user2" class="com.sz.dao.doma.User" >
        <property name="name" value="lucy"/>
        <property name="addr" value="tianjin"/>
    </bean>
</beans>
```



#### 7,引入其他配置文件(分模块开发),因为Spring的配置文件可能非常庞大

Spring![image](https://user-images.githubusercontent.com/65000660/172512773-8f9c81f3-736a-4c4e-870e-38e86b93649f.png)配置文件庞杂.,可以将部分配置拆解到其他配置文件中,而在Spring主配置文件通过import标签去进行加载

<import resource=".xml"/>
