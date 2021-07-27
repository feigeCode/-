

### 简介

- Spring : 春天 --->给软件行业带来了春天
- 2002年，Rod Jahnson首次推出了Spring框架雏形interface21框架。
- 2004年3月24日，Spring框架以interface21框架为基础，经过重新设计，发布了1.0正式版。
- 很难想象Rod Johnson的学历 , 他是悉尼大学的博士，然而他的专业不是计算机，而是音乐学。
- Spring理念 : 使现有技术更加实用 . 本身就是一个大杂烩 , 整合现有的框架技术

官网 : http://spring.io/

官方下载地址 : https://repo.spring.io/libs-release-local/org/springframework/spring/

GitHub : https://github.com/spring-projects

### 优点

- Spring是一个开源免费的框架 , 容器 .
- Spring是一个轻量级的框架 , 非侵入式的 .
- **控制反转 IoC , 面向切面 Aop**
- 对事物的支持 , 对框架的支持

一句话概括：**Spring是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器（框架）。**

Spring 总共大约有 20 个模块， 由 1300 多个不同的文件构成。 而这些组件被分别整合在核心容器（Core Container） 、 AOP（Aspect Oriented Programming）和设备支持（Instrmentation） 、数据访问与集成（Data Access/Integeration） 、 Web、 消息（Messaging） 、 Test等 6 个模块中。 以下是 Spring 5 的模块结构图：

