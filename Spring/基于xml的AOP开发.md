### ① 导入 AOP 相关坐标

```xml
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.8.4</version>
</dependency>
```

AOP是一种思想,用aspectj的配置





### ② 创建目标接口和目标类（内部有切点）

目标类

```java
package com.itheima.aop;

public class Target implements TargetInterface {
    public void save() {
        System.out.println("save running.....");
        //int i = 1/0;
    }
}
```

目标接口:

```java
package com.itheima.aop;

public interface TargetInterface {

    public void save();

}
```



### ③ 创建切面类（内部有增强方法）



切面类:

```java
package com.itheima.aop;

import org.aspectj.lang.ProceedingJoinPoint;

public class MyAspect {

    public void before(){
        System.out.println("前置增强..........");
    }

}
```





### ④ 将目标类和切面类的对象创建权交给 spring

```xml
<!--目标对象-->
<bean id="target" class="com.itheima.aop.Target"></bean>

<!--切面对象-->
<bean id="myAspect" class="com.itheima.aop.MyAspect"></bean>


```







### ⑤ 在 applicationContext.xml 中配置织入关系



```xml
<!--配置织入：告诉spring框架 哪些方法(切点)需要进行哪些增强(前置、后置...)-->
<aop:config>
    <!--声明切面-->
    <aop:aspect ref="myAspect">
        <!--抽取切点表达式-->
        <aop:pointcut id="myPointcut" expression="execution(* com.itheima.aop.*.*(..))"></aop:pointcut>
        <!--切面：切点+通知-->
        <!--<aop:before method="before" pointcut="execution(public void com.itheima.aop.Target.save())"/>-->
        <!--<aop:before method="before" pointcut="execution(* com.itheima.aop.*.*(..))"/>
        <aop:after-returning method="afterReturning" pointcut="execution(* com.itheima.aop.*.*(..))"/>-->
        <!--<aop:around method="around" pointcut="execution(* com.itheima.aop.*.*(..))"/>
        <aop:after-throwing method="afterThrowing" pointcut="execution(* com.itheima.aop.*.*(..))"/>
        <aop:after method="after" pointcut="execution(* com.itheima.aop.*.*(..))"/>-->
        <aop:around method="around" pointcut-ref="myPointcut"/>
        <aop:after method="after" pointcut-ref="myPointcut"/>

    </aop:aspect>
</aop:config>
```





### ⑥ 测试代码

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class AopTest {

    @Autowired
    private TargetInterface target;

    @Test
    public void test1(){
        target.save();
    }


}
```







```xml
<!--配置织入：告诉spring框架 哪些方法(切点)需要进行哪些增强(前置、后置...)-->
<aop:config>
    <!--声明切面-->
    <aop:aspect ref="myAspect">
        <!--抽取切点表达式-->
        <aop:pointcut id="myPointcut" expression="execution(* com.itheima.aop.*.*(..))"></aop:pointcut>
        <!--切面：切点+通知-->
       

    </aop:aspect>
</aop:config>
```

### 总结:

SPring帮你的前提是几个大类在Spring里配置了bean

而且切面类要想让Spring识别,  得加<aop:aspect ref="myAspect">这句,因为切面类本身是一个普通bean

 --<aop:before method="before" pointcut="execution(public void com.itheima.aop.Target.save())"/>-

**before**表示前置增强,,   **method="before"**,那你增强得逻辑应该在一个方法中,那个方法就是切面类得before方法

**pointcut="execution(public void com.itheima.aop.Target.save())**,切点表达式得写法,对目标类得save方法起作用





### 关于切点表达式:

execution([修饰符] 返回值类型 包名.类名.方法名(参数))



访问修饰符可以省略



返回值类型、包名、类名、方法名可以使用星号* 代表任意

execution(void com.itheima.aop.Target.*(..))  *是通配符,Target包下面如果有一百个方法,这一百个方法都会被增强



 包名与类名之间一个点 . 代表当前包下的类，两个点 .. 表示当前包及其子包下的类

**execution(* com.itheima.aop.*.*(..)):最常用,aop包下得任意类得任意方法增强,有没有参数无所谓**



参数列表可以使用两个点 .. 表示任意个数，任意类型的参数列表

**execution(* com.itheima.aop..*.*(..)) aop包及其子包下得任意类得任意方法增强**





### 关于advice得类型:

![image-20220504215034699](../../../blog/zheng-s/source/image/image-20220504215034699.png)





### 切点表达式得抽取:

```xml
<aop:pointcut id="myPointcut" expression="execution(* com.itheima.aop.*.*(..))"></aop:pointcut>
<!--切面：切点+通知-->
<!--<aop:before method="before" pointcut="execution(public void com.itheima.aop.Target.save())"/>-->
<!--<aop:before method="before" pointcut="execution(* com.itheima.aop.*.*(..))"/>
<aop:after-returning method="afterReturning" pointcut="execution(* com.itheima.aop.*.*(..))"/>-->
<!--<aop:around method="around" pointcut="execution(* com.itheima.aop.*.*(..))"/>
<aop:after-throwing method="afterThrowing" pointcut="execution(* com.itheima.aop.*.*(..))"/>
<aop:after method="after" pointcut="execution(* com.itheima.aop.*.*(..))"/>-->
<aop:around method="around" pointcut-ref="myPointcut"/>
<aop:after method="after" pointcut-ref="myPointcut"/>
```





### 知识要点

![image-20220504220045126](../../../blog/zheng-s/source/image/image-20220504220045126.png)