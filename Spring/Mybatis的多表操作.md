 [05_MyBatis多表操作.pdf](E:\BaiduNetdiskDownload\笔记\第十五天资料\PDF\05_MyBatis多表操作.pdf) 



### 1.1 一对一查询

用户表和订单表的关系为，一个用户有多个订单，一个订单只从属于一个用户 一对一查询的需求：查询一个订单，与此同时查询出该订单所属的用户



```xml
<resultMap id="orderMap" type="com.itheima.domain.Order">
    <!--手动指定字段与实体属性的映射关系-->
    <id column="oid" property="id"></id><!--id标签代表主键-->
    <result column="ordertime" property="ordertime"></result>
    <result column="total" property="total"></result>
    <association property="user" javaType="com.itheima.domain.User">
        <result column="uid" property="id"></result>
        <result column="username" property="username"></result>
        <result column="password" property="password"></result>
        <result column="birthday" property="birthday"></result>
    </association>
</resultMap>



    <select id="findAll" resultMap="orderMap">
        SELECT *,o.id oid  FROM `order` o,`user` u WHERE o.uid=u.id
    </select>
```

**精髓就在此,多表查询主要哦就是在查询结果时没法自动的对上号,手动封一个map,认为的让表的字段与实体的属性对上号**





### 1.2 一对多查询

user对order是一对多的,一个用户可以有多个订单,我要查用户和当前用户所有的订单

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.mapper.UserMapper">

    <resultMap id="userMap" type="user">
        <id column="id" property="id"></id>
        <result column="username" property="username"></result>
        <result column="password" property="password"></result>
        <result column="birthday" property="birthday"></result>
        <!--配置集合-->
        <collection property="orderList" ofType="order">
            <id column="oid" property="id"></id>
            <result column="oid" property="id"></result>
            <result column="ordertime" property="ordertime"></result>
            <result column="total" property="total"></result>

        </collection>
    </resultMap>

    <select id="findAll" resultMap="userMap">
       SELECT *,o.id oid FROM `user` u,`order` o WHERE u.id=o.uid
    </select>
</mapper>
```



主文件起个别名

```xml
<!--自定义别名-->
<typeAliases>
    <typeAlias type="com.itheima.domain.User" alias="user"></typeAlias>
    <typeAlias type="com.itheima.domain.Order" alias="order"></typeAlias>

</typeAliases>
```





### 1.3 多对多查询:id和result都是映射单列值到一个属性或字段的简单数据类型。id作为唯一标识主键

多对多的建表原则需要引入一个中间表

![image-20220507110045159](../../../blog/zheng-s/source/image/image-20220507110045159.png)

中间表维护着两个外键\:

#### 1,创建role实体

```java
package com.itheima.domain;

public class Role {
    private int id;
    private String roleName;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getRoleName() {
        return roleName;
    }

    public void setRoleName(String roleName) {
        this.roleName = roleName;
    }

    @Override
    public String toString() {
        return "Role{" +
                "id=" + id +
                ", roleName='" + roleName + '\'' +
                '}';
    }
}
```

#### 2添加UserMapper接口方法 :修改user实体,把roleList加进去

List findAllUserAndRole()

```java
public class User {
    private int id;
    private String username;
    private String password;
    private Date birthday;

    //描述用户存在那些订单
    private List<Order> orderList;
    private List<Role> roleList;
```

#### 3,配置userMapper.xml:

```xml
<resultMap id="userRoleMpap" type="user">
    <!--user的信息.再封装user内部的roleList信息-->
    <id column="userId" property="id"></id>
    <result column="username" property="username"></result>
    <result column="password" property="password"></result>
    <result column="birthday" property="birthday"></result>
    <collection property="roleList" ofType="role">
        <result column="roleId" property="id"></result>
        <result column="roleName" property="roleName"></result>
    </collection>

</resultMap>
<select id="findAllUserAndRole" resultMap="userRoleMpap">
    SELECT * from user u,sys_user_role ur,sys_role r WHERE u.id=ur.userId and ur.roleId=r.id
</select>
```





#### 4测试:

```java
@Test
public void test2() throws IOException {
    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();
    UserMapper mapper=sqlSession.getMapper(UserMapper.class);

    List<User> roleList = mapper.findAllUserAndRole();
    for (User user : roleList) {
        System.out.println(user);
    }
}
```





### MyBatis多表配置方式： 

一对一配置：使用<result做配置

一对多配置：使用<resultMap+<co'l'lection做配置 

多对多配置：使用<resultMap+<co'l'lection+做配置