 [02_MyBatis的Dao层实现方式.pdf](E:\BaiduNetdiskDownload\笔记\第十二天资料\PDF\02_MyBatis的Dao层实现方式.pdf) 



之前都是单元测试进行的,但是真正在项目中都是在Dao层写的,所以看看它在Dao层s的实现

### 传统开发:

\1. 编写UserDao接口

\2. 编写UserDaoImpl实现

\3. 测试传统方式

### 接口代理开发:(我写接口,不写实现,让mybatis替我搞个代理对象实现想用功能)

采用 Mybatis 的代理开发方式实现 DAO 层的开发，这种方式是我们后面进入企业的主流。 Mapper 接口开发方法只需要程序员编写Mapper 接口（相当于Dao 接口），由Mybatis 框架根据接口定义创建接 口的动态代理对象，代理对象的方法体同上边Dao接口实现类方法。

#### Mapper 接口开发需要遵循以下规范：

 1、 Mapper.xml文件中的namespace与mapper接口的全限定名相同

 2、 Mapper接口方法名和Mapper.xml中定义的每个statement的id相同

 3、 Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql的parameterType的类型相同 

4、 Mapper接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType的类型相同

![image](https://user-images.githubusercontent.com/65000660/172402817-fa714945-5402-4b18-bda7-f319b66ab5d5.png)


```java
UserMapper mapper = sqlSession.getMapper(UserMapper.class);
```

这句话获得了一个代理对象:



#### 映射文件:Mapper.xml

```xml
<mapper namespace="com.itheima.dao.UserMapper">

    <!--查询操作-->
    <select id="findAll" resultType="user">
        select * from user
    </select>

    <!--根据id进行查询-->
    <select id="findById" parameterType="int" resultType="user">
        select * from user where id=#{id}
    </select>

</mapper>
```



#### 接口得定义:(没有接口得实现类)

```java
import java.io.IOException;
import java.util.List;
//我这个接口没实现.是mybtis帮我建立一个代理对象,建立的方法就得符合那四步
public interface UserMapper {

    public List<User> findAll() throws IOException;
    public User findById(int id);



}
```

#### 测试(service层:用一个模块模拟service层)

```java
public static void main(String[] args) throws IOException {
    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    SqlSession sqlSession = sqlSessionFactory.openSession();
   UserMapper mapper= sqlSession.getMapper(UserMapper.class);//获得代理对象
    List<User> all = mapper.findAll();
    System.out.println(all);

    User user = mapper.findById(1);
    System.out.println(user);

}
```

