#### 1,Spring-ApplicationContext解读

三个抽象类ClassPathXmlApplication:只要在工程的resource里有这个配置文件就行,最常用

![image-20220425105514951](../../../blog/zheng-s/source/image/image-20220425105514951.png)

​       FileSystemXmlApplication:从系统磁盘读取绝对路径,右键cpoypath

   AnnotationConfigApplicationContext:用注解配置容器对象是时要用到这个,读取注解

在Java中，用spring的框架，我们都会通过ApplicationContext接口提供应用程序配置。我们常常需要在代码中获取当前的ApplicationContext。有时会需要通过ApplicationContext获取各种Bean。这时可以使用FileSystemXmlApplicationContext通过提供配置文件的路径，来得到应用程序上下文：







BeanFactory和ApplicationContext
Spring通过一个配置文件描述Bean和Bean之间的依赖关系，利用Java反射功能实例化Bean，并建立Bean之间的依赖关系。

Spring的IOC容器在完成这些底层工作的基础上，还提供了Bean实例缓存、生命周期管理、Bean实例代理、时间发布、资源装载等高级服务。

BeanFactory是Spring框架最核心的接口，它提供了高级IOC的配置机制。

ApplicationContext建立在BeanFactory的基础上，提供了更多面向应用的功能， 它提供了国际化支持和框架事件体系。

我们一般称BeanFactory为IoC容器，而称ApplicationContext为应用上下文，但有时候为了行文方便，我们也将ApplicationContext称为Spring容器。

对于BeanFactory 和 ApplicationContext的用途：

BeanFactory是Spring框架的基础设施，面向Spring本身
ApplicationContext面向使用Spring框架的开发者，几乎所有的应用场合都可以直接使用Application而非底层的BeanFactory.





#### 2,ApplicationContext的继承体系

```java
ApplicationContext app=new ClassPathXmlApplicationContext("applicationContext.xml");
//这是获取Spring容器的配置相当于
```

接口后面是接口实现,通过多态方法;

它的实现类:

1)ClassPathXmlApplicationContext

从类的根路径下加载配置文件

2)FileSystemXmlApplicationContext

从磁盘路径上加载配置文件,配置文件可以在磁盘的任意位置

3)AnnotationConfigApplicationcontext

使用注解配置容器对象时,用此类创建Spring容器.它用来读取

#### 3,getBean的方法

public abstract Object getBean(String s)s就是id

```java
UserService userService= (UserService) app.getBean("userService");
```

其中,当参数的类型是字符串时,表示根据Bean的id从容器中获得Bean实例,返回的是OBject,需要强转

当参数的书库类型是class类型时,表示根据类型从容器中匹配Bean实例,当容器中相同的类型的Bean有多个时,此方法会报错

app.getBean("id");   如果容器中相同类型的Bean有多个,用id的方法来获取

app.getBean(Class);如果有一个,就用class来获取,如果有多个,虽然每个Bean有id,但是,这是通过CLAss类型来获取的,而他门ID可能不一样,但是类型可能相同