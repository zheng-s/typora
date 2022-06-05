

### SpringMvc的开发步骤:



![897ebbe493ccb12ef8497f48d8f7596](../../../blog/zheng-s/source/image/897ebbe493ccb12ef8497f48d8f7596.jpg)





![image-20220429090948086](../../../blog/zheng-s/source/image/image-20220429090948086.png)



![image-20220429092735898](../../../blog/zheng-s/source/image/image-20220429092735898.png)





### SpringMvc的组件解析与执行流程:





### SpringMvc的数据请求与响应:

#### 1,直接页面跳转:

##### 1.1直接返回字符串进行视图跳转,你反悔的字符串前面加forward:代表转发,加rediret:代表重定向

![image-20220429165838262](../../../blog/zheng-s/source/image/image-20220429165838262.png)

##### 1.2通过ModelAndView对象进行返回:在方法中new一个ModelAndView对象,也可以直接方法上进行注入

```java
@RequestMapping(value="/quick9")
public ModelAndView save9(ModelAndView modelAndView){  //这里的参数也可以只串Model,model.setAttribute
    
    //Springmvc可以帮你进行注入,直接给你提供了ModelAndView对象
    modelAndView.addObject("username","shen");
    
    modelAndView.setViewName("success");

    return modelAndView;
}
```







#### (插入知识点)request对象,用setAttribute方法往request域存数据,再转发再页面显示数据:

```java
@RequestMapping(value="/quick2")
public String save2(HttpServletRequest request){ //Spring框架调用这个方法时候,发现我需要HttpServletRequest这个类的对象,框架就负责给我
    request.setAttribute("username","苦丁与");
    return "success";

}
```

![image-20220429172924198](../../../blog/zheng-s/source/image/image-20220429172924198.png)



**request是tomcat帮你创建的对象,里边有东西,http请求的数据都再这里边封装,请求行,请求头,请求体等等,我要用它存数据,然后返回视图,类似于modelAndView,一个是Springmvc帮你封装好的都西昂,而request是javaweb的对象,这种不常用,尽可能与javaweb解耦**

![image-20220429172945894](../../../blog/zheng-s/source/image/image-20220429172945894.png)

#### 2,直接回写数据

##### 2.1直接返回字符串,需要借助注解@ResponseBody告知返回的是串不是视图名字

![image-20220429171641559](../../../blog/zheng-s/source/image/image-20220429171641559.png)



```java
@RequestMapping(value = "/quick3")
public void save3( HttpServletResponse response) throws IOException {
   response.getWriter().println("hello itcast");
}
```

方法的调用者是Spring框架,会提供一个实参给这个方法不用我们操心





```java
@RequestMapping(value = "/quick5")
@ResponseBody//告知SPringmvc框架该方法不进行视图(页面)跳转,直接数据响应回写
public String save5() throws IOException {
    return "{\"username\":\"zhangsan\":\"age\":18}";
}
```



```java

```





##### 2.2返回对象或者集合,,期望SpringMvc帮你把对象或者集合转化为josn再返回,你需要做的是两步::仍然加@ResponseBody,  要指定一个转换器SpringMvc的配置文件中加<mvc:annotation-driven/>,)

```java
@RequestMapping(value = "/quick6")
@ResponseBody//告知SPringmvc框架该方法不进行视图(页面)跳转,直接数据响应回写
public String save6() throws IOException {
    User user=new User();
    user.setUsername("lisa");
    user.setAge(30);
    //使用josn的一个转换工具将对象转换为josn格式字符串返回
    ObjectMapper objectMapper=new ObjectMapper();
    String josn=objectMapper.writeValueAsString(user);
    return josn;
}
```

```java
@RequestMapping(value = "/quick7")
@ResponseBody//告知SPringmvc框架该方法不进行视图(页面)跳转,直接数据响应回写
//期望Springmvc框架自动将User对象转化为josn字符串
public User save7() throws IOException {
    User user=new User();
    user.setUsername("lisa2");
    user.setAge(22);
    return user;
}
```



### SpringMVC获得请求数据:

![image-20220430160928126](../../../blog/zheng-s/source/image/image-20220430160928126.png)

#### 基本数据类型:

```java
@RequestMapping(value = "/quick10")
@ResponseBody//告知SPringmvc框架该方法不进行视图(页面)跳转,直接数据响应回写
//但void说明响应体为空,就是什么都不回写,
public void save10(String username, int age) throws IOException {
    System.out.println(username);
    System.out.println(age);

}
```

http://localhost:8888/itheima_spring_mvc/user/quick10?username=zhangsan&age=18

username,age就是请求参数



