# 一、MVC概念

[参考狂神说](https://blog.csdn.net/qq_33369905/article/details/106647313)

MVC是**模型(Model)**、**视图(View)**、**控制器(Controller)**的简写，是一种软件设计规范。

是将业务逻辑、数据、显示分离的方法来组织代码。

MVC主要作用是降低了视图与业务逻辑间的双向偶合。

MVC不是一种设计模式，MVC是一种架构模式。当然不同的MVC存在差异。

- Model（模型）：数据模型，提供要展示的数据，因此包含数据和行为，可以认为是领域模型或JavaBean组件（包含数据和行为），不过现在一般都分离开来：Value Object（数据Dao） 和 服务层（行为Service）。也就是模型提供了模型数据查询和模型数据的状态更新等功能，包括数据和业务。

- View（视图）：负责进行模型的展示，一般就是我们见到的用户界面，客户想看到的东西。

- Controller（控制器）：接收用户请求，委托给模型进行处理（状态改变），处理完毕后把返回的模型数据返回给视图，由视图负责展示。也就是说控制器做了个调度员的工作。

最典型的MVC就是JSP + servlet + javabean的模式。

<img src="https://gitee.com/feigeCode/picture/raw/master/img/www.yujzw.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg" alt="img" style="zoom:150%;" />

> ## Model1时代

在web早期的开发中，通常采用的都是Model1。

Model1中，主要分为两层，视图层和模型层。

![img](https://gitee.com/feigeCode/picture/raw/master/img/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy91SkRBVUtyR0M3S3dQT1BXcTAwcE1KaWFLODZsRjZCaklXZThSUGNDVWVleG9qQmlhUHRZN0hpYlFvblMzUGRDeTk4b1YyNEYwdFlrOEl4RVVZNDNOOTNUQS82NDA)

Model1优点：架构简单，比较适合小型项目开发；

Model1缺点：JSP职责不单一，职责过重，不便于维护；

> ## Model2时代

Model2把一个项目分成三部分，包括视图、控制、模型。

![img](https://gitee.com/feigeCode/picture/raw/master/img/img.jpeg)

用户发请求，Servlet接收请求数据，并调用对应的业务逻辑方法，业务处理完毕，返回更新后的数据给servlet，servlet转向到JSP，由JSP来渲染页面，响应给前端更新后的页面

职责分析：

- **Controller**：控制器：取得表单数据，调用业务逻辑，转向指定的页面

- **Model**：模型：业务逻辑，保存数据的状态

- **View**：视图：显示页面

Model2这样不仅提高的代码的复用率与项目的扩展性，且大大降低了项目的维护成本。Model 1模式的实现比较简单，适用于快速开发小规模项目，Model1中JSP页面身兼View和Controller两种角色，将控制逻辑和表现逻辑混杂在一起，从而导致代码的重用性非常低，增加了应用的扩展性和维护的难度。Model2消除了Model1的缺点。

# 二、SpringMVC概念


Spring Web MVC是基于Servlet API构建的原始Web框架，并且从一开始就已包含在Spring框架中。正式名称“ Spring Web MVC”来自其源模块（[`spring-webmvc`](https://github.com/spring-projects/spring-framework/tree/master/spring-webmvc)）的名称，但它通常被称为“ Spring MVC”。

[查看官方文档](https://docs.spring.io/spring-framework/docs/5.2.10.RELEASE/spring-framework-reference/web.html#spring-web)

我们为什么要学习SpringMVC呢?

Spring MVC的特点：

- 轻量级，简单易学

- 高效 , 基于请求响应的MVC框架

- 与Spring兼容性好，无缝结合

- 约定优于配置

- 功能强大：RESTful、数据验证、格式化、本地化、主题等

- 简洁灵活

- Spring的web框架围绕DispatcherServlet [ 调度Servlet ] 设计。

- DispatcherServlet的作用是将请求分发到不同的处理器。从Spring 2.5开始，使用Java 5或者以上版本的用户可以采用基于注解形式进行开发，十分简洁；

- 能够进行简单的junit测试 ， 支持Restful风格，异常处理，本地化，国际化， 数据验证，类型转换 ，拦截器等等

中心控制器
Spring的web框架围绕DispatcherServlet设计。DispatcherServlet的作用是将请求分发到不同的处理器。从Spring 2.5开始，使用Java 5或者以上版本的用户可以采用基于注解的controller声明方式。

Spring MVC框架像许多其他MVC框架一样, 以请求为驱动 , 围绕一个中心Servlet分派请求及提供其他功能，DispatcherServlet是一个实际的Servlet (它继承自HttpServlet 基类)。

![DispatcherServlet](https://gitee.com/feigeCode/picture/raw/master/img/DispatcherServlet.png)




## SpringMVC执行原理

当发起请求时被前置的控制器拦截到请求，根据请求参数生成代理请求，找到请求对应的实际控制器，控制器处理请求，创建数据模型，访问数据库，将模型响应给中心控制器，控制器使用模型与视图渲染视图结果，将结果返回给中心控制器，再将结果返回给请求者。

![img](https://gitee.com/feigeCode/picture/raw/master/img/249993-20161212142542042-2117679195.jpg)

## 执行流程

DispatcherServlet表示前置控制器，是整个SpringMVC的控制中心。用户发出请求，DispatcherServlet接收请求并拦截请求。

我们假设请求的url为 : http://localhost:8080/SpringMVC/hello

如上url拆分成三部分：

http://localhost:8080服务器域名

SpringMVC部署在服务器上的web站点

hello表示控制器

通过分析，如上url表示为：请求位于服务器localhost:8080上的SpringMVC站点的hello控制器。

HandlerMapping为处理器映射。DispatcherServlet调用HandlerMapping,HandlerMapping根据请求url查找Handler。

HandlerExecution表示具体的Handler,其主要作用是根据url查找控制器，如上url被查找控制器为：hello。

HandlerExecution将解析后的信息传递给DispatcherServlet,如解析控制器映射等。

HandlerAdapter表示处理器适配器，其按照特定的规则去执行Handler。

Handler让具体的Controller执行。

Controller将具体的执行信息返回给HandlerAdapter,如ModelAndView。

HandlerAdapter将视图逻辑名或模型传递给DispatcherServlet。

DispatcherServlet调用视图解析器(ViewResolver)来解析HandlerAdapter传递的逻辑视图名。

视图解析器将解析的逻辑视图名传给DispatcherServlet。

DispatcherServlet根据视图解析器解析的视图结果，调用具体的视图。

最终视图呈现给用户。
