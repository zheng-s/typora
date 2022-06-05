ArrayList底层使用Object数组来存储元素数据。所有的方法，都围绕这个核心的Object数组来开展。

我们知道，数组长度是有限的，而ArrayList是可以存放任意数量的对象，长度不受限制，那么它是怎么实现的呢?本质上就是通过定义新的更大的数组，将旧数组中的内容拷贝到新数组，来实现扩容。 ArrayList的Object数组初始化长度为10，如果我们存储满了这个数组，需要存储第11个对象，就会定义新的长度更大的数组，并将原数组内容和新的元素一起加入到新数组中，

 由于List、Set是Collection的子接口，意味着所有List、Set的实现类都有上面的方

### ArrayList实现类的底层理解(实现List接口)

```java
package port9;

/**
 * 自写ArraList的底层实现
 */
public class TestArrayListYuanMa {
    private Object[] elementDate;
    private  int size;
    private static final  int DEFUALT_CAPACITY=10;

    public TestArrayListYuanMa(){
        elementDate=new Object[DEFUALT_CAPACITY];
     }

    public TestArrayListYuanMa(int capacity) {  //构造方法重载

        elementDate=new Object[capacity];
    }
    public void add(Object obj){
        elementDate[size++]=obj;
    }
    public String toString(){
        StringBuilder sb=new StringBuilder();
        sb.append("[");
        for(int i=0;i<size;i++){
            sb.append(elementDate[i]+",");
        }
        sb.setCharAt(sb.length()-1,']');
        return sb.toString();
    }

    public static void main(String[] args) {
        TestArrayListYuanMa s1=new TestArrayListYuanMa(20);
        s1.add("aa");
        s1.add("bb");
        System.out.println(s1);//输出的是类名加引用,要重写toString方法


    }
}

```





```java
package port9;

/**
 * 自写ArraList的底层实现
 * 增加数组扩容,并加泛型
 */
public class TestArrayListYuanMa01 <E>{
    private Object[] elementDate;
    private  int size;                              //只是做一个计数器用
    private static final  int DEFUALT_CAPACITY=10;  //这里才是容器

    public TestArrayListYuanMa01(){
        elementDate=new Object[DEFUALT_CAPACITY];
     }//无参构造函数

    public TestArrayListYuanMa01(int capacity) {    //构造方法重载,有参

        elementDate=new Object[capacity];           //构造时候直接建立一个Object类型的数组,引用名字为elementDATe
                                                    //直接传入了容量
    }
    public void add(E elem){   //在add方法里加扩容
        //什么时候扩容
        if(size==elementDate.length) {
            //怎么扩容

            Object[] newArray=new Object[elementDate.length+ (elementDate.length>>1)];
            System.arraycopy(elementDate,0,newArray,0,elementDate.length);
            elementDate=newArray;
        }

        elementDate[size++]=elem;
    }
    public String toString(){                  //toString()函数返回是String类型,这里重写来输出对象的内容
        StringBuilder sb=new StringBuilder();
        sb.append("[");
        for(int i=0;i<size;i++){
            sb.append(elementDate[i]+",");
        }
        sb.setCharAt(sb.length()-1,']');
        return sb.toString();
    }

    public static void main(String[] args) {
        TestArrayListYuanMa01 s1=new TestArrayListYuanMa01(20);
        for(int i=0;i<40;i++){
            s1.add("申"+i);
        }
        System.out.println(s1);                             //输出的是类名加引用,要重写toString方法


    }
}


```





```java
package port9;

/**
 * 自写ArraList的底层实现
 * 增加数组扩容,并加泛型
 * 增加set ,get方法
 * 增加数组边界的检查
 */
public class TestArrayListYuanMa02<E>{
    private Object[] elementDate;
    private  int size;                              //只是做一个计数器用
    private static final  int DEFUALT_CAPACITY=10;  //这里才是容器



    public TestArrayListYuanMa02(){
        elementDate=new Object[DEFUALT_CAPACITY];
     }//无参构造函数




    public TestArrayListYuanMa02(int capacity) {    //构造方法重载,有参
        if(capacity<0){
            throw new RuntimeException("容器容量越界");
        }else if(capacity==0){
            elementDate=new Object[DEFUALT_CAPACITY];
        }else{
            elementDate=new Object[capacity];
        }//构造时候直接建立一个Object类型的数组,引用名字为elementDATe
        //直接传入了容量
    }



    public E get(int index){
        checkRange(index);
        return (E)elementDate[index];
    }

    public void checkRange(int index){
        if(index<0||index>=size){
            //手动抛出异常
            throw new RuntimeException("索引不合法"+index);  //public RuntimeException(String message)
            // 使用指定的详细消息构造新的运行时异
        }

    }
    public void set(E elem,int index){
        //增加索引合法判断[0,size)是合法的
        checkRange(index);
       /* if(index<0||index>=size){
            //手动抛出异常
            throw new RuntimeException("索引不合法"+index);  //public RuntimeException(String message)
                                                      // 使用指定的详细消息构造新的运行时异
        }*/
        elementDate[index]=elem;

    }


    public void add(E elem){   //在add方法里加扩容
        //什么时候扩容
        if(size==elementDate.length) {
            //怎么扩容

            Object[] newArray=new Object[elementDate.length+ (elementDate.length>>1)];
            System.arraycopy(elementDate,0,newArray,0,elementDate.length);
            elementDate=newArray;
        }

        elementDate[size++]=elem;             //elem是一个对象,赋值给Object类型的数组elementDate
    }



    public String toString(){                  //toString()函数返回是String类型,这里重写来输出对象的内容
        StringBuilder sb=new StringBuilder();
        sb.append("[");
        for(int i=0;i<size;i++){
            sb.append(elementDate[i]+",");
        }
        sb.setCharAt(sb.length()-1,']');
        return sb.toString();
    }



    public static void main(String[] args) {
        TestArrayListYuanMa02 s1=new TestArrayListYuanMa02(20);
        for(int i=0;i<40;i++){
            s1.add("申"+i);
        }
        s1.set("dd",-1);
        System.out.println(s1);                             //输出的是类名加引用,要重写toString方法
    }
}

```