结果:![image-20220430161947369](../../../blog/zheng-s/source/image/image-20220430161947369.png)

#### POJO数据类型:

就是客户端发送的数据到达服务端之后,到达web层,Springmvc要把参数封装到实体中

请求参数名曾要与实体属性名一致,



```java
@RequestMapping(value = "/quick11")
@ResponseBody
public void save11(User user)throws IOException{
    System.out.println(user);
}
```

http://localhost:8888/itheima_spring_mvc/user/quick11?username=zhangsan&age=19



![image-20220430162728605](../../../blog/zheng-s/source/image/image-20220430162728605.png)



#### 数组数据类型:

数组名称要与请求参数名称一致:

strs=aaa&strs=bbb&strs=ccc这些就相当于请求参数,这是获得的集合请求参数

```java
@RequestMapping(value = "/quick12")
@ResponseBody
public void save12(String[] strs)throws IOException{
    System.out.println(Arrays.asList(strs));
}
```

http://localhost:8888/itheima_spring_mvc/user/quick12?strs=aaa&strs=bbb&strs=ccc

![image-20220430163213284](../../../blog/zheng-s/source/image/image-20220430163213284.png)





#### 以上提交数据时候都是用的get方法:

#### 集合数据类型:



##### 方法一

获得集合参数时候,要将集合参数包装到一个POJO中才行:这里用的post方法

```java
@RequestMapping(value = "/quick13")
@ResponseBody
public void save13(VO vo)throws IOException{
    System.out.println(vo);

}
```

```java
package com.itheima.domain;

import java.util.List;

/**
 * 这个是获得获得集合参数时候,要将集合参数包装到一个POJO中才行:这就是那个POJO类
 */
public class VO {
    private List<User> userList;

    public List<User> getUserList() {
        return userList;
    }

    @Override
    public String toString() {
        return "VO{" +
                "userList=" + userList +
                '}';
    }

    public void setUserList(List<User> userList) {
        this.userList = userList;
    }
}
```

```jsp
<%--
  Created by IntelliJ IDEA.
  User: lenovo
  Date: 2022/4/30
  Time: 16:41
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="${pageContext.request.contextPath}/user/quick13" method="post">
        <%--表明集合里是第几个User对象的username与age--%>
        <input type="text" name="userList[0].username"><br/>
        <input type="text" name="userList[0].age"><br/>
        <input type="text" name="userList[1].username"><br/>
        <input type="text" name="userList[1].age"><br/>
        <input type="submit" value="提交">
    </form>
</body>
</html>
```





##### 方法二:

![image-20220430170725410](../../../blog/zheng-s/source/image/image-20220430170725410.png)



ajax.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script src="${pageContext.request.contextPath}/js/jquery-3.3.1.js"></script>
    <script>
        var userList=new Array();
        userList.push({username:"zhang",age:18});
        userList.push({username:"lisi",age:22});

        $.ajax({
            type:"POST",
            url:"${pageContext.request.contextPath}/user/quick14",
            data:JSON.stringify(userList),
            contentType:"application/json;charset=utf-8"
        });
    </script>
</head>
<body>

</body>
</html>
```



![image-20220430174825123](../../../blog/zheng-s/source/image/image-20220430174825123.png)

```java
@RequestMapping(value = "/quick14")
@ResponseBody
public void save14(@RequestBody List<User> userList)throws IOException{
    System.out.println(userList);

}
```



### 小知识点:

```xml
<mvc:resources mapping="/js/**" location="/js/"/>
<!--开放资源的访问,mapping代表映射地址,location代表资源具体所在的路径-->

<mvc:default-servlet-handler/><!--Springmvc帮你找不到资源就交给tomcat来找-->
```



### 请求数据的乱码问题:

![image-20220430203119845](../../../blog/zheng-s/source/image/image-20220430203119845.png)

```xml
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping><!--表示对那个资源进行过滤-->
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

![image-20220430203608698](../../../blog/zheng-s/source/image/image-20220430203608698.png)





###  参数绑定注解@requestParam:

```java
@RequestMapping(value = "/quick15")
@ResponseBody
public void save15(@RequestParam(value = "name") String username)throws IOException{
    System.out.println(username);
}
```

![image-20220430204409842](../../../blog/zheng-s/source/image/image-20220430204409842.png)



![image-20220430204615675](../../../blog/zheng-s/source/image/image-20220430204615675.png)





```java
@RequestMapping(value = "/quick15")
@ResponseBody
public void save15(@RequestParam(value = "name",required = false,defaultValue = "shenshen") String username)throws IOException{
    System.out.println(username);
}
```





