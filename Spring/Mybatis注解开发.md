 [06_MyBatis注解开发.pdf](E:\BaiduNetdiskDownload\笔记\第十六天资料\PDF\06_MyBatis注解开发.pdf) 



### MyBatis的常用注解(简单开发)

其实就是Mapper.xml的映射文件减少了

@Insert：实现新增 

@Update：实现更新

 @Delete：实现删除 

@Select：实现查询 

@Result：实现结果集封装

 @Results：可以与@Result 一起使用，封装多个结果集

 @One：实现一对一结果集封装 :assocation标签

@Many：实现一对多结果集封装 :collection标签



#### 第一步:在核心配置文件中把映射关系那个xml删除了,但是还是得夹加载映射关系

修改MyBatis的核心配置文件，我们使用了注解替代的映射文件，所以我们只需要加载使用了注解的Mapper接口即可:

或者指定扫描包含映射关系的接口所在的包也可以

```xml
<!--加载映射关系-->
<mappers>
    <!--指定接口所在的包-->
    <package name="com.itheima.mapper"></package>
</mappers>
```



```xml
<mappers>
<!--扫描使用注解的类-->
<mapper class="com.itheima.mapper.UserMapper"></mapper>
</mappers>
```



#### 第二部:在UserMapper接口里的方法(对应sql语句)上直接上注解

```java
public interface UserMapper {

    @Insert("insert into user values(#{id},#{username},#{password},#{birthday})")
    public void save(User user);

    @Update("update user set username=#{username},password=#{password} where id=#{id}")
    public void update(User user);

    @Delete("delete from user where id=#{id}")
    public void delete(int id);

    @Select("select * from user where id=#{id}")
    public User findById(int id);

    @Select("select * from user")
    public List<User> findAll();
}
```



**测试语句还是那些**





### MyBatis的注解实现复杂映射开发:

#### 一对一查询:

```java
public class Order {

    private int id;
    private Date ordertime;
    private double total;

    //当前订单属于哪一个用户
    private User user;
}
```

```java
public class User {

    private int id;
    private String username;
    private String password;
    private Date birthday;

    //当前用户具备哪些角色
    private List<Role> roleList;
}
```

```java
@Select("select *,o.id oid from orders o,user u where o.uid=u.id")
    @Results({//相当于resultMap标签
            @Result(column = "oid",property = "id"),
            @Result(column = "ordertime",property = "ordertime"),
            @Result(column = "total",property = "total"),
            @Result(column = "uid",property = "user.id"),
            @Result(column = "username",property = "user.username"),
            @Result(column = "password",property = "user.password")
    })
    public List<Order> findAll();
//这是写再orderMapper接口中的方法
```

```java
@Select("select * from orders")//这是第二种写法,查一张,再查询另一张
@Results({//<resultMap>标签
        @Result(column = "id",property = "id"),
        @Result(column = "ordertime",property = "ordertime"),
        @Result(column = "total",property = "total"),
        @Result(
                property = "user", //要封装的属性名称
                column = "uid", //根据那个字段去查询user表的数据
                javaType = User.class, //要封装的实体类型
                //select属性 代表查询哪个接口的方法获得数据
                one = @One(select = "com.itheima.mapper.UserMapper.findById")
        )
})
public List<Order> findAll();
```

#### 一对多查询:一个用户的订单

##### 1user里添加private List<Order> orderList;

##### 2在OrderMapper接口里键一个方法:

```java
@Select("select * from orders where uid=#{uid}")
public List<Order> findByUid(int uid);
```

##### 3再Usermapper接口中创建一个新的方法:用到了上面的方法

```java
@Select("select * from user")
@Results({
        @Result(id=true ,column = "id",property = "id"),
        @Result(column = "username",property = "username"),
        @Result(column = "password",property = "password"),
        @Result(
                property = "orderList",
                column = "id",
                javaType = List.class,
                many = @Many(select = "com.itheima.mapper.OrderMapper.findByUid")
        )
})
public List<User> findUserAndOrderAll();
```



#### 多对多查询:

##### 1,创建Role实体，修改User实体

```java
public class User {
private int id;
private String username;
private String password;
private Date birthday;
//代表当前用户具备哪些订单
private List<Order> orderList;
//代表当前用户具备哪些角色
private List<Role> roleList;
}
```





```java
public class Role {
private int id;
private String rolename;
}

```



##### 2添加UserMapper接口方法

```java
package com.itheima.mapper;

import com.itheima.domain.Role;
import org.apache.ibatis.annotations.Select;

import java.util.List;

public interface RoleMapper {

    @Select("SELECT * FROM sys_user_role ur,sys_role r WHERE ur.roleId=r.id AND ur.userId=#{uid}")
    public List<Role> findByUid(int uid);

}
```

List findAllUserAndRole()



##### 3,UserMapper接口方法里加注解

```java
@Select("SELECT * FROM USER")
@Results({
        @Result(id = true,column = "id",property = "id"),
        @Result(column = "username",property = "username"),
        @Result(column = "password",property = "password"),
        @Result(
                property = "roleList",
                column = "id",
                javaType = List.class,
                many = @Many(select = "com.itheima.mapper.RoleMapper.findByUid")
        )
})
public List<User> findUserAndRoleAll();
```

多对多就是多了一张中间表,体现在**2那里**SELECT * FROM sys_user_role ur,sys_role r WHERE ur.roleId=r.id AND ur.userId=#{uid}