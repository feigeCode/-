# SpringMVC拦截器+文件上传下载

SpringMVC的处理器拦截器类似于Servlet开发中的过滤器Filter,用于对处理器进行预处理和后处理。开发者可以自己定义一些拦截器来实现特定的功能。

过滤器与拦截器的区别：拦截器是AOP思想的具体应用。

- 过滤器

	- servlet规范中的一部分，任何java web工程都可以使用

	- 在url-pattern中配置了/*之后，可以对所有要访问的资源进行拦截

- 拦截器

	- 拦截器是SpringMVC框架自己的，只有使用了SpringMVC框架的工程才能使用

	- 拦截器只会拦截访问的控制器方法， 如果访问的是jsp/html/css/image/js是不会进行拦截的

## 拦截器

> 自定义拦截器



~~~java
package com.feige.interceptor;

import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * @author feige<br />
 * @ClassName: LoginInterceptor <br/>
 * @Description: <br/>
 * @date: 2021/4/16 11:00<br/>
 */
@Component
public class MyInterceptor implements HandlerInterceptor {

    /**
     * @description: 在请求处理的方法之前执行
     * @author: feige
     * @date: 2021/4/16 11:07
     * @param	request
     * @param	response
     * @param	handler
     * @return: boolean true执行下一个拦截器 false就不执行下一个拦截器
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("----处理前----");
        return true;
    }

    /**
     * @description: 在请求处理方法执行之后执行
     * @author: feige
     * @date: 2021/4/16 11:09
     * @param	request
     * @param	response
     * @param	handler
     * @param	modelAndView
     * @return: void
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("----处理后----");
    }

    /**
     * @description: 在dispatcherServlet处理后执行,做清理工作
     * @author: feige
     * @date: 2021/4/16 11:09
     * @param	request
     * @param	response
     * @param	handler
     * @param	ex
     * @return: void
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("----清理----");
    }
}

~~~



WebMvcConfig.java

~~~java
     @Autowired
     private MyInterceptor myInterceptor; 

/**
     * @description: 注册拦截器 /** 包括路径及其子路径
     *                /admin/* 拦截的是/admin/add等等这种 , /admin/add/user不会被拦截
     *                 /admin/** 拦截的是/admin/下的所有
     * @author: feige
     * @date: 2021/4/16 11:11
     * @param	registry
     * @return: void
     */
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(myInterceptor).addPathPatterns("/**");
    }
~~~



> xml方式配置

