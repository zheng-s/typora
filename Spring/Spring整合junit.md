在测试类中，每个测试方法都有以下两行代码： 

ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml"); 

IAccountService as = ac.getBean("accountService",IAccountService.class); 

这两行代码的作用是获取容器，如果不写的话，直接会提示空指针异常。所以又不能轻易删掉。

### • 让SpringJunit负责创建Spring容器，但是需要将配置文件的名称告诉它

![image](https://user-images.githubusercontent.com/65000660/172419051-fcd02d2f-f22a-4f4b-9dbc-017aa0bd50c0.png)





#### (1)导入(Spring集成junit)的坐标:

#### (2)

![image](https://user-images.githubusercontent.com/65000660/172419084-bcf69ed1-d127-45b5-a4a5-06ef75623112.png)

#### (3)

![image](https://user-images.githubusercontent.com/65000660/172419123-8c1fe758-644f-4274-993a-60f48af82ea3.png)

#### (4)

![image](https://user-images.githubusercontent.com/65000660/172419158-d11dc627-64d6-4614-8164-3f3afda08908.png)

#### (5)

![image](https://user-images.githubusercontent.com/65000660/172419194-78aa6a39-e83c-4d82-be0c-9cc2d36f3a69.png)



###  • 将需要进行测试Bean直接在测试类中进行注入
