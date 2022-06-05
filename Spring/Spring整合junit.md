在测试类中，每个测试方法都有以下两行代码： 

ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml"); 

IAccountService as = ac.getBean("accountService",IAccountService.class); 

这两行代码的作用是获取容器，如果不写的话，直接会提示空指针异常。所以又不能轻易删掉。

### • 让SpringJunit负责创建Spring容器，但是需要将配置文件的名称告诉它

![image-20220427162137606](../../../blog/zheng-s/source/image/image-20220427162137606.png)





#### (1)导入(Spring集成junit)的坐标:

#### (2)

![image-20220427163500065](../../../blog/zheng-s/source/image/image-20220427163500065.png)

#### (3)

![image-20220427163605388](../../../blog/zheng-s/source/image/image-20220427163605388.png)

#### (4)

![image-20220427163619477](../../../blog/zheng-s/source/image/image-20220427163619477.png)

#### (5)

![image-20220427163633589](../../../blog/zheng-s/source/image/image-20220427163633589.png)



###  • 将需要进行测试Bean直接在测试类中进行注入