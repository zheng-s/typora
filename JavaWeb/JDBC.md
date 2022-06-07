![image](https://user-images.githubusercontent.com/65000660/172335159-2cc58ed2-0133-40d6-95dd-b8c76c1f6e6e.png)


![image](https://user-images.githubusercontent.com/65000660/172335180-745c92a5-063d-493d-a50e-71ba15d5fca8.png)




## JDBC就是一个接口,让各大数据库厂家来实现

### 通过JBDC你可以用java代码操作数据库


![image](https://user-images.githubusercontent.com/65000660/172335278-f09a31c3-345c-476f-9b92-6abf5df7bfc3.png)



#### 1导入mysql的对于JBDC的实现类(驱动)

#### 2注册驱动让IDEA识别

#### 3这里是获取连接,通过JBDC来操作本地计算机的mysql数据库

#### 4,定义ssql语句来实现你想要的对于数据库的操作

#### 5,获取执行sql的对象

#### 6,执行sql对象

#### 7,处理返回结果

#### 8,释放资源

### 每一步都可以查到JBDC相关的API

只需要用ＪＢＤＣ就可以用ｊａｖａ代码操作不同的数据库

![image](https://user-images.githubusercontent.com/65000660/172335318-bfe75112-7de2-49b1-9eb5-e999002529cb.png)






![image](https://user-images.githubusercontent.com/65000660/172335357-017bb3d8-a523-4e1a-8c10-8dd7da1da203.png)




JDBC连接url是用的8.....的jar包,所以要设置时区

**java.sql.SQLException: The server time zone value '�й���׼ʱ��' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the serverTimezone configuration property) to use a more specifc time zone value if you want to utilize time zone support.**







**制台洋洋洒洒几百个错误刷屏，内心居然能够波澜不惊，看来已经是见过世面的人了(另类老司机？)。**

**简单翻译一下，就是服务器时区跟数据库所用时区不一样，需要在服务器端或者JDBC驱动配置里面指定一个，否则就不给你用。**

**好吧，我投降，因为我不能不用呀。总不能改电脑的时区吧，那就怎么简单怎么来吧，在 JDBC URL 后面加个参数。?serverTimezone=UTC**

**如果你有多个参数，像我一样，就&serverTimezone=UTC**
