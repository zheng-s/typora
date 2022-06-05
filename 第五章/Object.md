Object类是所有Java类的根基类，也就意味着所有的Java对象都拥有Object类的属性和方法。如果在类的声明中未使用extends关键字指明其父类，则默认继承Object类。

```java
public class Person {
    ...
}
//等价于：
public class Person extends Object {
    ...
}
```

其中obje类中有个toString方法可以用

```java
public String toString() {``  ``return getClass().getName() + "@" + Integer.toHexString(hashCode());``}
```



   根据如上源码得知，默认会返回“类名+@+16进制的hashcode”。在打印输出或者用字符串连接对象时，会自动调用该对象的toString()方法。

所以在输出对象时返回的时hashcode(可以理解为地址)





Object类中还定义的equal方法.表达对象内容相等的逻辑,我们也经常重写equal()方法

