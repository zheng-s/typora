### ① 创建目标接口和目标类（内部有切点）

### ② 创建切面类（内部有增强方法）
![image](https://user-images.githubusercontent.com/65000660/172513953-605ce70c-7ba1-475c-b70a-78d0ad4c861a.png)

### ③ 将目标类和切面类的对象创建权交给 spring

```java
@Component("target")
public class Target implements TargetInterface {
    public void save() {
        System.out.println("save running.....");
        //int i = 1/0;
    }
}
```

```java
@Component("myAspect")
@Aspect //标注当前MyAspect是一个切面类
public class MyAspect {
```

![image](https://user-images.githubusercontent.com/65000660/172514042-a7ba7205-619a-4fa5-9a4b-b56f0e27d05e.png)

### ④ 在切面类中使用注解配置织入关系

```xml

```

```java
//@Before("execution(* com.itheima.anno.*.*(..))")
public void before(){
    System.out.println("前置增强..........");
}
```

### ⑤ 在配置文件中开启组件扫描和 AOP 的自动代理

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
       http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
">

    <!--组件扫描-->
    <context:component-scan base-package="com.itheima.anno"/>

    <!--aop自动代理-->
    <aop:aspectj-autoproxy/>

</beans>
```



### ⑥ 测试









### \1. 注解通知的类型:
![image](https://user-images.githubusercontent.com/65000660/172514022-0ead8a4b-127a-467b-be99-5fa4cc51e118.png)






### \2. 切点表达式的抽取
![image](https://user-images.githubusercontent.com/65000660/172514210-b2ee0d6a-9e86-4acf-9d75-6af7bfbadb72.png)

```java
//定义切点表达式
@Pointcut("execution(* com.itheima.anno.*.*(..))")
public void pointcut(){}
```

```java
@Around("pointcut()")
public Object around(ProceedingJoinPoint pjp) throws Throwable {
    System.out.println("环绕前增强....");
    Object proceed = pjp.proceed();//切点方法
    System.out.println("环绕后增强....");
    return proceed;
}
```



```java
@After("MyAspect.pointcut()")
public void after(){
    System.out.println("最终增强..........");
}
```







### 注解开发步骤:

#### ① 使用@Aspect标注切面类 

#### ② 使用@通知注解标注通知方法 

#### ③ 在配置文件中配置aop自动代理

![image](https://user-images.githubusercontent.com/65000660/172514233-5d037e32-d00e-4ad6-a704-522f34aeec25.png)
