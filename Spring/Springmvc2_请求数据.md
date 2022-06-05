### 获得restful风格的擦参数:

Restful是一种软件架构风格、设计风格，而不是标准，只是提供了一组设计原则和约束条件。主要用于客户端和服务 器交互类的软件，基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存机制等。

Restful风格的请求是使用“url+请求方式”表示一次请求目的的，HTTP 协议里面四个表示操作方式的动词如下：



![image-20220430210116300](../../../blog/zheng-s/source/image/image-20220430210116300.png)



### 自定义类型转换器:



• SpringMVC 默认已经提供了一些常用的类型转换器，例如客户端提交的字符串转换成int型进行参数设置。 • 但是不是所有的数据类型都提供了转换器，没有提供的就需要自定义转换器，例如：日期类型的数据就需要自 定义转换器。 自定义类型转换器的开发步骤： ① 定义转换器类实现Converter接口 ② 在配置文件中声明转换器 ③ 在中引用转换

![image-20220502161608023](../../../blog/zheng-s/source/image/image-20220502161608023.png)

![image-20220502161622688](../../../blog/zheng-s/source/image/image-20220502161622688.png)

### servlet相关API:

![image-20220502161711561](../../../blog/zheng-s/source/image/image-20220502161711561.png)





获得请求头:

![image-20220502161830525](../../../blog/zheng-s/source/image/image-20220502161830525.png)

![image-20220502161843752](../../../blog/zheng-s/source/image/image-20220502161843752.png)





### 文件上传:



![image-20220502161922308](../../../blog/zheng-s/source/image/image-20220502161922308.png)





![image-20220502161939418](../../../blog/zheng-s/source/image/image-20220502161939418.png)





![image-20220502161957677](../../../blog/zheng-s/source/image/image-20220502161957677.png)



![image-20220502162026158](../../../blog/zheng-s/source/image/image-20220502162026158.png)





![image-20220502162041135](../../../blog/zheng-s/source/image/image-20220502162041135.png)





![image-20220502162055664](../../../blog/zheng-s/source/image/image-20220502162055664.png)



![image-20220502162121579](../../../blog/zheng-s/source/image/image-20220502162121579.png)
