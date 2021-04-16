# 基于Java配置类搭建SpringMVC

导入依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>springMVC-annotation-demo</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.10.RELEASE</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
        </dependency>


        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
    </dependencies>
</project>
```



一般用来配置service和dao层，不扫描带@Controller注解的组件（让SpringMVC自己扫描）

```java
package com.feige.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.FilterType;
import org.springframework.stereotype.Controller;

/**
 * @author feige<br />
 * @ClassName: ApplicationConfig <br/>
 * @Description: <br/>
 * @date: 2021/4/12 17:19<br/>
 */

@Configuration
@ComponentScan(basePackages = {"com.feige"},excludeFilters = {@ComponentScan.Filter(type = FilterType.ANNOTATION,value = Controller.class)})
public class ApplicationConfig {

}
```

SpringMVC配置类

```java
package com.feige.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.view.InternalResourceViewResolver;

/**
 * @author feige<br />
 * @ClassName: WebMvcConfig <br/>
 * @Description: <br/>
 * @date: 2021/4/13 8:23<br/>
 */
@Configuration
@EnableWebMvc
@ComponentScan(basePackages = {"com.feige.controller"})
public class WebMvcConfig implements WebMvcConfigurer {

    /**
     * @description: 配置视图解析器
     * @author: feige
     * @date: 2021/4/14 8:47
     * @return: org.springframework.web.servlet.ViewResolver
     */
    @Bean
    public ViewResolver viewResolver(){
        return new InternalResourceViewResolver("/WEB-INF/jsp/",".jsp");
    }

    /**
     * @description: 配置跨域
     * @author: feige
     * @date: 2021/4/14 8:49
     * @param  registry
     * @return: void
     */
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("*").allowedOrigins("*").allowedMethods("*");
    }

    /**
     * @description: 静态资源处理器
     * @author: feige
     * @date: 2021/4/14 8:50
     * @param  registry
     * @return: void
     */
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/static/**").addResourceLocations("/static/");
    }
}
```

DispatcherServlet配置类，servlet3.0之后容器会自动创建一个servlet

AbstractDispatcherServletInitializer的源码

~~~java
public void onStartup(ServletContext servletContext) throws ServletException {
    super.onStartup(servletContext);
    this.registerDispatcherServlet(servletContext);
}

protected void registerDispatcherServlet(ServletContext servletContext) {
    String servletName = this.getServletName();
    Assert.hasLength(servletName, "getServletName() must not return null or empty");
    WebApplicationContext servletAppContext = this.createServletApplicationContext();
    Assert.notNull(servletAppContext, "createServletApplicationContext() must not return null");
    FrameworkServlet dispatcherServlet = this.createDispatcherServlet(servletAppContext);
    Assert.notNull(dispatcherServlet, "createDispatcherServlet(WebApplicationContext) must not return null");
    dispatcherServlet.setContextInitializers(this.getServletApplicationContextInitializers());
    Dynamic registration = servletContext.addServlet(servletName, dispatcherServlet);
    if (registration == null) {
        throw new IllegalStateException("Failed to register servlet with name '" + servletName + "'. Check if there is another servlet registered under the same name.");
    } else {
        registration.setLoadOnStartup(1);
        registration.addMapping(this.getServletMappings());
        registration.setAsyncSupported(this.isAsyncSupported());
        Filter[] filters = this.getServletFilters();
        if (!ObjectUtils.isEmpty(filters)) {
            Filter[] var7 = filters;
            int var8 = filters.length;

            for(int var9 = 0; var9 < var8; ++var9) {
                Filter filter = var7[var9];
                this.registerServletFilter(servletContext, filter);
            }
        }

        this.customizeRegistration(registration);
    }
}

protected String getServletName() {
    return "dispatcher";
}

protected abstract WebApplicationContext createServletApplicationContext();

protected FrameworkServlet createDispatcherServlet(WebApplicationContext servletAppContext) {
    return new DispatcherServlet(servletAppContext);
}
~~~

导入配置

```java
package com.feige.config;

import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

/**
 * @author feige<br />
 * @ClassName: AppInitializer <br/>
 * @Description: <br/>
 * @date: 2021/4/13 10:20<br/>
 */
