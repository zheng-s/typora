### 开门见山

[设计模式](https://so.csdn.net/so/search?q=设计模式&spm=1001.2101.3001.7020)--Proxy

增强一个对象的功能

买火车票，app就是一个代理，他代理了火车站，小区当中的代售窗口

java当中如何实现代理





### 什么是AOP?

面向切面编程,技术,运行期间执行的,通过动态代理技术实现(不修改源码对目标)

动态代理就是，在程序运行期，创建目标对象的代理对象，并对目标对象中的方法进行功能性增强的一种技术。在生成代理对象的过程中，目标对象不变，代理对象中的方法是目标对象方法的增强方法。可以理解为运行期间，对象中方法的动态拦截，在拦截方法的前后执行功能操作。

代理类在程序运行期间，创建的代理对象称之为动态代理对象。这种情况下，创建的代理对象，并不是事先在Java代码中定义好的。而是在运行期间，根据我们在动态代理对象中的“指示”，动态生成的。也就是说，你想获取哪个对象的代理，动态代理就会为你动态的生成这个对象的代理对象。动态代理可以对被代理对象的方法进行功能增强。有了动态代理的技术，那么就可以在不修改方法源码的情况下，增强被代理对象的方法的功能，在方法执行前后做任何你想做的事情。

![image](https://user-images.githubusercontent.com/65000660/172407229-fd9af3f6-b772-4eb1-b46b-9d90cddb7f4b.png)

创建代理对象的两个方法：

#### //JDK动态代理

Proxy.newProxyInstance(三个参数);

#### //CGLib动态代理

Enhancer.create(两个参数);

![image](https://user-images.githubusercontent.com/65000660/172407405-a71f9641-7665-4328-9ee7-f66d62568e61.png)


### AOP的底层实现:

#### JDK动态代理:

需要接口,接口的实现(目标对象),增强对象,实现动态代理的方法

接口:

```java
package com.itheima.proxy.jdk;

public interface TargetInterface {

    public void save();

}
```

```java

package com.itheima.proxy.jdk;

public class  Target implements TargetInterface {
    public void save() {
        System.out.println("save running.....");
    }
}


```

**目标类

动态代理实现:



```java
package com.itheima.proxy.jdk;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class ProxyTest {

    public static void main(String[] args) {

        //目标对象
        final Target target = new Target();

        //增强对象
        final Advice advice = new Advice();

        //返回值 就是动态生成的代理对象
        TargetInterface proxy = (TargetInterface) Proxy.newProxyInstance(
                target.getClass().getClassLoader(), //目标对象类加载器
                target.getClass().getInterfaces(), //目标对象相同的接口字节码对象数组
                new InvocationHandler() {
                    //调用代理对象的任何方法  实质执行的都是invoke方法
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        advice.before(); //前置增强
                        Object invoke = method.invoke(target, args);//执行目标方法
                        advice.afterReturning(); //后置增强
                        return invoke;
                    }
                }
        );

        //调用代理对象的方法
        proxy.save();

    }

}
//创建目标对象的代理对象，并对目标对象中的方法进行功能性增强
 //建一个增强的对象,代理对象中的方法是目标对象方法的增强方法
```

代理对象

```java
package com.itheima.proxy.jdk;
public class Advice {  

    public void before(){
        System.out.println("前置增强....");
    }

    public void afterReturning(){
        System.out.println("后置增强....");
    }

}
```





### cglib的动态实现:

第三方的,要导入jar包,Spring已经集成到了core核心中了

需要目标对象,增强方法,实现动态代理的测试(返回动态代理对象)[这些不需要写,有spring来写]

```java
package com.itheima.proxy.cglib;


public class Target {
    public void save() {
        System.out.println("save running.....");
    }
}
```

```java
//advice:目标对象你截到之后需要增强,你需要一个增强的逻辑,得放到一个方法中,它就叫做advice
//也就是一个切面类
package com.itheima.proxy.cglib;

public class Advice {

    public void before(){
        System.out.println("前置增强....");
    }

    public void afterReturning(){
        System.out.println("后置增强....");
    }

}
```



```java
package com.itheima.proxy.cglib;

import com.itheima.proxy.jdk.TargetInterface;
import org.springframework.cglib.proxy.Enhancer;
import org.springframework.cglib.proxy.MethodInterceptor;
import org.springframework.cglib.proxy.MethodProxy;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class ProxyTest {

    public static void main(String[] args) {

        //目标对象
        final Target target = new Target();

        //增强对象
        final Advice advice = new Advice();

        //返回值 就是动态生成的代理对象  基于cglib
        //1、创建增强器
        Enhancer enhancer = new Enhancer();
        //2、设置父类（目标）
        enhancer.setSuperclass(Target.class);
        //3、设置回调
        enhancer.setCallback(new MethodInterceptor() {
            public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
                advice.before(); //执行前置
                Object invoke = method.invoke(target, args);//执行目标
                advice.afterReturning(); //执行后置
                return invoke;
            }
        });
        //4、创建代理对象
        Target proxy = (Target) enhancer.create();


        proxy.save();


    }

}
```





### AOP相关概念:

Spring 的 AOP 实现底层就是对上面的动态代理的代码进行了封装，封装后我们只需要对需要关注的部分进行代码编 写，并通过配置的方式完成指定目标的方法增强。

![image](https://user-images.githubusercontent.com/65000660/172407504-1fcf1dbc-97ba-4cf3-8fde-28a1f39b150b.png)


被切入的连接点(就是一个等待被增强的方法)叫做切入点

advice:目标对象你截到之后需要增强,你需要一个增强的逻辑,得放到一个方法中,它就叫做advice

切点加通知(advice)就是切面

weaving(织入):将你的切点与advice结合得过程就叫做weaving,是动词,通过配置文件来体现



### AOP开发明确得事项:

#### 1,需要编写得内容

##### 编写业务核心代码(目标类得目标方法)

##### 编写切面类(advice),切面类中有通知(增强方法)

//advice:目标对象你截到之后需要增强,你需要一个增强的逻辑,得放到一个方法中,它就叫做advice

##### 在配置文件中,哦欸之植入关系.即将那些advice与那些连接带你进行结合

### 2,AOP技术实现的内容(就相当于动态代理的那一堆源码)

Spring 框架监控切入点方法的执行。一旦监控到切入点方法被运行，使用代理机制，动态创建目标对象的 代理对象，根据通知类别，在代理对象的对应位置，将通知对应的功能织入，完成完整的代码逻辑运行。

### 3. AOP 底层使用哪种代理方式

在 spring 中，框架会根据目标类是否实现了接口来决定采用哪种动态代理的方式