![img](https://gitee.com/feigeCode/picture/raw/master/img/2019102923475419.png)

都可以单独存在，或者与其他一个或多个模块联合实现。每个模块的功能如下：

- **核心容器**：核心容器提供 Spring 框架的基本功能。核心容器的主要组件是 `BeanFactory`，它是工厂模式的实现。`BeanFactory` 使用*控制反转*（IOC） 模式将应用程序的配置和依赖性规范与实际的应用程序代码分开。
- **Spring context**：Spring 上下文是一个配置文件，向 Spring 框架提供上下文信息。Spring 上下文包括企业服务，例如 JNDI、EJB、电子邮件、国际化、校验和调度功能。
- **Spring AOP**：通过配置管理特性，Spring AOP 模块直接将面向切面的编程功能 , 集成到了 Spring 框架中。所以，可以很容易地使 Spring 框架管理任何支持 AOP的对象。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖组件，就可以将声明性事务管理集成到应用程序中。
- **Spring DAO**：JDBC DAO 抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理和不同数据库供应商抛出的错误消息。异常层次结构简化了错误处理，并且极大地降低了需要编写的异常代码数量（例如打开和关闭连接）。Spring DAO 的面向 JDBC 的异常遵从通用的 DAO 异常层次结构。
- **Spring ORM**：Spring 框架插入了若干个 ORM 框架，从而提供了 ORM 的对象关系工具，其中包括 JDO、Hibernate 和 iBatis SQL Map。所有这些都遵从 Spring 的通用事务和 DAO 异常层次结构。
- **Spring Web 模块**：Web 上下文模块建立在应用程序上下文模块之上，为基于 Web 的应用程序提供了上下文。所以，Spring 框架支持与 Jakarta Struts 的集成。Web 模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。
- **Spring MVC 框架**：MVC 框架是一个全功能的构建 Web 应用程序的 MVC 实现。通过策略接口，MVC 框架变成为高度可配置的，MVC 容纳了大量视图技术，其中包括 JSP、Velocity、Tiles、iText 和 POI。



# 一、Spring IOC（控制反转）

##  1、IOC容器

- 什么是IOC：把对象创建和对象之间的调用过程，交给Spring进行管理

- 使用IOC目的：为了降低耦合度

- IOC底层：xml解析、工厂模式、反射

- Spring提供的IOC容器实现的两种方式（两个接口）

	- BeanFactory接口：IOC容器基本实现是Spring内部接口的使用接口，不提供给开发人员进行使用（加载配置文件时候不会创建对象，在获取对象时才会创建对象。）

	- ApplicationContext接口：BeanFactory接口的子接口，提供更多更强大的功能，提供给开发人员使用（加载配置文件时候就会把在配置文件对象进行创建）推荐使用！

- ApplicationContext接口的实现类

## 2、IOC管理bean

- Spring创建对象
- Spring注入属性：set方式注入，构造器注入方式，p命名空间，c命名空间

### 2.1、基于xml的配置方式

首先创建一个maven项目（其他项目也可以），导入jar包

~~~xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.10.RELEASE</version>
    </dependency>
</dependencies>
~~~



Class.java

~~~java
package com.feige.pojo;

/**
 * @author feige<br />
 * @ClassName: Class <br/>
 * @Description: <br/>
 * @date: 2021/4/8 14:57<br/>
 */
public class Class {

    private String classId;

    private String name;
    ...省略get set方法
}
~~~



Student.java

~~~java
package com.feige.pojo;

import java.util.*;

/**
 * @author feige<br />
 * @ClassName: Student <br/>
 * @Description: <br/>
 * @date: 2021/4/8 11:26<br/>
 */

public class Student {

    private Integer id;

    private String name;

    private String age;

    private Class aClass;

    private String address;

    private String[] books;

    private List<String> hobbies;

    private Map<String,String> card;

    private Set<String> games;

    private String wife;

    private Properties info;

    public Student() {
    }

    public Student(Integer id, String name, String age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    ...省略get set方法
}

~~~



resource目录下创建 applicationContext.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
>
    <!--团队合作 导入配置文件-->
    <!--<import resource="{path}/beans.xml"/>-->

    <bean id="class" class="com.feige.pojo.Class">
        <property name="name" value="20181431"/>
        <property name="classId" value="14"/>
    </bean>
    <!--set方式注入-->
    <bean id="student" class="com.feige.pojo.Student">
        <property name="id" value="1"/>
        <property name="name" value="feige"/>
        <!--（1）null值-->
        <property name="wife">
            <null/>
        </property>
        <!--（2）特殊符号赋值-->
        <!--属性值包含特殊符号 1. 把<>进行转义 &lt; &gt; 2. 把带特殊符号内容写到CDATA-->
        <property name="address">
            <value><![CDATA[<<云南>>]]></value>
        </property>
        <property name="age" value="20"/>
        <!-- 注入外部bean-->
        <property name="aClass" ref="class"/>
        <!--        注入数组-->
        <property name="books">
            <array>
                <value>Java编程思想</value>
                <value>Java并发编程之美</value>
                <value>Go核心编程</value>
            </array>
        </property>

        <!--        注入列表-->
        <property name="hobbies">
            <list>
                <value>编程</value>
                <value>跑步</value>
                <value>睡觉</value>
            </list>
        </property>

        <!--        注入map-->
        <property name="card">
            <map>
                <entry key="feige" value="hufei"/>
                <entry key="feige1" value="hufei1"/>
            </map>
        </property>
        <!--        注入set-->
        <property name="games">
            <set>
                <value>王者荣耀</value>
                <value>吃鸡</value>
            </set>
        </property>
        <!--        注入properties-->
        <property name="info">
            <props>
                <prop key="username">root</prop>
                <prop key="password">123456</prop>
            </props>
        </property>
    </bean>


    <!--构造函数注入-->
    <bean id="student2" class="com.feige.pojo.Student">
        <constructor-arg name="id" value="2"/>
        <constructor-arg name="name" value="feige1"/>
        <constructor-arg name="age" value="21"/>
    </bean>


    <!--p命名空间注入 导入约束xmlns:p="http://www.springframework.org/schema/p-->
    <bean id="student3" class="com.feige.pojo.Student" p:id="3" p:name="feige3" p:age="21"/>

    <!--c命名空间注入 导入约束xmlns:c="http://www.springframework.org/schema/c-->
    <!--C(构造: Constructor)命名空间 , 属性依然要设置set方法-->
    <bean id="student4" class="com.feige.pojo.Student" c:name="feige4" c:id="4" c:age="10"/>
</beans>
~~~

SpringDemo1.java

~~~java
package com.feige.spring;

import com.feige.pojo.Student;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * @author feige<br />
 * @ClassName: SpringDemo1 <br/>
 * @Description: <br/>
 * @date: 2021/4/8 11:25<br/>
 */
public class SpringDemo1 {

    public static void main(String[] args) {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        Student student = applicationContext.getBean("student");
        System.out.println(student);
    }
}

~~~

#### 1、xml配置方式的自动装配

##### byName

**autowire byName (按名称自动装配)**

- 将查找其类中所有的set方法名，获得将set去掉并且首字母小写的字符串

- 去spring容器中寻找是否有此字符串名称id的对象。

- 如果有，就取出注入；如果没有，就报空指针异常。

修改bean配置，增加一个属性 autowire="byName"

```xml
<bean id="aClass" class="com.feige.pojo.Class">
    <property name="name" value="20181431"/>
    <property name="classId" value="14"/>
</bean>

<bean id="student" class="com.feige.pojo.Student" autowire="byName">
    <property name="id" value="1"/>
    <property name="name" value="feige"/>
</bean>

```

##### byType

**autowire byType (按类型自动装配)**

使用autowire byType首先需要保证：同一类型的对象，在spring容器中唯一。如果不唯一，会报不唯一（`NoUniqueBeanDefinitionException`）的异常。

```java
<bean id="class" class="com.feige.pojo.Class">
    <property name="name" value="20181431"/>
    <property name="classId" value="14"/>
</bean>
<bean id="student1" class="com.feige.pojo.Student" autowire="byType">
    <property name="id" value="1"/>
    <property name="name" value="feige"/>
</bean>
```

### 2.2、基于注解的配置方式

#### **1、什么是注解**

 （1）注解是代码特殊标记，格式：@注解名称(属性名称=属性值, 属性名称=属性值…)

 （2）使用注解，注解作用在类上面，方法上面，属性上面

 （3）使用注解目的：简化 xml 配置

#### 2、创建bean实例

下面四个注解功能是一样的，都可以用来创建 bean 实例

- @Component 通用的

- @Service 一般用在义务层

- @Controller 一般用在前端控制层

- @Repository 一般用在dao层

#### 2、注解配置的自动装配

- **@Autowired**由spring提供的
	- 首先通过类型来查找bean，如果自找到一个，则直接注入，如果没有找到，则抛出异常，如果找到多个bean也会抛出异常
	- 可以在配置bean的时候加上@Primary注解，来提高优先级，这样就不会报错
	- 会默认使用字段名来匹配，如果没有匹配上，抛出异常。如果需要直接通过bean的id来查找，可以配合**@Qualifier**来使用，没有找到抛出异常
	- 这里重点需要指出，使用@Autowired 是有优先级的，@Qualifier > 按类型找（如果找到多个继续使用之后的策略） > @Primary > 按名字找
	- 通过设置required=false，在没有找到bean的情况下，不会抛出异常
- **@Resource**由java语言提供的，使用该注解能实现对spring的解耦
	- 如果注解中设置了name属性，直接通过id去查找，如果没有，抛出异常；如果注解中没有设置name属性，默认先通过字段名（或参数名）去查找，没有找到，再通过类型去查找，如果查找到0个或多个，抛出异常，注意：这里不支持使用@Primary来解决一个类型查找到多个bean的情况。



**@Value**：注入普通类型属性

```java
@Value(value = "abc")
private String name
```

方式一：ApplicationConfig.java

```java
package com.feige.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

/**
 * @author feige<br />
 * @ClassName: ApplicationConfig <br/>
 * @Description: 完全注解开发，替代xml<br/>
 * @date: 2021/4/8 16:23<br/>
 */
@Configuration
@ComponentScan(basePackages = {"com.feige"})
public class ApplicationConfig {


}
```

方式二：

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--开启组件扫描
     1 如果扫描多个包，多个包使用逗号隔开
     2 扫描包上层目录
    -->
    <context:component-scan base-package="com.feige"/>
    
</beans>
```

```java
package com.feige.dao;


/**
 * @author feige<br />
 * @ClassName: UserDao <br/>
 * @Description: <br/>
 * @date: 2021/4/8 16:04<br/>
 */

public interface UserDao {

    void add();

    void delete();

    void select();

    void update();
}
```



```java
package com.feige.dao.impl;

import com.feige.dao.UserDao;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Repository;

/**
 * @author feige<br />
 * @ClassName: UserDaoImpl <br/>
 * @Description: <br/>
 * @date: 2021/4/8 16:06<br/>
 */
//@Component(value = "userDao")也可以
@Repository("userDao")
public class UserDaoImpl implements UserDao {

    @Override
    public void add(){
        System.out.println("添加一个用户");
    }

    @Override
    public void delete(){
        System.out.println("删除一个用户");
    }

    @Override
    public void select(){
        System.out.println("查询一个用户");
    }

    @Override
    public void update(){
        System.out.println("修改一个用户");
    }
}

```



```java
package com.feige.service;



/**
 * @author feige<br />
 * @ClassName: UserService <br/>
 * @Description: <br/>
 * @date: 2021/4/8 16:15<br/>
 */
public interface UserService {

    void add();

    void delete();

    void select();

    void update();
}
```





```
package com.feige.service.impl;

import com.feige.dao.UserDao;
import com.feige.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Component;
import org.springframework.stereotype.Service;

import javax.annotation.Resource;

/**
 * @author feige<br />
 * @ClassName: UserServiceImpl <br/>
 * @Description: <br/>
 * @date: 2021/4/8 16:16<br/>
 */
@Service
//@Component 也可以
public class UserServiceImpl implements UserService {

    //@Resource(name = "userDao")也可以
    @Autowired
    @Qualifier("userDao")
    private UserDao userDao;


    @Override
    public void add() {
        userDao.add();
    }

    @Override
    public void delete() {
        userDao.delete();
    }

    @Override
    public void select() {
        userDao.select();
    }

    @Override
    public void update() {
        userDao.update();
    }
}

```



```java
@Test
public void test2() {
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext1.xml");
    UserService userService = applicationContext.getBean(UserService.class);
    userService.add();
}


@Test
public void test3() {
    ApplicationContext applicationContext = new AnnotationConfigApplicationContext(ApplicationConfig.class);
    UserService userService = applicationContext.getBean(UserService.class);
    userService.add();
}
```

## 3、Bean的作用域

在Spring中，那些组成应用程序的主体及由Spring IoC容器所管理的对象，被称之为bean。简单地讲，bean就是由IoC容器初始化、装配及管理的对象 .

几种作用域中，request、session作用域仅在基于web的应用中使用（不必关心你所采用的是什么web应用框架），只能用在基于web的Spring ApplicationContext环境。

### Singleton

当一个bean的作用域为Singleton，那么Spring IoC容器中只会存在一个共享的bean实例，并且所有对bean的请求，只要id与该bean定义相匹配，则只会返回bean的同一实例。Singleton是单例类型，就是在创建起容器时就同时自动创建了一个bean的对象，不管你是否使用，他都存在了，每次获取到的对象都是同一个对象。注意，Singleton作用域是Spring中的缺省作用域。要在XML中将bean定义成singleton，可以这样配置：

```xml
<bean id="student" class="com.feige.pojo.Student" scope="singleton">
    <property name="id" value="1"/>
    <property name="name" value="feige"/>
</bean>
```

测试：

```java
@Test
    public void test2() {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext1.xml");
        Student student = (Student) applicationContext.getBean("student");
        Student student1 = (Student) applicationContext.getBean("student");
        System.out.println(student == student1);
    }
```

### Prototype

当一个bean的作用域为Prototype，表示一个bean定义对应多个对象实例。Prototype作用域的bean会导致在每次对该bean请求（将其注入到另一个bean中，或者以程序的方式调用容器的getBean()方法）时都会创建一个新的bean实例。Prototype是原型类型，它在我们创建容器的时候并没有实例化，而是当我们获取bean的时候才会去创建一个对象，而且我们每次获取到的对象都不是同一个对象。根据经验，对有状态的bean应该使用prototype作用域，而对无状态的bean则应该使用singleton作用域。在XML中将bean定义成prototype，可以这样配置：

```xml
<bean id="student" class="com.feige.pojo.Student" scope="prototype">
    <property name="id" value="1"/>
    <property name="name" value="feige"/>
</bean>  
 或者
<bean id="student" class="com.feige.pojo.Student"  singleton="false">
    <property name="id" value="1"/>
    <property name="name" value="feige"/>
</bean>

```

### Request

当一个bean的作用域为Request，表示在一次HTTP请求中，一个bean定义对应一个实例；即每个HTTP请求都会有各自的bean实例，它们依据某个bean定义创建而成。该作用域仅在基于web的Spring ApplicationContext情形下有效。考虑下面bean定义：

```xml
<bean id="loginAction" class=cn.csdn.LoginAction" scope="request"/>
```

针对每次HTTP请求，Spring容器会根据loginAction bean的定义创建一个全新的LoginAction bean实例，且该loginAction bean实例仅在当前HTTP request内有效，因此可以根据需要放心的更改所建实例的内部状态，而其他请求中根据loginAction bean定义创建的实例，将不会看到这些特定于某个请求的状态变化。当处理请求结束，request作用域的bean实例将被销毁。

### Session

当一个bean的作用域为Session，表示在一个HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。考虑下面bean定义：

```xml
<bean id="userPreferences" class="com.foo.UserPreferences" scope="session"/>
```

针对某个HTTP Session，Spring容器会根据userPreferences bean定义创建一个全新的userPreferences bean实例，且该userPreferences bean仅在当前HTTP Session内有效。与request作用域一样，可以根据需要放心的更改所创建实例的内部状态，而别的HTTP Session中根据userPreferences创建的实例，将不会看到这些特定于某个HTTP Session的状态变化。当HTTP Session最终被废弃的时候，在该HTTP Session作用域内的bean也会被废弃掉。

## 4、bean的生命周期

1、生命周期 ：从对象创建到对象销毁的过程

2、bean 生命周期

 （1）通过构造器创建 bean 实例（无参数构造）

 （2）为 bean 的属性设置值和对其他 bean 引用（调用 set 方法）

 （3）调用 bean 的初始化的方法（需要进行配置初始化的方法）

 （4）bean 可以使用了（对象获取到了）

 （5）当容器关闭时候，调用 bean 的销毁的方法（需要进行配置销毁的方法）



```java
package com.feige.pojo;

public class Orders {


    private String oname;

    /**
     * @description: 无参数构造
     * @author: feige
     * @date: 2021/4/8 17:24
      * @param
     * @return:
     */
    public Orders() {
        System.out.println("第一步 执行无参数构造创建 bean 实例");
    }

    public void setOname(String oname) {
        this.oname = oname;
        System.out.println("第二步 调用 set 方法设置属性值");
    }

    /**
     * @description: 创建执行的初始化的方法
     * @author: feige
     * @date: 2021/4/8 17:23
     * @param
     * @return: void
     */
    public void initMethod() {
        System.out.println("第三步 执行初始化的方法");
    }

    /**
     * @description: 创建执行的销毁的方法
     * @author: feige
     * @date: 2021/4/8 17:24
      * @param
     * @return: void
     */
    public void destroyMethod() {
        System.out.println("第五步 执行销毁的方法");
    }
}
```



```java
package com.feige.pojo;

import org.springframewo
    rk.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;

/**
 * @author feige<br />
 * @ClassName: MyBeanPost <br/>
 * @Description: <br/>
 * @date: 2021/4/8 17:26<br/>
 */
public class MyBeanPost implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("在初始化之前执行的方法");
        return bean;
    }
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("在初始化之后执行的方法");
        return bean;
    }
}
```





```xml
<bean id="order" class="com.feige.pojo.Orders" init-method="initMethod" destroy-method="destroyMethod">
    <property name="oname" value="荣耀手机"/>
</bean>

<bean id="myBean" class="com.feige.pojo.MyBeanPost"/>
```





```java
@Test
public void test2() {
    ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
    Orders order = (Orders) applicationContext.getBean("order");
    System.out.println("第四步 获取创建 bean 实例对象");
    applicationContext.close();

}
```

~~~txt
第一步 执行无参数构造创建 bean 实例
第二步 调用 set 方法设置属性值
在初始化之前执行的方法
第三步 执行初始化的方法
在初始化之后执行的方法
第四步 获取创建 bean 实例对象
第五步 执行销毁的方法
~~~



bean 的后置处理器，bean 生命周期有七步 （正常生命周期为五步，而配置后置处理器后为七步）

 （1）通过构造器创建 bean 实例（无参数构造）

 （2）为 bean 的属性设置值和对其他 bean 引用（调用 set 方法）

 （3）把 bean 实例传递 bean 后置处理器的方法 postProcessBeforeInitialization

 （4）调用 bean 的初始化的方法（需要进行配置初始化的方法）

 （5）把 bean 实例传递 bean 后置处理器的方法 postProcessAfterInitialization

 （6）bean 可以使用了（对象获取到了）

 （7）当容器关闭时候，调用 bean 的销毁的方法（需要进行配置销毁的方法）