```java
package port9;

/**
 * 自写ArraList的底层实现
 * 增加数组扩容,并加泛型
 * 增加set ,get方法
 * 增加数组边界的检查
 * 增加remove方法
 */
public class TestArrayListYuanMa03<E>{
    private Object[] elementDate;        //增加泛型E.E可以抽象为任意的对象类型
    private  int size;                              //只是做一个计数器用
    private static final  int DEFUALT_CAPACITY=10;  //这里才是容器



    public TestArrayListYuanMa03(){
        elementDate=new Object[DEFUALT_CAPACITY];
     }//无参构造函数




    public TestArrayListYuanMa03(int capacity) {    //构造方法重载,有参
        if(capacity<0){
            throw new RuntimeException("容器容量越界");
        }else if(capacity==0){
            elementDate=new Object[DEFUALT_CAPACITY];
        }else{
            elementDate=new Object[capacity];
        }//构造时候直接建立一个Object类型的数组,引用名字为elementDATe
        //直接传入了容量
    }



    public E get(int index){
        checkRange(index);
        return (E)elementDate[index];
    }

    public void checkRange(int index){
        if(index<0||index>=size){
            //手动抛出异常
            throw new RuntimeException("索引不合法"+index);  //public RuntimeException(String message)
            // 使用指定的详细消息构造新的运行时异
        }

    }
    public void set(E elem,int index){
        //增加索引合法判断[0,size)是合法的
        checkRange(index);
       /* if(index<0||index>=size){
            //手动抛出异常
            throw new RuntimeException("索引不合法"+index);  //public RuntimeException(String message)
                                                      // 使用指定的详细消息构造新的运行时异
        }*/
        elementDate[index]=elem;

    }


    public void add(E elem){   //在add方法里加扩容
        //什么时候扩容
        if(size==elementDate.length) {
            //怎么扩容

            Object[] newArray=new Object[elementDate.length+ (elementDate.length>>1)];
            System.arraycopy(elementDate,0,newArray,0,elementDate.length);
            elementDate=newArray;
        }

        elementDate[size++]=elem;             //elem是一个对象,赋值给Object类型的数组elementDate
    }



    public void remove(E elem){                  //第一个remove方法,根据内容删
        //elem将他和所有数组的元素进行比较,第一个为true的返回
        for(int i=0;i<size;i++){
            if(elem.equals(get(i))){  //容器中所有的比较操作用的都是equals
                //将该元素从此处移除
                remove(i);
            }
        }
    }

    public void remove(int index){          //第二个remove方法,根据索引删,构成方法重载
        //a,b,c,d,e,f
        //a,b,d,e,f,null 数组拷贝操作,删2的位置,从3开始拷贝,最后一个为空
        int movelength=elementDate.length-index-1;
        if(movelength>0) {
            System.arraycopy(elementDate, index + 1, elementDate, index, movelength);
            elementDate[size-1]=null;
            size--;            //实时跟进数组长度
        }else {
            elementDate[size-1]=null;         //所有句子可以用elemDate[--size]=null;
        }
    }

    public String toString(){                  //toString()函数返回是String类型,这里重写来输出对象的内容
        StringBuilder sb=new StringBuilder();
        sb.append("[");
        for(int i=0;i<size;i++){
            sb.append(elementDate[i]+",");
        }
        sb.setCharAt(sb.length()-1,']');
        return sb.toString();
    }

    public int size(){
        return size;
   }

    public boolean isEmpty(){
        return size==0?true:false;
   }

    public static void main(String[] args) {
        TestArrayListYuanMa03 s1=new TestArrayListYuanMa03(20);
        for(int i=0;i<40;i++){
            s1.add("申"+i);
        }
        s1.set("dd",1);
        System.out.println(s1.get(2));
        System.out.println(s1);
        s1.remove(3);
        s1.remove("申0");
        System.out.println(s1);//输出的是类名加引用,要重写toString方法
        System.out.println(s1.size());
    }
}

```

