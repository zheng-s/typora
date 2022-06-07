### 获得restful风格的擦参数:

Restful是一种软件架构风格、设计风格，而不是标准，只是提供了一组设计原则和约束条件。主要用于客户端和服务 器交互类的软件，基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存机制等。

Restful风格的请求是使用“url+请求方式”表示一次请求目的的，HTTP 协议里面四个表示操作方式的动词如下：



![image](https://user-images.githubusercontent.com/65000660/172417255-84c969f5-efce-4e97-bf36-64f4de23adcb.png)



### 自定义类型转换器:



• SpringMVC 默认已经提供了一些常用的类型转换器，例如客户端提交的字符串转换成int型进行参数设置。 • 但是不是所有的数据类型都提供了转换器，没有提供的就需要自定义转换器，例如：日期类型的数据就需要自 定义转换器。 自定义类型转换器的开发步骤： ① 定义转换器类实现Converter接口 ② 在配置文件中声明转换器 ③ 在中引用转换

![image](https://user-images.githubusercontent.com/65000660/172417347-bb6f194e-47b1-403d-8b7f-25e4c3ae3288.png)
![image](https://user-images.githubusercontent.com/65000660/172417377-623fef85-f926-4807-9f47-6309a9669106.png)


### servlet相关API:

![image](https://user-images.githubusercontent.com/65000660/172417419-20b9ff24-7001-44b7-8775-f12ceab4e2fa.png)





获得请求头:

![image](https://user-images.githubusercontent.com/65000660/172417471-825c53ee-f747-4a3f-ade5-407b71975e23.png)

![image](https://user-images.githubusercontent.com/65000660/172417499-5b39e4cd-1f2c-40f8-b223-d9eaf78af99a.png)



### 文件上传:



![image](https://user-images.githubusercontent.com/65000660/172417535-c283619c-086b-4ee5-86b8-f6887eb8eb1e.png)





![image](https://user-images.githubusercontent.com/65000660/172417561-37375dc5-c2e0-486b-8aad-dc656a7898ee.png)





![image](https://user-images.githubusercontent.com/65000660/172417594-53f8ef41-158c-45da-b699-9ccff19b3858.png)



![image](https://user-images.githubusercontent.com/65000660/172417640-50028e2d-267e-4e78-9e34-a2fd4d48675c.png)





![image](https://user-images.githubusercontent.com/65000660/172417684-63983c58-4ebf-4035-917a-5f0778426c84.png)





![image](https://user-images.githubusercontent.com/65000660/172417715-e7a993b9-1f19-47c3-917f-6290209a4f5a.png)



![image](https://user-images.githubusercontent.com/65000660/172417753-2e5bc124-af76-4012-9752-11c5688a2b18.png)
