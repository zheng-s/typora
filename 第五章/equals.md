public boolean equals(Object obj) {
        //java中==符号在比较复合型数据类型（类）时比较的是对象在内存中的存放地址，只有同个new出来的对象，
        //他们在内存中的存发地址才一样，比较结果才为true，否则为false
        return (this == obj);
    }

equals方法

```java
package port5;

public class TestEquals {
    public static void main(String[] args) {

        User u1=new User(1000,"sss","123");
        User u2=new User(1000,"ggg","1234");
        System.out.println(u1==u2);
        System.out.println(u1.equals(u2));//这里在User类里重写了equals方法,只要id一样就可以
        String str1=new String("sxt");//String也是类,所以可以初始化
        String str2=new String("sxt");
        System.out.println(str1==str2);//不是同一个对象,是false
        System.out.println(str1.equals(str2));//equals()内容相同,为true,因为String类重写了equals()方法
    }

}
class User{
    int id;
    String name;
    String pwd;

    public User(int id, String name, String pwd) {
        this.id = id;
        this.name = name;
        this.pwd = pwd;
    }
    public boolean equals(Object obj){   //传入的参数是Object类 aka int i
        if(obj==null){
            return false;
        }else{
            if(obj instanceof User){//如果对象是由右边的类或者子类创造的,fanhuitrue
                User c=(User)obj;
                if(c.id==this.id){
                    return true;
                }
            }
            return false;
        }
    }   // 重写equals
}
```

