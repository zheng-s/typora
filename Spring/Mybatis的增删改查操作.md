### 插入:
![image](https://user-images.githubusercontent.com/65000660/172403112-c2dc276e-1335-4566-8fad-588221a09554.png)
![image](https://user-images.githubusercontent.com/65000660/172403131-97508eda-16b7-4faa-9447-bd2553b83bde.png)



#### 插入操作注意的问题:插图涉及变化事务,一气呵成
![image](https://user-images.githubusercontent.com/65000660/172403222-57b83bd8-3d47-49d1-84e5-cb48ef572173.png)



### 修改:
![image](https://user-images.githubusercontent.com/65000660/172403296-672b1eb6-0df6-4543-bcc5-ebeccb23be65.png)

![image](https://user-images.githubusercontent.com/65000660/172403314-9b7a3f2f-ad17-40d7-bd10-fa2396830359.png)


```java
//修改操作
public void test3() throws IOException {

    //模拟user对象
    User user = new User();
    user.setId(7);
    user.setUsername("lucy");
    user.setPassword("123");

    //获得核心配置文件
    InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
    //获得session工厂对象
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    //获得session回话对象
    SqlSession sqlSession = sqlSessionFactory.openSession();
    //执行操作  参数：namespace+id
    sqlSession.update("userMapper.update",user);

    //mybatis执行更新操作  提交事务
    sqlSession.commit();

    //释放资源
    sqlSession.close();
}
```

#### \3. 修改操作注意问题

 • 修改语句使用update标签 

• 修改操作使用的API是sqlSession.update(“命名空间.id”,实体对象);

#### \3. 删除操作注意问题 

• 删除语句使用delete标签 

• Sql语句中使用#{任意字符串}方式引用传递的单个参数

 • 删除操作使用的API是sqlSession.delete(“命名空间.id”,Object)





### 增删改查映射配置与API：

![image](https://user-images.githubusercontent.com/65000660/172403398-2c04b8e0-1f61-4eae-8977-a86617ec616f.png)


如果查询有条件,也会有parameterType这个参数类型,条件就是这个类型,根据id,他就是int
