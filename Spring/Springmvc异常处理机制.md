![image](https://user-images.githubusercontent.com/65000660/172418759-01519065-d08e-4f3e-9463-f8cafa4ba41c.png)

###  异常处理两种方式:

####  使用Spring MVC提供的简单异常处理器SimpleMappingExceptionResolver 

得配置:

SpringMVC已经定义好了该类型转换器，在使用时可以根据项目情况进行相应异常与视图的映射配置:

就是一个异常对应一个显示界面:

![image](https://user-images.githubusercontent.com/65000660/172418800-acefe251-e190-4ffd-99ed-c5f21e68a99c.png)

```xml
<!--配置异常处理器-->
<!--<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
    &lt;!&ndash;<property name="defaultErrorView" value="error"/>&ndash;&gt;
    <property name="exceptionMappings">
        <map>
            <entry key="java.lang.ClassCastException" value="error1"/>
            <entry key="com.itheima.exception.MyException" value="error2"/>
        </map>
    </property>
</bean>-->
```

####  实现Spring的异常处理接口HandlerExceptionResolver 自定义自己的异常处理器

自定义,异常怎么处理我说的算

#### 4 自定义异常处理步骤

#####  ① 创建异常处理器类实现HandlerExceptionResolver 

##### ② 配置异常处理器 

##### ③ 编写异常页面 

##### ④ 测试异常跳转



```java
package com.itheima.resolver;

import com.itheima.exception.MyException;
import org.springframework.web.servlet.HandlerExceptionResolver;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class MyExceptionResolver implements HandlerExceptionResolver {

    /*
        参数Exception：异常对象
        返回值ModelAndView：跳转到错误视图信息
     */
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
        ModelAndView modelAndView = new ModelAndView();

        if(e instanceof MyException){
            modelAndView.addObject("info","自定义异常");
        }else if(e instanceof ClassCastException){
            modelAndView.addObject("info","类转换异常");
        }

        modelAndView.setViewName("error");

        return modelAndView;
    }
}
```

### 异常只管往上抛
