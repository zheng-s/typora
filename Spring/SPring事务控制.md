### 编程式:自己写(基础知识点,便于学声明式)

#### **PlatformTransactionManager 接口**

是 spring 的事务管理器，它里面提供了我们常用的操作事务的方法。

PlatformTransactionManager 是接口类型，不同的 Dao 层技术则有不同的实现类，例如：Dao 层技术是jdbc  或 mybatis 时：org.springframework.jdbc.datasource.DataSourceTransactionManager  Dao 层技术是hibernate时：org.springframework.orm.hibernate5.HibernateTransactionManage





#### **TransactionDefinition** 是事务的定义信息对象，里面有如下方法：



![image-20220505094524754](../../../blog/zheng-s/source/image/image-20220505094524754.png)



### \1. 事务隔离级别

设置隔离级别，可以解决事务并发产生的问题，如脏读、不可重复读和虚读。

ISOLATION_DEFAULT 

 ISOLATION_READ_UNCOMMITTED 

 ISOLATION_READ_COMMITTED 

 ISOLATION_REPEATABLE_READ 

 ISOLATION_SERIALIZABL



### \2. 事务传播行为

 REQUIRED：如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。一般的选择（默认值）(a调b的业务方法,b看a有事务,就用a的事务,b看a没事务,自己新建一个事务)



  SUPPORTS：支持当前事务，如果当前没有事务，就以非事务方式执行（没有事务）

(a调b的业务方法,b看a有事务,就用a的事务,b看a没事务,就非事务方式运行)



 MANDATORY：使用当前的事务，如果当前没有事务，就抛出异常

(a调b的业务方法,b看a有事务,就用a的事务,b看a没事务,异常)



 超时时间：默认值是-1，没有超时限制。如果有，以秒为单位进行设置



 是否只读：建议查询时设置为只读





#### 1.3 TransactionStatus

TransactionStatus 接口提供的是事务具体的运行状态，方法介绍如下。

![image-20220505095511270](../../../blog/zheng-s/source/image/image-20220505095511270.png)

### 1.4 知识要点 编程式事务控制三大对象 

 PlatformTransactionManager:指定行为,事务怎么控制

 TransactionDefinition :封装事务的参数,属性参数信息

第一和第二需要配置,第三个不需要我告诉spring框架

 TransactionStatus:状态信息

### 声明式:配置,由Spring帮你控制

Spring 的声明式事务顾名思义就是采用声明的方式来处理事务。这里所说的声明，就是指在配置文件中声明 ，用在 Spring 配置文件中声明式的处理事务来代替代码式的处理事务。

作用:业务代码与事务控制(系统层面)通过配置松耦合,思想是AOP思想,业务方法是切点,事务是增强,通过配置方法进行织入



#### **注意：Spring 声明式事务控制底层就是AOP**

#### 1.基于xml,以下三部分基本是配死的



 谁是切点？

```java
public void transfer(String outMan, String inMan, double money) {
    accountDao.out(outMan,money);

    accountDao.in(inMan,money);
}
```

transfer方法是切点 

```xml
<!--目标对象  内部的方法就是切点-->
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
    <property name="accountDao" ref="accountDao"/>
</bean>
```

 谁是通知(增强)？

不用自己写,spring以提供,我们需要引入对应的命名空间,配置一下

#### **<!--配置平台事务管理器-->:**

```xml
<!--配置平台事务管理器-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>//要注入dataSource,从中拿一个connection进行事务控制
```

需要配置指定,不然不知道dao层用的那个技术

```xml
<!--通知  事务的增强-->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <!--设置事务的属性信息的-->
    <tx:attributes>
        <tx:method name="transfer" isolation="REPEATABLE_READ" propagation="REQUIRED" read-only="false"/>
        <tx:method name="save" isolation="REPEATABLE_READ" propagation="REQUIRED" read-only="false"/>
        <tx:method name="findAll" isolation="REPEATABLE_READ" propagation="REQUIRED" read-only="true"/>
        <tx:method name="update*" isolation="REPEATABLE_READ" propagation="REQUIRED" read-only="true"/>
        <tx:method name="*"/>
    </tx:attributes>
</tx:advice>
```

  配置切面(配置织入)？

```xml
<!--配置事务的aop织入-->
<aop:config>
    <aop:pointcut id="txPointcut" expression="execution(* com.itheima.service.impl.*.*(..))"/>
    <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>
</aop:config>
```



#### 2.3 切点方法的事务参数的配置

![image-20220505104812139](../../../blog/zheng-s/source/image/image-20220505104812139.png)



#### 声明式事务控制的配置要点

  平台事务管理器配置

  事务通知的配置 

 事务aop织入的配置

### 2,基于注解:

```java
@Service("accountService")
@Transactional(isolation = Isolation.REPEATABLE_READ)
public class AccountServiceImpl implements AccountService {

    @Autowired
    private AccountDao accountDao;

    @Transactional(isolation = Isolation.READ_COMMITTED,propagation = Propagation.REQUIRED)
    public void transfer(String outMan, String inMan, double money) {
        accountDao.out(outMan,money);
//        int i = 1/0;
        accountDao.in(inMan,money);
    }
```

```java
@Repository("accountDao")
public class AccountDaoImpl implements AccountDao {

    @Autowired
    private JdbcTemplate jdbcTemplate;
```

```xml
<!--组件扫描-->
<context:component-scan base-package="com.itheima"/>
```

```xml
<!--事物的注解驱动-->
<tx:annotation-driven transaction-manager="transactionManager"/>
```



加上去事物的注解驱动-->,注解@Transactional(isolation = Isolation.REPEATABLE_READ)才会起作用

① 使用 @Transactional 在需要进行事务控制的类或是方法上修饰，注解可用的属性同 xml 配置方式，例如隔离 级别、传播行为等。 

② 注解使用在类上，那么该类下的所有方法都使用同一套注解参数配置。

 ③ 使用在方法上，不同的方法可以采用不同的事务参数配置。

 ④ Xml配置文件中要开启事务的注解驱动





注解声明式事务控制的配置要点 

 平台事务管理器配置（xml方式）

  事务通知的配置（@Transactional注解配置）

  事务注解驱动的配置 

 [10_声明式事务控制.pdf](E:\BaiduNetdiskDownload\笔记\第十天资料\PDF\10_声明式事务控制.pdf) 