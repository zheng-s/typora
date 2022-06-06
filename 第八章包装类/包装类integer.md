但是我们在实际应用中经常需要将基本数据转化成对象，以便于操作。比如：将基本数据类型存储到Object[]数组或集合中的操作等等。

   为了解决这个不足，Java在设计类时为每个基本数据类型设计了一个对应的类进行代表，这样八个和基本数据类型对应的类统称为包装类(Wrapper Class)。





 包装类均位于java.lang包，八种包装类和基本数据类型的对应关系如表8-1所示：

![image](https://user-images.githubusercontent.com/65000660/172188615-14e0d843-33df-45ed-ba01-8135a858cf2b.png)







```java
public` `class` `WrapperClassTest {
  ``public` `static` `void` `main(String[] args) {
    ``Integer i = ``new` `Integer(``10``);
    ``Integer j = ``new` `Integer(``50``);
  ``}
}
```





![image](https://user-images.githubusercontent.com/65000660/172188659-ec6de774-8b65-4467-b8d6-131bd6a35aeb.png)








**对于包装类来说，这些类的用途主要包含两种：**

   \1. 作为和基本数据类型对应的类型存在，方便涉及到对象的操作，如Object[]、集合等的操作。

   \2. 包含每种基本数据类型的相关属性如最大值、最小值等，**以及相关的操作方法(这些操作方法的作用是在基本数据类型、包装类对象、字符串之间提供相互之间的转化!)。**