public class AppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {
    /**
     * @description: 返回带有@Configuration 注解的类，它将会用来配置ContextLoaderListener 创建的的应用上下文中的bean，通常是驱动应用后端的中间层和数据层组件。
     * @author: feige
     * @date: 2021/4/14 8:52
     * @return: java.lang.Class<?>[]
     */
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class<?>[]{ApplicationConfig.class};
    }

    /**
     * @description: DispatcherServlet 启动的时候，会创建Spring应用上下文，加载配置文件或配置类中所声明的Bean，web组件的bean如控制器，视图解析器和处理器映射。
     * @author: feige
     * @date: 2021/4/14 8:53
     * @return: java.lang.Class<?>[]
     */
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class<?>[]{WebMvcConfig.class};
    }

    /**
     * @description: getServletMappings 方法将一个或多个路径映射到DispatcherServlet 上，“/” 表示它会是应用的默认Servlet，它会处理进入应用的所有请
     * @author: feige
     * @date: 2021/4/14 8:54
     * @return: java.lang.String[]
     */
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```



控制器

```java
package com.feige.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

/**
 * @author feige<br />
 * @ClassName: HelloController <br/>
 * @Description: <br/>
 * @date: 2021/4/12 17:19<br/>
 */
@Controller
public class HelloController {

    @RequestMapping(value = "/hello",method = RequestMethod.GET)
    public String hello(Model model){
        model.addAttribute("msg","hello world!");
        return "hello";
    }
}
```



hello.jsp页面

```jsp
<%--
  Created by IntelliJ IDEA.
  User: feige
  Date: 2021/4/13
  Time: 8:54
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>hello</title>
</head>
<body>
<h1>${msg}</h1>
</body>
</html>
```



# 基于xml配置搭建SpringMVC

springmvc-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">


    <!--    配置视图解析器-->
    <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>


    <!-- 自动扫描包，让指定包下的注解生效,由IOC容器统一管理 -->
    <context:component-scan base-package="com.feige.controller"/>
    <!-- 让Spring MVC不处理静态资源 -->
    <mvc:default-servlet-handler/>
    <!--
    支持mvc注解驱动
        在spring中一般采用@RequestMapping注解来完成映射关系
        要想使@RequestMapping注解生效
        必须向上下文中注册DefaultAnnotationHandlerMapping
        和一个AnnotationMethodHandlerAdapter实例
        这两个实例分别在类级别和方法级别处理。
        而annotation-driven配置帮助我们自动完成上述两个实例的注入。
     -->
    <mvc:annotation-driven/>

    <!--    配置处理器映射器-->
    <!--    <bean id="beanNameUrlHandlerMapping" class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>-->

    <!--    配置处理器适配器-->
    <!--    <bean id="simpleServletHandlerAdapter" class="org.springframework.web.servlet.handler.SimpleServletHandlerAdapter"/>-->


</beans>
```

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

<!--    导入配置文件-->
    <import resource="springmvc-servlet.xml"/>
<!--    配置不扫描Controller注解-->
    <context:component-scan base-package="com.feige">
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
</beans>
```

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--1.注册DispatcherServlet-->
    <servlet>
        <servlet-name>DispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--关联一个springmvc的配置文件:【servlet-name】-servlet.xml-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:applicationContext.xml</param-value>
        </init-param>
        <!--启动级别-1-->
        <load-on-startup>1</load-on-startup>
    </servlet>


    <!--/ 匹配所有的请求；（不包括.jsp）-->
    <!--/* 匹配所有的请求；（包括.jsp）-->
    <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

</web-app>
```



```java
package com.feige.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

/**
 * @author feige<br />
 * @ClassName: HelloController <br/>
 * @Description: <br/>
 * @date: 2021/4/14 9:24<br/>
 */
@Controller
public class HelloController {

    @RequestMapping(value = "/hello",method = RequestMethod.GET)
    public String hello(Model model){
        model.addAttribute("msg","hello world!");
        return "hello";
    }
}
```





```jsp
<%--
  Created by IntelliJ IDEA.
  User: feige
  Date: 2021/4/14
  Time: 9:29
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>hello</title>
</head>
<body>
<h1>${msg}</h1>
</body>
</html>
```



