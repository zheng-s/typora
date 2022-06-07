# IO流分类

java为我们提供了多种多样的IO流，我们可以根据不同的功能及性能要求挑选合适的IO流，如图10-7所示，为Java中IO流类的体系。

能使用字符流的地方可以使用字节,使用字节的地方不一定使用字符



输入输出是以人(程序为中心的)

![image](https://user-images.githubusercontent.com/65000660/172332880-fcb41813-da5f-42f5-b877-ec1a17c93997.png)




流的功能不一样,有的直接连在源头文件上,处于第一线的叫做节点流,在其基础上进行扩展包装称为处理流

名字有字节数组Byte与文件FIle一般称为节点流

**1,2总共是四个抽象类,InputStream/OutputStream和Reader/writer类是所有IO流类的抽象父类**

## 一.InputStream/OutputStream 字节流的抽象类。

数据单位是字节,一切东西都可以变为字节,通过程序的读入和输出可以实现拷贝



### 1. FileInputStream/FileOutputStream,节点流：以字节为单位直接操作“文件”。



### 2.ByteArrayInputStream/ByteArrayOutputStream,节点流：以字节为单位直接操作“字节数组对象”。



### 3.ObjectInputStream/ObjectOutputStream处理流：以字节为单位直接操作“对象”。



### 4.DataInputStream/DataOutputStream处理流：以字节为单位直接操作“基本数据类型与字符串类型

## 二.Reader/Writer字符流的抽象类

数据单位是字符

### 1.FileReader/FileWriter节点流：以字符为单位直接操作“文本文件”(注意：只能读写文本文件)。





### 2. BufferedReader/BufferedWriter处理流：将Reader/Writer对象进行包装，增加缓存功能，提高读写效率。







###  3.BufferedInputStream/BufferedOutputStream处理流：将InputStream/OutputStream对象进行包装，增加缓存功能，提高 读写效率。







### 4.InputStreamReader/OutputStreamWriter处理流：将字节流对象转化成字符流对象。







### 5.1PrintStream处理流：将OutputStream进行包装，可以方便地输出字符，更加灵活。
