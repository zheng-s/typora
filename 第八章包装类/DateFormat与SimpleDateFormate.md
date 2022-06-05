```java
package port8;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
/**
 * DateFormat类把时间对象转化为指定格式的字符串,繁殖,吧置顶格式的字符串转化为时间对象
 * DateFormat是一个抽象类,一般使用它的子类SimpleDateFormat类来实现
 * **parse方法：将字符串类型（java.lang.String）解析为日期类型（java.util.Date）**
 * **format方法：将日期类型（Date）数据格式化为字符串（String）**
 */
public class TestDateFormat {
    public static void main(String[] args) throws ParseException {
        //用new实例化SimpleDateFormat对象
        SimpleDateFormat s1=new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
        SimpleDateFormat s=new SimpleDateFormat("yyyy-MM-dd");
        //将时间对象转化为字符串
        String daytime=s1.format(new Date());     //得到的是当前的时间,到毫秒
        System.out.println(daytime);              //得到的是当前时间,精确到几号
        System.out.println(s.format(new Date())); //format就是格式转化
        System.out.println(new SimpleDateFormat("hh:mm:ss").format(new Date()));

        String time="2007-10-7";
        Date date=s.parse(time);  //s的格式"yyyy-MM-dd"
        System.out.println("date1:"+date);

        time="2007-10-7 20:15:30";
        date=s1.parse(time);     //s1的格式:"yyyy-MM-dd hh:mm:ss"所以是s1.什么
        System.out.println("date2:"+date);


    }
}

```

