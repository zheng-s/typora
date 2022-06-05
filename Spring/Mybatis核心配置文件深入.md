 [04_MyBatis核心配置文件深入.pdf](E:\BaiduNetdiskDownload\笔记\第十四天资料\PDF\04_MyBatis核心配置文件深入.pdf) 



### 1.1 typeHandlers标签:

![image-20220506172212509](../../../blog/zheng-s/source/image/image-20220506172212509.png)





你可以重写类型处理器或创建你自己的类型处理器来处理不支持的或非标准的类型。

**具体做法为**：实现 org.apache.ibatis.type.TypeHandler 接口， 或继承一个很便利的类 org.apache.ibatis.type.BaseTypeHandler， 然 后可以选择性地将它映射到一个JDBC类型。

**例如需求：一个Java中的Date数据类型，我想将之存到数据库的时候存成一 个1970年至今的毫秒数，取出来时转换成java的Date，即java的Date与数据库的varchar毫秒值之间转换。**





一句话,有类型转换器不符合我的要求你可以覆盖掉,或者根本没有,可以自定义





```java
public class User {            //定义表存放的对象

    private int id;
    private String username;
    private String password;
    private Date birthday;
    
    }
```

```java
public interface UserMapper { //定义接口

    public void save(User user);
    }
```

```xml
<mapper namespace="com.itheima.mapper.UserMapper">//mapper.xml
   

    <insert id="save" parameterType="user">
        insert into user values(#{id},#{username},#{password},#{birthday})
    </insert>
```

得到Mybatis为我生成的代理对象

```java
    @Test
    public void test1() throws IOException {
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory sqlSessionFactory = new          SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);//得到代理对象

        //创建user
        User user = new User();
        user.setUsername("ceshi");
        user.setPassword("abc");
        user.setBirthday(new Date());//关键就在于这个date,要把java得date型转换为数据库里得bigint
        //执行保存造作
        mapper.save(user);

        sqlSession.commit();
        sqlSession.close();
    }

```

//关键就在于这个date,要把java得date型转换为数据库里得bigint,得自定义一个类型转换器:

 **开发步骤**：

 ① 定义转换类继承类BaseTypeHandler

 ② 覆盖4个未实现的方法，其中setNonNullParameter为java程序设置数据到数据库的回调方法，getNullableResult 为查询时 mysql的字符串类型转换成 java的Type类型的方法 

③ 在MyBatis核心配置文件中进行注册

 ④ 测试转换是否正确

一下就是主要步骤:



```java
package com.itheima.handler;

import org.apache.ibatis.type.BaseTypeHandler;
import org.apache.ibatis.type.JdbcType;

import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Date;

public class DateTypeHandler extends BaseTypeHandler<Date> {
    //将java类型 转换成 数据库需要的类型
    public void setNonNullParameter(PreparedStatement preparedStatement, int i, Date date, JdbcType jdbcType) throws SQLException {
        long time = date.getTime();//此方法获得时间毫秒值
        preparedStatement.setLong(i,time);
    }

    //将数据库中类型 转换成java类型
    //String参数  要转换的字段名称
    //ResultSet 查询出的结果集
    public Date getNullableResult(ResultSet resultSet, String s) throws SQLException {
        //获得结果集中需要的数据(long) 转换成Date类型 返回
        long aLong = resultSet.getLong(s);
        Date date = new Date(aLong);
        return date;
    }

    //将数据库中类型 转换成java类型
    public Date getNullableResult(ResultSet resultSet, int i) throws SQLException {
        long aLong = resultSet.getLong(i);
        Date date = new Date(aLong);
        return date;
    }

    //将数据库中类型 转换成java类型
    public Date getNullableResult(CallableStatement callableStatement, int i) throws SQLException {
        long aLong = callableStatement.getLong(i);
        Date date = new Date(aLong);
        return date;
    }
}
```



```java
@Test
public void test2() throws IOException {
    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);

    User user = mapper.findById(15);//你需要在接口与mapper.xml里定义响应语句,然后获得一个mapper代理对象放得是通过id查询得结果
    System.out.println("user中的birthday："+user.getBirthday());

    sqlSession.commit();
    sqlSession.close();
}
```

输出是个date类型说明成功:

```xml
<!--注册类型处理器-->//全限定名赋值过来
<typeHandlers>
    <typeHandler handler="com.itheima.handler.DateTypeHandler"></typeHandler>
</typeHandlers>
```







### 1.2 plugins标签:

MyBatis可以使用第三方的插件来对功能进行扩展，分页助手PageHelper是将分页的复杂操作进行封装，使用简单的方式即 可获得分页的相关数据

 **开发步骤：**

 ① 导入通用PageHelper的坐标 

② 在mybatis核心配置文件中配置PageHelper插件 

③ 测试分页数据获

![image-20220506200155853](../../../blog/zheng-s/source/image/image-20220506200155853.png)



![image-20220506200210952](../../../blog/zheng-s/source/image/image-20220506200210952.png)



![image-20220506200226116](../../../blog/zheng-s/source/image/image-20220506200226116.png)











### MyBatis核心配置文件常用标签：

 1、properties标签：该标签可以加载外部的properties文件 

2、typeAliases标签：设置类型别名

 3、environments标签：数据源环境配置标签

4、typeHandlers标签：配置自定义类型处理器 5、plugins标签：配置MyBatis的插件