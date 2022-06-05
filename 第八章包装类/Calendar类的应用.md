```java
package port8;

import sun.util.resources.cldr.aa.CalendarData_aa_ER;

import java.text.DateFormat;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;
import java.util.GregorianCalendar;
import java.util.Scanner;

/**
 * 用Clalendar实现可视化日历
 * SimpleDateFormat 是一个各种项目中使用频度都很高的类，主要用于时间解析与格式化，频繁使用的主要方法有parse和format.
 * parse方法：将字符串类型（java.lang.String）解析为日期类型（java.util.Date）
 * format方法：将日期类型（Date）数据格式化为字符串（String）
 *
 * SimpleDateFormat是线程不安全的
 * SimpleDateFormat解析异常java.text.ParseException: Unparseable date
 */
public class TestCalendar {
    public static void main(String[] args) throws ParseException {
        System.out.println("请输入日期(格式为:2010-3-3):");
        Scanner scanner=new Scanner(System.in);
        String dateString=scanner.nextLine();
                                               //得到输入结果
        System.out.println("您刚才输入的日期为:"+dateString);
        String[] str=dateString.split("-");
        int year=Integer.parseInt(str[0]);
        int month=new Integer(str[1]);
        int day=new Integer(str[2]);
        Calendar c=new GregorianCalendar(year,month-1,day);
        //这是将字符串转化为Calendar对象

        c.set(Calendar.DATE,1);
        int dow=c.get(Calendar.DAY_OF_WEEK);//week:1-7,日一二三到六
        System.out.println("日\t一\t二\t三\t四\t五\t六");
        for (int i=0;i<dow-1;i++){
            System.out.print("\t");

        }
        int maxDate=c.getActualMaximum(Calendar.DATE);
        for(int i=0;i<=maxDate;i++){
            StringBuilder s=new StringBuilder();
            if(c.get(Calendar.DATE)==day){
                s.append(c.get(Calendar.DATE)+"*\t");
            }else{
                s.append(c.get(Calendar.DATE)+"\t");
            }
            System.out.print(s);
            if(c.get(Calendar.DAY_OF_WEEK)==Calendar.SATURDAY){
                System.out.print("\n");
            }
            c.add(Calendar.DATE,1);
        }




    }
}
```

