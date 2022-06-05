# 构造方法

```java
package port4;

public class TestGouZao {
    public static void main(String[] args) {
        MagicBox box=new MagicBox(50,60,30);//这里就相当于认为赋值
        System.out.println("length:"+box.length);
        System.out.println("width:"+box.width);
        System.out.println("height:"+box.height);
         MagicBox box2=new MagicBox();
        /*
        在没有定义构造方法之前,MagicBox box2=new MagicBox()是可以被使用的,但我们已经
        定义了一个带参数列表的构造方法,发现原来的写法不适用了
        因为在java中一旦用户定义了构造方法,系统就不再提供默认的构造方法了
         */
    }

    /*
    在java语言中,构造方法用于创建对象的升级后进行初始化操作
    如Preson p=new person();
    此处的person()就是一个构造方法,构造方法默认是不可见的
    初始化数值类型为0;bool类型设置为true
    在没有自己定义构造方法的时候,在用的是系统提供的一个无参数的默认的构造方法
    public 类名(){}
     */
}

class MagicBox{    //定义一个描述盒子的类
    public int length;
    public  int width;
    public  int height;
    //此处定一个构造方法,用于对盒子的初始化
    //定一个一有参数的方法,用于接受传递进来的初始化参数
    //构造方法无返回值类型,只是一个初始化的操作
    public MagicBox(int length,int width,int height){
        this.length=length;
        this.width=width;
        this.height=height;
        //将外界传的参数赋值给了类内部的属性,用this区分

    }
    public MagicBox(){
        /*
        /这个一补上,第九行就不报错了,此构造方法等同于系统提供的默认的,
        只不过在用户为自定义构造方法之前.这个方法由系统切不可见,如果用户自定义的与原来系统默认的一致,原来的构造方法失效
        一个类中可以有多个构造方法,此时就形成了构造方法的重载
        若果使用自定义的构造方法,建议将构造方法的访问权限设为public
         */
    }
}
```

# 构造方法的重载与引用

```java
package port4;

public class TestGouZaoChongZai {

    public static void main(String[] args) {
        magicBox box = new magicBox(50, 60, 30);//这里就相当于认为赋值
        System.out.println("length:" + box.length);
        System.out.println("width:" + box.width);
        System.out.println("height:" + box.height);
        magicBox box2 = new magicBox();
    }
}


class magicBox {    //定义一个描述盒子的类
    public int length;
    public int width;
    public int height;
//----------------------------------------------------------------------------------
    public magicBox(int length, int width) {//下面的可以调用上面的
        this.length = length;
        this.width = width;
        //如果一个构造方法的结构能够包含另一个构造方法的结构,那么他们之间存在可以调用的关系

    }
 //------------------------------------------------------------------------------
    public magicBox(int length, int width, int height) {
        //可以使用this关键字对构造方法进行调用,且this调用构造方法的时候必须放在第一行
        this(length,width);
        this.height = height;
        //如果一个构造方法的结构能够包含另一个构造方法的结构,那么他们之间存在可以调用的关系
    }
 //-----------------------------------------------------------------------------
    public magicBox(){

    }
    /*public void print(){
        this           在普通方法中是无法引用构造方法的
                    对构造方法的引用只能出现在构造方法中
    */
}

/*
---------------------------------------------------------------------------------------
public magicBox(int length, int width) {//下面的可以调用上面的
        this(10,20,30);引用了下面的构造方法,错误,不可以相互引用
        this.length = length;
        this.width = width;
        //如果一个构造方法的结构能够包含另一个构造方法的结构,那么他们之间存在可以调用的关系

    }
    public magicBox(int length, int width, int height) {
        //可以使用this关键字对构造方法进行调用,且this调用构造方法的时候必须放在第一行
        this(length,width);//引用了上面的构造方法
        this.height = height;
        //如果一个构造方法的结构能够包含另一个构造方法的结构,那么他们之间存在可以调用的关系
    }
 */
```

