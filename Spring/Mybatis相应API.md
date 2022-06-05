 [01_MyBatis入门操作.pdf](E:\BaiduNetdiskDownload\笔记\第十一天资料\PDF\01_MyBatis入门操作.pdf) 







### 1,SqlSession工厂构建器SqlSessionFactoryBuilder

常用API：SqlSessionFactory build(InputStream inputStream) 通过加载mybatis的核心文件的输入流的形式构建一个SqlSessionFactory对象 

String resource = "org/mybatis/builder/mybatis-config.xml"

InputStream inputStream = Resources.getResourceAsStream(resource); 

SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();

 SqlSessionFactory factory = builder.build(inputStream);



其中， Resources 工具类，这个类在 org.apache.ibatis.io 包中。Resources 类帮助你从类路径下、文件系统或 一个 web URL 中加载资源文件

### 2,SqlSession工厂对象SqlSessionFactory

**与数据库交互的API都在sqlsession对象里封装,所以它很重要**

### 3 SqlSession会话对象

SqlSession 实例在 MyBatis 中是非常强大的一个类。在这里你会看到所有执行语句、提交或回滚事务和获取映射器实例的方法。 执行语句的方法主要有：



 T <E>selectOne(String statement, Object parameter)  

List<E> selectList(String statement, Object parameter) 

 int insert(String statement, Object parameter)  

int update(String statement, Object parameter) 

 int delete(String statement, Object parameter)





```xml
<!--根据id进行查询-->
<select id="findById" resultType="user" parameterType="int">
    select * from user where id=#{id}
</select>
```

```java
public void test5() throws IOException {
    //获得核心配置文件
    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    //获得session工厂对象
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    //获得session回话对象
    SqlSession sqlSession = sqlSessionFactory.openSession();
    //执行操作  参数：namespace+id
    User user = sqlSession.selectOne("userMapper.findById", 1);
    //打印数据
    System.out.println(user);
    //释放资源
    sqlSession.close();
}
```



void commit() :提交事务

void rollback()回滚事务







**1,Mybatis是什么?**

**2,Mybatis快速入门,不懂得地方就详细写**

**3,配置文件,映射文件与核心文件,API来细讲**

**总之就是怎么用**