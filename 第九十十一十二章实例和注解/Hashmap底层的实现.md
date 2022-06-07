当添加一个元素(key-value)时，首先计算key的hash值，以此确定插入数组中的位置，但是可能存在同一hash值的元素已经被放在数组同一位置了，这时就添加到同一hash值的元素的后面，他们在数组的同一位置，就形成了链表，同一个链表上的Hash值是相同的，所以说数组存放的是链表。 JDK8中，当链表长度大于8时，链表就转换为红黑树，这样又大大提高了查找的效率。

**▪ 取数据过程get(key)**

   我们需要通过key对象获得“键值对”对象，进而返回value对象。明白了存储数据过程，取数据就比较简单了，参见以下步骤：

   (1) 获得key的hashcode，通过hash()散列算法得到hash值，进而定位到数组的位置。

   (2) 在链表上挨个比较key对象。 调用equals()方法，将key对象和链表上所有节点的key对象进行比较，直到碰到返回true的节点对象为止。

   (3) 返回equals()为true的节点对象的value对象。

   明白了存取数据的过程，我们再来看一下hashcode()和equals方法的关系：

   Java中规定，两个内容相同(equals()为true)的对象必须具有相等的hashCode。因为如果equals()为true而两个对象的hashcode不同;那在整个存储过程中就发生了悖论。





HashMap的位桶数组，初始大小为16。实际使用时，显然大小是可变的。如果位桶数组中的元素达到(0.75*数组 length)， 就重新调整数组大小变为原来2倍大小。

   扩容很耗时。扩容的本质是定义新的更大的数组，并将旧数组内容挨个拷贝到新数组中。



