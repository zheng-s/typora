### 第一版

```java
package port9;

/**
 * 自定义一个链表
 * add方法和get方法
 */
public class TestLinkedList {
    private Node first;
    private  Node last;
    private  int size;
    //[]叫对象
    //[a,b,c]
    public void add(Object obj){
        Node node=new Node(obj);
        if(first==null){
//            node.previous=null;
//            node.next=null;
            first=node;
            last=node;
        }else{
            node.previous=last;
            node.next=null;
            last.next=node;
            last=node;
        }
        size++;

    }


    public void checkPointer(int index){
       if(index<0||index>size-1){
           throw new RuntimeException("超过链表长度"+index);
       }
    }

    //[a(first),b,c,d,e,f(last)]
    public Object get(int index){         //改进,index离头近从头找,离尾巴近从尾巴找
        checkPointer(index);
        Node temp=first;
        for(int i=0;i<index;i++){    //这里就可以看出链表查询聊率低
            temp=temp.next;
        }
        return temp.element;
    }


    public String toString(){       //重写的是父类Object类的toString方法
        //挨个遍历链表中的元素并打印出来[a,b,c]first=a,last=c
        StringBuilder sb=new StringBuilder("[");
        Node temp=first;//临时节点temp指向a,在指向b,c.next为空,就不打印了
        while(temp!=null){
            sb.append(temp.element+"\t");
            temp=temp.next;
        }
        sb.setCharAt(sb.length()-1,']');
        return sb.toString();//返回对象的字符串序列  这个是StringBuilder大的toString方法
                                //println方法默认调用toString方法

    }


    public static void main(String[] args) {
        TestLinkedList list=new TestLinkedList();

        list.add("a");
        list.add("b");
        list.add("c");
        System.out.println(list);
        System.out.println(list.get(2));

    }
}

```

### 第二版

```java
package port9;

/**
 * 自定义一个链表
 * add方法和get方法
 * 增加remove方法
 */
public class TestLinkedList01 {
    private Node first;
    private  Node last;
    private  int size;
    //[]加对象
    //[a,b,c]
    public void add(Object obj){
        Node node=new Node(obj);
        if(first==null){
//            node.previous=null;
//            node.next=null;
            first=node;
            last=node;
        }else{
            node.previous=last;
            node.next=null;
            last.next=node;
            last=node;
        }
        size++;

    }

    public  void remove(int index){   //其实重合的代码可以再封装一个函数
        checkPointer(index);
        Node temp=first;
        for(int i=0;i<index;i++){    //这里就可以看出链表查询聊率低
            temp=temp.next;
        }
        if(temp!=null){
            Node up=temp.previous;  //这里只是把他的上一个命名为up
            Node down=temp.next;    //它的下一个命名为down
            if(up!=null) {
                up.next = down;        //这里才开始改指针
            }if(down!=null) {
                down.previous = up;
            }
            if(index==0){
                first=down;  //我要删出第一个,所以first变为down

            }
            if(index==size-1){
                last=up;
            }
            size--;
        }

    }


    public void checkPointer(int index){
       if(index<0||index>size-1){
           throw new RuntimeException("超过链表长度"+index);
       }
    }

    //[a(first),b,c,d,e,f(last)]
    public Object get(int index){         //改进,index离头近从头找,离尾巴近从尾巴找
        checkPointer(index);
        Node temp=first;
        for(int i=0;i<index;i++){    //这里就可以看出链表查询聊率低
            temp=temp.next;
        }
        return temp.element;
    }


    public String toString(){       //重写的是父类Object类的toString方法
        //挨个遍历链表中的元素并打印出来[a,b,c]first=a,last=c
        StringBuilder sb=new StringBuilder("[");
        Node temp=first;//临时节点temp指向a,在指向b,c.next为空,就不打印了
        while(temp!=null){
            sb.append(temp.element+"\t");
            temp=temp.next;
        }
        sb.setCharAt(sb.length()-1,']');
        return sb.toString();//返回对象的字符串序列  这个是StringBuilder大的toString方法
                                //println方法默认调用toString方法

    }


    public static void main(String[] args) {
        TestLinkedList01 list=new TestLinkedList01();

        list.add("a");
        list.add("b");
        list.add("c");
        System.out.println(list);
        System.out.println(list.get(2));
        list.remove(0);
        System.out.println(list);
    }
}

```

### 第三版

