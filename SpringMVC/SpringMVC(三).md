# SpringMVC返回json格式数据

为什么需要返回json格式的数据呢？

现在处在前后端分离时代，前后端职责越来越专一，后端返回json格式数据，前端通过ajax请求数据

> 什么是JSON

JSON(JavaScript Object Notation, JS 对象标记) 是一种轻量级的数据交换格式，目前使用特别广泛。

- 采用完全独立于编程语言的文本格式来存储和表示数据。

- 简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。

- 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。

在 JavaScript 语言中，一切都是对象。因此，任何JavaScript 支持的类型都可以通过 JSON 来表示，例如字符串、数字、对象、数组等。看看他的要求和语法格式：

- 对象表示为键值对，数据由逗号分隔

- 花括号保存对象

- 方括号保存数组

JSON 键值对是用来保存 JavaScript 对象的一种方式，和 JavaScript 对象的写法也大同小异，键/值对组合中的键名写在前面并用双引号 "" 包裹，使用冒号 : 分隔，然后紧接着值：

导入jar包（类似fastjson json解析工具还有Jackson，Gson）

~~~java
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.47</version>
</dependency>
~~~

> HttpMessageConverter简介



org.springframework.http.converter.HttpMessageConverter 是SpringMVC中提供的一个策略接口，它是一个转换器类，负责转换HTTP请求和响应，可以把对象自动转换为JSON(使用Jackson库或Gson库)或XML(使用Jackson XML或者JAXB2)，对字符串默认使用UTF-8编码处理，一般情况下采用默认配置就可以。

该接口说明如下：
Strategy interface that specifies a converter that can convert from and to HTTP requests and responses.
简单说就是 HTTP request (请求)和response (响应)的转换器。该接口里有5个方法，接收到请求时判断是否能读（canRead），能读则读（read）；返回结果时判断是否能写（canWrite），能写则写（write），以及获取支持的 MediaType（application/json之类）方法。



![image-20210416103622988](SpringMVC(三)/image-20210416103622988.png)



> 自定义HttpMessageConverter

WebMvcConfig.java

~~~java
/**
     * @description: 配置FastJsonHttpMessageConverter
     * @author: feige
     * @date: 2021/4/16 10:35
     * @param	converters
     * @return: void
     */
    @Override
    public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
        System.out.println("--http--");
        FastJsonHttpMessageConverter converter = new FastJsonHttpMessageConverter();
        FastJsonConfig config = new FastJsonConfig();
        config.setSerializerFeatures(SerializerFeature.DisableCircularReferenceDetect);
        converter.setFastJsonConfig(config);
        converter.setDefaultCharset(StandardCharsets.UTF_8);
        converters.add(converter);
        converters.add(new StringHttpMessageConverter(StandardCharsets.UTF_8));

    }
~~~

> xml方式可参考官网例子

~~~xml
<mvc:annotation-driven>
    <mvc:message-converters>
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <property name="objectMapper" ref="objectMapper"/>
        </bean>
        <bean class="org.springframework.http.converter.xml.MappingJackson2XmlHttpMessageConverter">
            <property name="objectMapper" ref="xmlMapper"/>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>

<bean id="objectMapper" class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean"
      p:indentOutput="true"
      p:simpleDateFormat="yyyy-MM-dd"
      p:modulesToInstall="com.fasterxml.jackson.module.paramnames.ParameterNamesModule"/>

<bean id="xmlMapper" parent="objectMapper" p:createXmlMapper="true"/>
~~~



> 编写控制器

~~~java
package com.feige.controller;

import com.alibaba.fastjson.JSONObject;
import com.feige.pojo.User;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.ArrayList;

/**
 * @author feige<br />
 * @ClassName: JsonController <br/>
 * @Description: <br/>
 * @date: 2021/4/16 10:03<br/>
 */
@RestController
// 复合注解@Controller和@ResponseBody 不转发到视图，直接返回结果
public class JsonController {


    /**
     * @description:  produces = "application/json;charset=utf-8" 解决中文乱码（配置StringHttpMessageConverter之后就不需要每个都写了）
     * @author: feige
     * @date: 2021/4/16 10:52
     * @return: java.lang.String
     */
    @GetMapping(value = "/get",produces = "application/json;charset=utf-8" )
    public String get(){
        User user = new User(1, "飞哥", 21);
        return JSONObject.toJSONString(user);
    }

    /**
     * @description: 配置了FastJsonHttpMessageConverter
     * @author: feige
     * @date: 2021/4/16 10:24
     * @return: java.lang.String
     */
    @GetMapping(value = "/list")
    public Object list(){
        ArrayList<User> users = new ArrayList<>();
        users.add(new User(1, "feige", 21));
        users.add(new User(1, "胡飞", 21));
        users.add(new User(1, "飞哥", 21));
        return users;
    }
}


~~~

结果

~~~json
{"age":21,"id":1,"name":"feige"}
~~~

~~~json
[{"age":21,"id":1,"name":"feige"},{"age":21,"id":1,"name":"胡飞"},{"age":21,"id":1,"name":"飞哥"}]
~~~

