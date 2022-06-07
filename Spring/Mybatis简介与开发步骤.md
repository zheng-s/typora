 [01_MyBatis入门操作.pdf](E:\BaiduNetdiskDownload\笔记\第十一天资料\PDF\01_MyBatis入门操作.pdf) 

### 原始jdbc操作:

**这段代码一般在dao层,持久层,在一般开发中,层与层之间的数据传递通过实体pojo对象,或者叫做javabean实体,取出结果集数据封装为user实体,user对象返回业务层**
![image](https://user-images.githubusercontent.com/65000660/172404436-8c3799f2-83f2-4af7-af12-9481268fb2c7.png)
![image](https://user-images.githubusercontent.com/65000660/172404458-dda4e488-c8e4-4cc6-93ad-8040af957fca.png)



很多代码都是重复的,把变化的代码自己写,其他的抽取

分析:资源用就造,用完就毁,消耗资源,拖累速度

#### 原始jdbc开发存在的问题如下：

 ① 数据库连接创建、释放频繁造成系统资源浪费从而影响系统性能

 ② sql 语句在代码中硬编码，造成代码不易维护，实际应用 sql 变化的可能较大，sql 变动需要改变java代码。

配置文件解耦合

 ③ 查询操作时，需要手动将结果集中的数据手动封装到实体中。插入操作时，需要手动将实体的数据设置到sql语句的占位 符位置 

#### 应对上述问题给出的解决方案：

 ① 使用数据库连接池初始化连接资源 

② 将sql语句抽取到xml配置文件中 

③ 使用反射、内省等底层技术，自动将实体与表进行属性与字段的自动映射







### mybatis简介:(借车还车,不用造车)

![image](https://user-images.githubusercontent.com/65000660/172404523-e52df8fb-004e-4256-913b-99d9691c55bf.png)




#### mybatis解决了这个东西,是持久层的框架

 mybatis 是一个优秀的基于java的持久层框架，它内部封装了 jdbc，使开发者只需要关注sql语句本身，而不需要花费精力 去处理加载驱动、创建连接、创建statement等繁杂的过程。

 mybatis通过xml或注解的方式将要执行的各种 statement配 置起来，并通过java对象和statement中sql的动态参数进行 映射生成最终执行的sql语句。

 最后mybatis框架执行sql并将结果映射为**java对象**并返回。采 用ORM(对象与数据表关系映射)思想解决了实体和数据库映射的问题，对jdbc 进行了 封装，屏蔽了jdbc api 底层访问细节，使我们不用与jdbc api 打交道，就可以完成对数据库的持久化操作





#### mybatis快速入门:

官网:[mybatis – MyBatis 3 | Introduction](https://mybatis.org/mybatis-3/)

##### ① 添加MyBatis的坐标 

porm.xml

mysql驱动

mybatis

引入junit

```xml
<dependencies>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.32</version>
    </dependency>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.4.6</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
</dependencies>
```

##### ② 创建user数据表 (操作实体从而操作表,操作表后的数据封装为实体,比如session对象)

![image](https://user-images.githubusercontent.com/65000660/172404603-cd356538-50d4-407d-8baa-b0b4ed81c293.png)


##### ③ 编写User实体类 

###### user对应什么属性一般是与表对应的:

```java
package com.itheima.domain;

public class User {

    private int id;
    private String username;
    private String password;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
}
```

##### ④ 编写映射文件UserMapper.xml 

namesapce结合id组成一个调用的名字在java代码中出现

写映射关系,sql语句

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="userMapper">

    <!--删除操作-->
    <delete id="delete" parameterType="int">
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

##### ⑤ 编写核心文件SqlMapConfig.xml 

配置mybatis核心内容的

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

##### ⑥ 编写测试类

```java
 @Test
    //查询操作
    public void test1() throws IOException {
        //获得核心配置文件
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        //获得session工厂对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        //获得session回话对象
        SqlSession sqlSession = sqlSessionFactory.openSession();
        //执行操作  参数：namespace+id
        List<User> userList = sqlSession.selectList("userMapper.findAll");
        //打印数据
        System.out.println(userList);
        //释放资源
        sqlSession.close();
    }
```





### mybatis的映射文件概述:

![image](https://user-images.githubusercontent.com/65000660/172404666-8c00542e-80cf-492e-a7a4-5bb57fed87fc.png)
