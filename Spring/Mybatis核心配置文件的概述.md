```xml

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--通过properties标签加载外部properties文件-->
    <properties resource="jdbc.properties"></properties>

    <!--自定义别名-->
    <typeAliases>
        <typeAlias type="com.itheima.domain.User" alias="user"></typeAlias>
    </typeAliases>

    <!--数据源环境-->
    <environments default="developement">
        <environment id="developement">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    

    <!--加载映射文件-->
    <mappers>
        <mapper resource="com/itheima/mapper/UserMapper.xml"></mapper>
    </mappers>


</configuration>
```



 [01_MyBatis入门操作.pdf](E:\BaiduNetdiskDownload\笔记\第十一天资料\PDF\01_MyBatis入门操作.pdf) 



###  MyBatis常用配置解析:

#### \1. environments标签

![image-20220505201426228](../../../blog/zheng-s/source/image/image-20220505201426228.png)

其中，事务管理器（transactionManager）类型有两种：

 **• JDBC：**这个配置就是直接使用了JDBC 的提交和回滚设置，它依赖于从数据源得到的连接来管理事务作用域。

 **• MANAGED**：这个配置几乎没做什么。它从来不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（比如JEE  应用服务器的上下文）。 默认情况下它会关闭连接，然而一些容器并不希望这样，因此需要将 closeConnection 属性设置 为 false 来阻止它默认的关闭行为。 

其中，数据源（dataSource）类型有三种：

 **• UNPOOLED**：这个数据源的实现只是每次被请求时打开和关闭连接。

 **• POOLED**：这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来。

 **• JNDI**：这个数据源的实现是为了能在如 EJB 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置 一个 JNDI 上下文的引用





#### \2. mapper标签

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="userMapper">

    <!--删除操作-->
    <delete id="delete" parameterType="int">/*传递给这条语句的类型*/
        delete from user where id=#{abc}
    </delete>

    <!--修改操作-->
    <update id="update" parameterType="com.itheima.domain.User">
        update user set username=#{username},password=#{password} where id=#{id}
    </update>

    <!--插入操作-->
    <insert id="save" parameterType="com.itheima.domain.User">
        insert into user values(#{id},#{username},#{password})
    </insert>

    <!--查询操作-->
    <select id="findAll" resultType="user">
        select * from user
    </select>

    <!--根据id进行查询-->
    <select id="findById" resultType="user" parameterType="int">
        select * from user where id=#{id}
    </select>

</mapper>
```

该标签的作用是加载映射的，加载方式有如下几种：

```xml
<mappers>
    <mapper resource="com/itheima/mapper/UserMapper.xml"></mapper>
</mappers>
```

• 使用相对于类路径的资源引用，例如：<mapper resource="org/mybatis/builder/AuthorMapper.xml"/>

 • 使用完全限定资源定位符（URL），例如：<mapper url="file:///var/mappers/AuthorMapper.xml"/>

 • 使用映射器接口实现类的完全限定类名，例如:<mapper              **注解常用**class="org.mybatis.builder.AuthorMapper"/>

： • 将包内的映射器接口实现全部注册为映射器，例如：<package name="org.mybatis.builder"/>







#### \3. Properties标签

```xml
<!--通过properties标签加载外部properties文件-->
<properties resource="jdbc.properties"></properties>
```

```xml
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/test
jdbc.username=root
jdbc.password=sz152261
```

![image-20220505202443002](../../../blog/zheng-s/source/image/image-20220505202443002.png)





#### \4. typeAliases标签:(定义别名)

![image-20220505202730600](../../../blog/zheng-s/source/image/image-20220505202730600.png)

![image-20220505202811610](../../../blog/zheng-s/source/image/image-20220505202811610.png)