```java
package port9;

/**
 * 自定义一个链表
 * add方法和get方法
 * 增加remove方法
 * 增加插入方法,并把寻找节点位置封装为函数
 */
public class TestLinkedList02 {
    private Node first;
    private  Node last;
    private  int size;
    //[]加对象
    //[a,b,c]
    public void add(Object obj){
        Node node=new Node(obj);
        if(first==null){
//            node.previous=null;
//            node.next=null;
            first=node;
            last=node;
        }else{
            node.previous=last;
            node.next=null;
            last.next=node;
            last=node;
        }
        size++;

    }
   public  void insert(int index ,Object obj){

        Node newNode=new Node(obj);
        Node temp=getNode(index);
        if(temp!=null){
            Node up=temp.previous;
            up.next=newNode;
            newNode.previous=up;
            newNode.next=temp;
            temp.previous=newNode;
        }


   }
    public  void remove(int index){   //其实重合的代码可以再封装一个函数
        checkPointer(index);
        Node temp=getNode(index);

        if(temp!=null){
            Node up=temp.previous;  //这里只是把他的上一个命名为up
            Node down=temp.next;    //它的下一个命名为down
            if(up!=null) {
                up.next = down;        //这里才开始改指针
            }if(down!=null) {
                down.previous = up;
            }
            if(index==0){
                first=down;  //我要删出第一个,所以first变为down

            }
            if(index==size-1){
                last=up;
            }
            size--;
        }

    }


    public void checkPointer(int index){
       if(index<0||index>size-1){
           throw new RuntimeException("超过链表长度"+index);
       }
    }


    public Node getNode(int index){
        Node temp=first;
        for(int i=0;i<index;i++){    //这里就可以看出链表查询聊率低
            temp=temp.next;
        }
        return temp;      //返回一个自定义的节点类Node
    }




    //[a(first),b,c,d,e,f(last)]
    public Object get(int index){         //改进,index离头近从头找,离尾巴近从尾巴找
        checkPointer(index);
        Node temp=getNode(index);
        return temp!=null?temp.element:null;
    }


    public String toString(){       //重写的是父类Object类的toString方法
        //挨个遍历链表中的元素并打印出来[a,b,c]first=a,last=c
        StringBuilder sb=new StringBuilder("[");
        Node temp=first;//临时节点temp指向a,在指向b,c.next为空,就不打印了
        while(temp!=null){
            sb.append(temp.element+"\t");
            temp=temp.next;
        }
        sb.setCharAt(sb.length()-1,']');
        return sb.toString();//返回对象的字符串序列  这个是StringBuilder大的toString方法
                                //println方法默认调用toString方法

    }


    public static void main(String[] args) {
        TestLinkedList02 list=new TestLinkedList02();

        list.add("a");
        list.add("b");
        list.add("c");
        System.out.println(list);
        System.out.println(list.get(2));
        list.remove(0);
        System.out.println(list);

        list.insert(1,"老高");
        System.out.println(list);
    }
}

```

### 第四版

```java
package port9;

/**
 * 自定义一个链表
 * add方法和get方法
 * 增加remove方法
 * 增加插入方法,并把寻找节点位置封装为函数
 * 小封装加泛型
 */
public class TestLinkedList03<E> {
    private Node first;
    private  Node last;
    private  int size;
    //[]加对象
    //[a,b,c]
    public void add(E element){
        Node node=new Node(element);
        if(first==null){
//            node.previous=null;
//            node.next=null;
            first=node;
            last=node;
        }else{
            node.previous=last;
            node.next=null;
            last.next=node;
            last=node;
        }
        size++;

    }
   public  void insert(int index ,E element ){
        checkPointer(index);
        Node newNode=new Node(element);
        Node temp=getNode(index);
        if(temp!=null){
            Node up=temp.previous;
            up.next=newNode;
            newNode.previous=up;
            newNode.next=temp;
            temp.previous=newNode;
        }


   }
    public  void remove(int index){   //其实重合的代码可以再封装一个函数
        checkPointer(index);
        Node temp=getNode(index);

        if(temp!=null){
            Node up=temp.previous;  //这里只是把他的上一个命名为up
            Node down=temp.next;    //它的下一个命名为down
            if(up!=null) {
                up.next = down;        //这里才开始改指针
            }if(down!=null) {
                down.previous = up;
            }
            if(index==0){
                first=down;  //我要删出第一个,所以first变为down

            }
            if(index==size-1){
                last=up;
            }
            size--;
        }

    }


    public void checkPointer(int index){
       if(index<0||index>size-1){
           throw new RuntimeException("超过链表长度"+index);
       }
    }


    public Node getNode(int index){
        Node temp=first;              //每次都是从头开始查询
        for(int i=0;i<index;i++){    //这里就可以看出链表查询聊率低
            temp=temp.next;
        }
        return temp;      //返回一个自定义的节点类Node
    }




    //[a(first),b,c,d,e,f(last)]
    public E get(int index){         //改进,index离头近从头找,离尾巴近从尾巴找
        checkPointer(index);
        Node temp=getNode(index);
        return temp!=null?(E)temp.element:null;
    }


    public String toString(){       //重写的是父类Object类的toString方法
        //挨个遍历链表中的元素并打印出来[a,b,c]first=a,last=c
        StringBuilder sb=new StringBuilder("[");
        Node temp=first;//临时节点temp指向a,在指向b,c.next为空,就不打印了
        while(temp!=null){
            sb.append(temp.element+"\t");
            temp=temp.next;
        }
        sb.setCharAt(sb.length()-1,']');
        return sb.toString();//返回对象的字符串序列  这个是StringBuilder大的toString方法
                                //println方法默认调用toString方法

    }


    public static void main(String[] args) {
        TestLinkedList03<String> list=new TestLinkedList03<>();//这一句说明泛型E为String

        list.add("a");
        list.add("b");
        list.add("c");
        System.out.println(list);
        System.out.println(list.get(2));
        list.remove(0);
        System.out.println(list);

        list.insert(1,"老高");
        System.out.println(list);
    }
}

```