~~~xml
<!--关于拦截器的配置-->
<mvc:interceptors>
    <mvc:interceptor>
        <!--/** 包括路径及其子路径-->
        <!--/admin/* 拦截的是/admin/add等等这种 , /admin/add/user不会被拦截-->
        <!--/admin/** 拦截的是/admin/下的所有-->
        <mvc:mapping path="/**"/>
        <!--bean配置的就是拦截器-->
        <bean class="com.feige.interceptor.MyInterceptor"/>
    </mvc:interceptor>
</mvc:interceptors>
~~~

## 文件上传

文件上传是项目开发中最常见的功能之一 ,springMVC 可以很好的支持文件上传，但是SpringMVC上下文中默认没有装配MultipartResolver，因此默认情况下其不能处理文件上传工作。如果想使用Spring的文件上传功能，则需要在上下文中配置MultipartResolver。

前端表单要求：为了能上传文件，必须将表单的method设置为POST，并将enctype设置为multipart/form-data。只有在这样的情况下，浏览器才会把用户选择的文件以二进制数据发送给服务器；

对表单中的 enctype 属性做个详细的说明：

- application/x-www=form-urlencoded：默认方式，只处理表单域中的 value 属性值，采用这种编码方式的表单会将表单域中的值处理成 URL 编码方式。

- multipart/form-data：这种编码方式会以二进制流的方式来处理表单数据，这种编码方式会把文件域指定文件的内容也封装到请求参数中，不会对字符编码。

- text/plain：除了把空格转换为 "+" 号外，其他字符都不做编码处理，这种方式适用直接通过表单发送邮件。

```html
<form action="/upload" enctype="multipart/form-data" method="post">
    <input type="file" name="file"/>
    <input type="submit">
</form>
```

一旦设置了enctype为multipart/form-data，浏览器即会采用二进制流的方式来处理表单数据，而对于文件上传的处理则涉及在服务器端解析原始的HTTP响应。在2003年，Apache Software Foundation发布了开源的Commons FileUpload组件，其很快成为Servlet/JSP程序员上传文件的最佳选择。

- Servlet3.0规范已经提供方法来处理文件上传，但这种上传需要在Servlet中完成。

- 而Spring MVC则提供了更简单的封装。

- Spring MVC为文件上传提供了直接的支持，这种支持是用即插即用的MultipartResolver实现的。

- Spring MVC使用Apache Commons FileUpload技术实现了一个MultipartResolver实现类：

- CommonsMultipartResolver。因此，SpringMVC的文件上传还需要依赖Apache Commons FileUpload的组件。
	

配置commonsMultipartResolver（WebMvcConfig.java）

```java
@Bean(value = "multipartResolver")
public CommonsMultipartResolver commonsMultipartResolver(){
    CommonsMultipartResolver resolver = new CommonsMultipartResolver();
    resolver.setMaxUploadSize(10485760L);
    resolver.setMaxInMemorySize(40960);
    resolver.setDefaultEncoding("UTF-8");
    return resolver;
}
```



~~~java

package com.feige.controller;

import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartFile;

import java.io.*;

/**
 * @author feige<br />
 * @ClassName: UploadController <br/>
 * @Description: <br/>
 * @date: 2021/4/16 11:24<br/>
 */
@RestController
public class UploadController {

    @PostMapping("/upload")
    public String upload(MultipartFile file) throws IOException {
        // 获取文件名包括扩展名
        String filename = file.getOriginalFilename();
        // 第一种方式
//        // 获取输入流
//        InputStream is = file.getInputStream();
//        //文件输出流
//        OutputStream os = new FileOutputStream(new File("E:\\project\\java-project\\springMVC-annotation-demo\\file\\" + filename)); 
// 
//        //读取写出
//        int len=0;
//        byte[] buffer = new byte[1024];
//        while ((len=is.read(buffer))!=-1){
//            os.write(buffer,0,len);
//            os.flush();
//        }
//        os.close();
//        is.close();


        // 第二种方式 把文件保存到指定目录
        file.transferTo(new File("E:\\project\\java-project\\springMVC-annotation-demo\\file\\" + filename));

        return "success";
    }

}

~~~

## 文件下载

```java
package com.feige.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import javax.servlet.ServletOutputStream;
import javax.servlet.http.HttpServletResponse;
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.net.URLEncoder;

/**
 * @author feige<br />
 * @ClassName: DownloadController <br/>
 * @Description: <br/>
 * @date: 2021/4/16 11:46<br/>
 */
@RestController
public class DownloadController {

    @GetMapping("/download/{fileName}")
    public void download(@PathVariable String fileName, HttpServletResponse response) throws IOException {
        //1、设置response 响应头
        //设置页面不缓存,清空buffer
        response.reset();
        //字符编码
        response.setCharacterEncoding("UTF-8");
        //二进制传输数据
        response.setContentType("multipart/form-data");
        //设置响应头
        response.setHeader("Content-Disposition", "attachment;fileName="+ URLEncoder.encode(fileName + ".docx", "UTF-8"));
        File file = new File("E:\\project\\java-project\\springMVC-annotation-demo\\file", fileName + ".docx");
        FileInputStream is = new FileInputStream(file);
        ServletOutputStream os = response.getOutputStream();
        byte[] bytes = new byte[1024];
        int length;
        while ((length = is.read(bytes)) != -1){
            os.write(bytes,0,length);
            os.flush();
        }
        os.close();
        is.close();
    }
}
```