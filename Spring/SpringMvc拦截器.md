 [07_SpringMVC拦截器.pdf](E:\BaiduNetdiskDownload\笔记\第七天资料\PDF\07_SpringMVC拦截器.pdf) 

在访问你目标资源时候做一些相应的过滤

[SpringMVC](https://so.csdn.net/so/search?q=SpringMVC&spm=1001.2101.3001.7020)中的拦截器类似于Servlet中的过滤器（Filter），它主要用于拦截用户请求并作相应的处理。例如通过拦截器可以进行权限验证、记录请求信息的日志、判断用户是否登录等等。

### 创建拦截器类实现HandlerInterceptor接口:里面有三个方法

![image](https://user-images.githubusercontent.com/65000660/172415507-4f43131b-9bb3-4a1b-8e9f-01a29df54256.png)

### 配置拦截器:

![image](https://user-images.githubusercontent.com/65000660/172415568-d0f340fb-b082-4715-a5b4-b935b2dc4c7c.png)

### 测试效果

![image](https://user-images.githubusercontent.com/65000660/172415612-f146bae7-231b-43c8-9d23-a9b8dfec9956.png)
![image](https://user-images.githubusercontent.com/65000660/172415669-a4328ba2-cf3f-4151-adb9-af6ec41cfd19.png)

三个方法可以对controller层进行影响







### 案例:紧接Spring练习,用户没有登陆得情况下,不能对后台菜单进行访问

![image](https://user-images.githubusercontent.com/65000660/172415712-1d5960a2-3458-48e3-a54c-2c5197bfb31c.png)

**入口**  :任何操作逗得登录,所以入口很多,需要拦截器判断session(session通过request进行获取)中有没有user,若有retuen true放行,若false,返回(重定向)登录界面

#### 第一:在prrHandle当中判断用户登录没有



```xml
<!--配置权限拦截器-->
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/**"/>
        <bean class="com.itheima.interceptor.PrivilegeInterceptor"/>
    </mvc:interceptor>
</mvc:interceptors>
```





```java
package com.itheima.interceptor;

import com.itheima.damain.User;
import org.springframework.web.servlet.HandlerInterceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

public class PrivilegeInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //判断用户是否登录,本质判断session中有没有我的user
        HttpSession session = request.getSession();
        User user = (User)session.getAttribute("user");
        if(user==null){
            //没有登录
            response.sendRedirect(request.getContextPath()+"/login.jsp");
            return false;
        }
        return true;
    }
}
```







#### 第二:如果用户名密码确证(比对数据库里的角色),跳转到角色管理界面可操作,若不正确会跳回登录界面(操作方法)

1.在login.jsp表单那里找到地址改成自己得方法

```jsp
<form action="${pageContext.request.contextPath}/user/login"<%--改自己的方法(资源),再到对应地方去创建--%>
```

2,在userController里创建这个方法login,并注解相应的地址,通过UserService调用login方法

返回一个user对象,不空将user存储到sessi中(注入的),跳到首页index.jsp,否则继续到登录界面

```java
@RequestMapping("/login")
public String login(String username, String password, HttpSession session){ //后面加则个session是为了注入,Spring看帮你产生session

    User user=userService.login(username,password);
    if(user!=null){
        //登录成弓,将user存储到session中
        session.setAttribute("user",user);//名字和值
        return "redirect:/index.jsp";
    }
    return "redirect:/login.jsp";
}
```

然后在UserService

```java
public interface UserService {
    List<User> list();

    void save(User user, Long[] roleIds);

    void del(Long userId);

    User login(String username, String password);
}
```



在UserService得实现里:

```java
@Override
public User login(String username, String password) {
    User user=userDao.findByUsernameAndPassword(username,password);
    return user;
}
```

然后userDao.findByUsernameAndPassword在UserDao接口定义,在userDao实现类里实现:

```java
public interface UserDao {
    List<User> findAll();

     Long save(User user);

    void saveUserRoleRel(Long id, Long[] roleIds);

    void delUserRoleRel(Long userId);

    void del(Long userId);

    User findByUsernameAndPassword(String username, String password);
}
```





```java
@Override
public User findByUsernameAndPassword(String username, String password) {
    User user=jdbcTemplate.queryForObject("select * from sys_user where username=? and password=?",new BeanPropertyRowMapper<User>(User.class),username,password);
    return user;
}
```

最后配置文件操作:spring-mvc-xml放行我i刚才自己定义的登录方法login

```xml
<!--配置权限拦截器-->
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/**"/><!--配置对那些资源请求拦截(过滤)这样不行,因为你妹登录成功,所以不让你登录-->
        <!--对那个资源不拦截-->
        <mvc:exclude-mapping path="/user/login"/>
        <bean class="com.itheima.interceptor.PrivilegeInterceptor"/>
    </mvc:interceptor>
</mvc:interceptors>
```





### 异常:





```java
@Override
public User login(String username, String password) {
    try {
        User user = userDao.findByUsernameAndPassword(username, password);
        return user;
    }catch (EmptyResultDataAccessException e) {
        return null;
    }
}
```

```java
@Override
public User findByUsernameAndPassword(String username, String password) throws EmptyResultDataAccessException {
    User user=jdbcTemplate.queryForObject("select * from sys_user where username=? and password=?",new BeanPropertyRowMapper<User>(User.class),username,password);
    return user;
}
```

