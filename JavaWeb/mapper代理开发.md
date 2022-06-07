

# 企业中更加常见,进一步简化了mybatis

### 之前:

![image](https://user-images.githubusercontent.com/65000660/172345775-09e7a5c1-9718-454e-adc6-d405d52122ab.png)

这里这个test.selectAll又出现了硬编码的问题

### 之后:
![image](https://user-images.githubusercontent.com/65000660/172345795-cd640ccb-9563-493c-927d-3f148781144f.png)





### mapper代理开发案例步骤:

![image](https://user-images.githubusercontent.com/65000660/172346532-7549cc2e-4313-48a9-aaca-51232a4c9d5e.png)






第一步要在resources下面的配置文件xml新建一个文件架(因为这里没有包),用斜杠表示分隔符,将xml文件放在了mapper接口同一个文件架下:

![image](https://user-images.githubusercontent.com/65000660/172346531-b1b4cc88-012e-4b07-8b11-019006c2d49a.png)

### 复盘流程:

获取sqlsession对象,通过sqlSession.getMapper获取mapper接口的代理对象,找到接口的定义类,然后因为xml映射文件放在了mapper接口同一个文件架下,然后找到xml里面的sql语句



然后usermapper.selectAll调用方法,这个方法对应sql语句的一个id

![image](https://user-images.githubusercontent.com/65000660/172346530-22b7bb88-2993-427a-a479-a92b64591fd8.png)

返回值是个list容器放了很多查询的结果的封装对象

### mybatis核心配置文件



![image](https://user-images.githubusercontent.com/65000660/172346592-baa8f586-867c-47a8-b981-fd6dc5d3d73d.png)
