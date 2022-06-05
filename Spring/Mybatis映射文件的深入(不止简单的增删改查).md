

 [03_MyBatis映射文件深入（动态SQL）.pdf](E:\BaiduNetdiskDownload\笔记\第十三天资料\PDF\03_MyBatis映射文件深入（动态SQL）.pdf) 

### \1. 动态sql语句概述 

Mybatis 的映射文件中，前面我们的 SQL 都是比较简单的，有些时候业务逻辑复杂时，我们的 SQL是动态变化的， 此时在前面的学习中我们的 SQL 就不能满足要求了。 

业务层传给dao层参数不用,sql语句也不同.



这节就是在Mapeer.xm映射文件中学习更多的标签:

#### if标签

我们根据实体类的不同取值，使用不同的 SQL语句来进行查询。比如在 id如果不为空时可以根据id查询，如果 username 不同空时还要加入用户名作为条件。这种情况在我们的多条件组合查询中经常会碰到。

```java
//模拟条件user
User condition = new User();
condition.setId(1);
condition.setUsername("zhangsan");
condition.setPassword("123");//实际这一部分是由网页操作的人点击决定的,不确定,它不确定,那Mapper.xml文件中的sql语句也是不确定的

List<User> userList = mapper.findByCondition(condition);
```

实际这一部分是由网页操作的人点击决定的,不确定,它不确定,那Mapper.xml文件中的sql语句也是不确定的

所以就用到动态sql了

```xml
<select id="findByCondition" parameterType="user" resultType="user">
    select * from user
    <where>
        <if test="id!=0">
            and id=#{id}
        </if>
        <if test="username!=null">
            and username=#{username}
        </if>
        <if test="password!=null">
            and password=#{password}
        </if>
    </where>
</select>
```



#### <foreach标签:

循环执行sql的拼接操作，例如：SELECT * FROM USER WHERE id IN (1,2,5)。

```java
public List<User> findByIds(List<Integer> ids);//传给我的是很多个id,接口中的定义方法
```

```xml
<select id="findByIds" parameterType="list" resultType="user">
    select * from user 
    <where>
        <foreach collection="list" open="id in(" close=")" item="id" separator=",">
            #{id}
        </foreach>
    </where>
</select>
```

```java
//模拟ids的数据
List<Integer> ids = new ArrayList<Integer>();
ids.add(1);
ids.add(2);

List<User> userList = mapper.findByIds(ids);
System.out.println(userList);
```

foreach标签的属性含义如下：

 标签用于遍历集合，它的属性：

 • collection：代表要遍历的集合元素，注意编写时不要写#{}

 • open：代表语句的开始部分 • close：代表结束部分

 • item：代表遍历集合的每个元素，生成的变量名

 • sperator：代表分隔符



#### sql语句的抽取:

```xml
<!--sql语句抽取-->
<sql id="selectUser">select * from user</sql>
```

用标签<include refid="selectuser"></include>





### MyBatis映射文件配置： 

查询<select   用resultType把查询到的数据封装到目标实体

<insert插入 :paramType:参数类型,从实体中获取属性的值用的是#{实体内部属性名称},比如User user

<update修改 ：

<delete删除 ：

<where:where条件 ：

<if   if判断 ：

<foreach   循环 ：常用于where 什么in什么

<sql   sql片段抽取