![image](https://user-images.githubusercontent.com/65000660/172333246-9b0f8e8c-51e8-4efd-998d-7e68f376652b.png)




 TreeMap和HashMap实现了同样的接口Map，因此，用法对于调用者来说没有区别。HashMap效率高于TreeMap;在需要排序的Map时才选用TreeMap。

### map的实现hashmap第一版

```java
package port9;

/**
 * 自定义hashmap理解结构,add方法,解决键重复覆盖问题
 *
 */
public class TestHashMap01 {
    Node2[]  table;//位通数组
    int size;//记录存放键值对数量

    public TestHashMap01() {
        table=new Node2[16] ;//长度一般位2的整数次幂
    }


    public void put(Object key,Object value){
        //定一个新节点来放新存储的K-V对
        Node2 newNode=new Node2();
        newNode.hash=getHash(key.hashCode(), table.length);   //Object类里有返回hashcode的方法,放哪
        newNode.key=key;
        newNode.value=value;


        Node2 temp=table[newNode.hash]; //temp节点就是该放的节点名字
        Node2 iterLast=null; //正在遍历的最后一个元素
        boolean keyRepeat=false;
        //你得到temp就知道了放的位置是啥
        if(temp==null){
            table[newNode.hash]=newNode;//如果计算出hash值该放的位置为空,则直接把新节点newNode放进去
        }else{
            //不空,往后加建立对应的链表
            while(temp!=null){
                //判断key是否重复,则覆盖
                if(temp.key.equals(key)){
                    System.out.println("重复的key");
                    keyRepeat=true;
                    temp.value=value;  //只覆盖value,那到这就不用再往后遍历了
                    break;
                }else{
                    //如果不重复,往后加
                    iterLast=temp;//,如果不同直接加到后面就行了,先用iterLast保存了前面这个元素
                    temp=temp.next;//  往后走一直走到不重复为止
                }

            }
            if(!keyRepeat)
            {iterLast.next=newNode;
            };  //遍历完了,加到最后一个元素后面,如果没有发生重复才用这个,若重复上面已经覆盖了

        }


    }

    public int getHash(int v,int length){
        System.out.println("hash in Myhash:"+(v&(length-1)));//位运算效率高
        return v&(length-1);//      按位与作用相当于取余数,值并不一样,都是散列放的位置
    }

    public static void main(String[] args) {
        TestHashMap01 map=new TestHashMap01();
        map.put(10,"aa");
        map.put(20,"bb");
        map.put(30,"cc");
        map.put(20,"sz");
        System.out.println(map);
    }

}

```

### 第二版

```java
package port9;

/**
 * 自定义hashmap理解结构,add方法,解决键重复覆盖问题
 * 重写toString方法
 */
public class TestHashMap02 {
    Node2[]  table;//位通数组
    int size;//记录存放键值对数量

    public TestHashMap02() {
        table=new Node2[16] ;//长度一般位2的整数次幂
    }


    public void put(Object key,Object value){
        //定一个新节点来放新存储的K-V对
        Node2 newNode=new Node2();
        newNode.hash=getHash(key.hashCode(), table.length);   //Object类里有返回hashcode的方法,放哪
        newNode.key=key;
        newNode.value=value;


        Node2 temp=table[newNode.hash]; //temp节点就是该放的节点名字
        Node2 iterLast=null; //正在遍历的最后一个元素
        boolean keyRepeat=false;
        //你得到temp就知道了放的位置是啥
        if(temp==null){
            table[newNode.hash]=newNode;//如果计算出hash值该放的位置为空,则直接把新节点newNode放进去
        }else{
            //不空,往后加建立对应的链表
            while(temp!=null){
                //判断key是否重复,则覆盖
                if(temp.key.equals(key)){
                    System.out.println("重复的key");
                    keyRepeat=true;
                    temp.value=value;  //只覆盖value,那到这就不用再往后遍历了
                    break;
                }else{
                    //如果不重复,往后加
                    iterLast=temp;//,如果不同直接加到后面就行了,先用iterLast保存了前面这个元素
                    temp=temp.next;//  往后走一直走到不重复为止
                }

            }
            if(!keyRepeat)
            {iterLast.next=newNode;
            };  //遍历完了,加到最后一个元素后面,如果没有发生重复才用这个,若重复上面已经覆盖了

        }


    }


    @Override
    public String toString() {
       StringBuilder sb=new StringBuilder("[");
       for(int i=0;i< table.length;i++){  //两层遍历,一次数组,一次链表
           Node2 temp=table[i];
           while(temp!=null){
               sb.append(temp.key+":"+temp.value+",");
               temp=temp.next;

           }
       }
       sb.setCharAt(sb.length()-1,']');
       return  sb.toString();
    }




    public int getHash(int v, int length){
        System.out.println("hash in Myhash:"+(v&(length-1)));//位运算效率高
        return v&(length-1);//      按位与作用相当于取余数,值并不一样,都是散列放的位置
    }

    public static void main(String[] args) {
        TestHashMap02 map=new TestHashMap02();
        map.put(10,"aa");
        map.put(20,"bb");
        map.put(30,"cc");
        map.put(20,"sz");
        System.out.println(map);
    }

}

```

### 第三版

```java
package port9;

/**
 * 自定义hashmap理解结构,add方法,解决键重复覆盖问题
 * 重写toString方法
 * 增加查询键值对get方法
 */
public class TestHashMap03 {
    Node2[]  table;//位通数组
    int size;//记录存放键值对数量

    public TestHashMap03() {
        table=new Node2[16] ;//长度一般位2的整数次幂
    }


    public void put(Object key,Object value){
        //定一个新节点来放新存储的K-V对
        Node2 newNode=new Node2();
        newNode.hash=getHash(key.hashCode(), table.length);   //Object类里有返回hashcode的方法,放哪
        newNode.key=key;
        newNode.value=value;


        Node2 temp=table[newNode.hash]; //temp节点就是该放的节点名字
        Node2 iterLast=null; //正在遍历的最后一个元素
        boolean keyRepeat=false;
        //你得到temp就知道了放的位置是啥
        if(temp==null){
            table[newNode.hash]=newNode;//如果计算出hash值该放的位置为空,则直接把新节点newNode放进去
            size++;
        }else{
            //不空,往后加建立对应的链表
            while(temp!=null){
                //判断key是否重复,则覆盖
                if(temp.key.equals(key)){
                    System.out.println("重复的key");
                    keyRepeat=true;
                    temp.value=value;  //只覆盖value,那到这就不用再往后遍历了
                    break;
                }else{
                    //如果不重复,往后加
                    iterLast=temp;//,如果不同直接加到后面就行了,先用iterLast保存了前面这个元素
                    temp=temp.next;//  往后走一直走到不重复为止
                }

            }
            if(!keyRepeat)
            {iterLast.next=newNode;
            size++;
            };  //遍历完了,加到最后一个元素后面,如果没有发生重复才用这个,若重复上面已经覆盖了

        }


    }


    @Override
    public String toString() {
       StringBuilder sb=new StringBuilder("[");
       for(int i=0;i< table.length;i++){  //两层遍历,一次数组,一次链表
           Node2 temp=table[i];
           while(temp!=null){
               sb.append(temp.key+":"+temp.value+",");
               temp=temp.next;

           }
       }
       sb.setCharAt(sb.length()-1,']');
       return  sb.toString();
    }


   public Object get(Object key){
        int hash=getHash(key.hashCode(), table.length);
        Object value=null;  //相当于声明 一下
       if(table[hash]!=null){
           Node2 temp=table[hash];
           while(temp!=null){
               if (temp.key.equals(key)){
                   value=temp.value;
                   break;
               }else{
                   temp=temp.next;
               }
           }
       }

       return value;

   }

    public int getHash(int v, int length){
        System.out.println("hash in Myhash:"+(v&(length-1)));//位运算效率高
        return v&(length-1);//      按位与作用相当于取余数,值并不一样,都是散列放的位置
    }

    public static void main(String[] args) {
        TestHashMap03 map=new TestHashMap03();
        map.put(10,"aa");
        map.put(20,"bb");
        map.put(30,"cc");
        map.put(20,"sz");

        System.out.println(map);
        System.out.println(map.get(30));
    }

}

```

### 第四版

```java
package port9;

/**
 * 自定义hashmap理解结构,add方法,解决键重复覆盖问题
 * 重写toString方法
 * 增加查询键值对get方法
 */
public class TestHashMap04 <K,V>{
    Node3[]  table;//位通数组
    int size;//记录存放键值对数量

    public TestHashMap04() {
        table=new Node3[16] ;//长度一般位2的整数次幂
    }


    public void put(K key,V value){
        //定一个新节点来放新存储的K-V对
        Node3 newNode=new Node3();
        newNode.hash=getHash(key.hashCode(), table.length);   //Object类里有返回hashcode的方法,放哪
        newNode.key=key;
        newNode.value=value;


        Node3 temp=table[newNode.hash]; //temp节点就是该放的节点名字
        Node3 iterLast=null; //正在遍历的最后一个元素
        boolean keyRepeat=false;
        //你得到temp就知道了放的位置是啥
        if(temp==null){
            table[newNode.hash]=newNode;//如果计算出hash值该放的位置为空,则直接把新节点newNode放进去
            size++;
        }else{
            //不空,往后加建立对应的链表
            while(temp!=null){
                //判断key是否重复,则覆盖
                if(temp.key.equals(key)){
                    System.out.println("重复的key");
                    keyRepeat=true;
                    temp.value=value;  //只覆盖value,那到这就不用再往后遍历了
                    break;
                }else{
                    //如果不重复,往后加
                    iterLast=temp;//,如果不同直接加到后面就行了,先用iterLast保存了前面这个元素
                    temp=temp.next;//  往后走一直走到不重复为止
                }

            }
            if(!keyRepeat)
            {iterLast.next=newNode;
            size++;
            };  //遍历完了,加到最后一个元素后面,如果没有发生重复才用这个,若重复上面已经覆盖了

        }


    }


    @Override
    public String toString() {
       StringBuilder sb=new StringBuilder("[");
       for(int i=0;i< table.length;i++){  //两层遍历,一次数组,一次链表
           Node3 temp=table[i];
           while(temp!=null){
               sb.append(temp.key+":"+temp.value+",");
               temp=temp.next;

           }
       }
       sb.setCharAt(sb.length()-1,']');
       return  sb.toString();
    }


   public V get(K key){
        int hash=getHash(key.hashCode(), table.length);
        V value=null;  //相当于声明 一下
       if(table[hash]!=null){
           Node3 temp=table[hash];
           while(temp!=null){
               if (temp.key.equals(key)){
                   value=(V)temp.value;
                   break;
               }else{
                   temp=temp.next;
               }
           }
       }

       return value;

   }

    public int getHash(int v, int length){
        System.out.println("hash in Myhash:"+(v&(length-1)));//位运算效率高
        return v&(length-1);//      按位与作用相当于取余数,值并不一样,都是散列放的位置
    }

    public static void main(String[] args) {
        TestHashMap04<Integer,String> map=new TestHashMap04<>();
        map.put(10,"aa");
        map.put(20,"bb");
        map.put(30,"cc");
        map.put(20,"sz");

        System.out.println(map);
        System.out.println(map.get(30));
    }

}

```