# 结果跳转方式

需配置路由解析器（xml方式可查看上面）

```java
/**
 * @description: 配置视图解析器
 * @author: feige
 * @date: 2021/4/14 8:47
 * @return: org.springframework.web.servlet.ViewResolver
 */
@Bean
public ViewResolver viewResolver(){
    return new InternalResourceViewResolver("/WEB-INF/jsp/",".jsp");
}
```

~~~java
package com.feige.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * @author feige<br />
 * @ClassName: HelloController <br/>
 * @Description: <br/>
 * @date: 2021/4/12 17:19<br/>
 */
@Controller
// 父路径 访问 http://localhost:8100/parent/hello (方法的路径）
@RequestMapping("/parent")
public class HelloController {

    /**
     * @description: 结果跳转的第一种的方式（推荐使用）
     * @author: feige
     * @date: 2021/4/16 9:29
     * @param	model
     * @return: java.lang.String
     */
    //@GetMapping("/hello") 等价下边的注解 类似的还有PostMapping,DeleteMapping,PutMapping
    @RequestMapping(value = "/hello",method = RequestMethod.GET)
    public String hello(Model model){
        model.addAttribute("msg","hello world!");
        //return "redirect:/index.jsp"; 重定向
        //return "forward:/index.jsp"; 转发
        return "hello";
    }

    /**
     * @description: 结果跳转的第二种的方式（还可通过实现Controller接口（需要到配置文件注册），麻烦不推荐使用）
     * @author: feige
     * @date: 2021/4/16 9:28
     * @param	req 可通过该参数访问servletApi 
     * @param	rsp 可通过该参数访问servletApi
     * @return: org.springframework.web.servlet.ModelAndView
     */
    @RequestMapping(value = "/hello1",method = RequestMethod.GET)
    public ModelAndView hello1(HttpServletRequest req, HttpServletResponse rsp){
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","hello world!");
        mv.setViewName("hello");
        return mv;
    }
}

~~~

# 数据处理

处理提交数据

> 1、提交的域名称和处理方法的参数名一致

提交数据 : http://localhost:8100/hello?name=feige

处理方法 :

~~~java
@RequestMapping("/hello")
public String hello(String name){
    System.out.println(name);
    return "hello";
}
~~~

后台输出 : feige

> 2、提交的域名称和处理方法的参数名不一致

提交数据 : http://localhost:8100/hello?username=feige

处理方法 :

~~~java
//@RequestParam("username") : username提交的域的名称 .
@RequestMapping("/hello")
public String hello(@RequestParam("username") String name){
    System.out.println(name);
    return "hello";
}
~~~

后台输出 : feige

> 3、提交的是一个对象

要求提交的表单域和对象的属性名一致  , 参数使用对象即可

1、实体类

~~~java
public class User {
    private int id;
    private String name;
    private int age;
    ...省略get/set/toString方法
}
~~~

2、提交数据 : http://localhost:8100/user?name=feige&id=1&age=21

3、处理方法 :

~~~java
@RequestMapping("/user")
public String user(User user){
    System.out.println(user);
    return "hello";
}
~~~

后台输出 : User { id=1, name='feige', age=21 }

说明：如果使用对象的话，前端传递的参数名和对象名必须一致，否则就是null。


# 把数据传到前端

- Model 只有寥寥几个方法只适合用于储存数据，简化了新手对于Model对象的操作和理解；

- ModelMap 继承了 LinkedMap ，除了实现了自身的一些方法，同样的继承 LinkedMap 的方法和特性；

- ModelAndView 可以在储存数据的同时，可以进行设置返回的逻辑视图，进行控制展示层的跳转。

# 处理乱码

> xml方式（web.xml文件）

~~~xml
<!--解决乱码-->
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
~~~



> java配置类方式（AppInitializer.java）

```java
/**
 * @description: 配置过滤器
 * @author: feige
 * @date: 2021/4/16 9:59
 * @return: javax.servlet.Filter[]
 */
@Override
protected Filter[] getServletFilters() {
    CharacterEncodingFilter encodingFilter = new CharacterEncodingFilter("UTF-8");
    return new Filter[]{encodingFilter};
}
```

