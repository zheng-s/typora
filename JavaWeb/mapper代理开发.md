

# 企业中更加常见,进一步简化了mybatis

### 之前:

![image-20220418112046915](../../../blog/zheng-s/source/image/image-20220418112046915.png)

这里这个test.selectAll又出现了硬编码的问题

### 之后:

![image-20220418112239020](../../../blog/zheng-s/source/image/image-20220418112239020.png)





### mapper代理开发案例步骤:

![image-20220418112330165](../../../blog/zheng-s/source/image/image-20220418112330165.png)





第一步要在resources下面的配置文件xml新建一个文件架(因为这里没有包),用斜杠表示分隔符,将xml文件放在了mapper接口同一个文件架下:

![image-20220418113359003](../../../blog/zheng-s/source/image/image-20220418113359003.png)

### 复盘流程:

获取sqlsession对象,通过sqlSession.getMapper获取mapper接口的代理对象,找到接口的定义类,然后因为xml映射文件放在了mapper接口同一个文件架下,然后找到xml里面的sql语句



然后usermapper.selectAll调用方法,这个方法对应sql语句的一个id

![image-20220418113829161](../../../blog/zheng-s/source/image/image-20220418113829161.png)

返回值是个list容器放了很多查询的结果的封装对象

### mybatis核心配置文件



![image-20220418160720498](../../../blog/zheng-s/source/image/image-20220418160720498.png)