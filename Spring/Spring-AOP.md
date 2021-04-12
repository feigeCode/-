# Spring-AOP

# 1、代理模式

为什么要学习代理模式，因为AOP的底层机制就是动态代理！

代理模式：

- 静态代理
- 动态代理

学习aop之前 , 我们要先了解一下代理模式！

## 静态代理

**静态代理角色分析**

- 抽象角色 : 一般使用接口或者抽象类来实现
- 真实角色 : 被代理的角色
- 代理角色 : 代理真实角色 ; 代理真实角色后 , 一般会做一些附属的操作 .
- 客户 : 使用代理角色来进行一些操作 .

**代码实现**

```java
package com.feige.staticproxy;

/**
 * @author feige<br />
 * @ClassName: Rent <br/>
 * @Description: <br/>
 * @date: 2021/4/8 19:07<br/>
 */
public interface Rent {

    /**
     * 租房
     */
    void rent();
}
```

```java
package com.feige.staticproxy;

/**
 * @author feige<br />
 * @ClassName: Host <br/>
 * @Description: <br/>
 * @date: 2021/4/8 19:08<br/>
 */
public class Host implements Rent {

    @Override
    public void rent() {
        System.out.println("房屋出租");
    }
}
```

```java
package com.feige.staticproxy;


/**
 * @author feige<br />
 * @ClassName: Proxy <br/>
 * @Description: <br/>
 * @date: 2021/4/8 19:09<br/>
 */
public class Proxy implements Rent {

    private Host host;

    public Proxy(Host host) {
        this.host = host;
    }

    @Override
    public void rent() {
        seeHouse();
        host.rent();
        fare();
    }

    public void seeHouse(){
        System.out.println("看房");
    }

    public void fare(){
        System.out.println("收中介费");
    }
}
```

~~~java
package com.feige.staticproxy;

/**
 * @author feige<br />
 * @ClassName: Client <br/>
 * @Description: <br/>
 * @date: 2021/4/8 19:11<br/>
 */
public class Client {

    public static void main(String[] args) {
        Rent proxy = new Proxy(new Host());
        proxy.rent();
    }
}

~~~

**静态代理的好处**

- 可以使得我们的真实角色更加纯粹 . 不再去关注一些公共的事情 .
- 公共的业务由代理来完成 . 实现了业务的分工 ,
- 公共业务发生扩展时变得更加集中和方便 .

缺点 :

- 类多了 , 多了代理类 , 工作量变大了 . 开发效率降低 .

我们想要静态代理的好处，又不想要静态代理的缺点，所以 , 就有了动态代理 !

## 动态代理

- 动态代理的角色和静态代理的一样 .
- 动态代理的代理类是动态生成的 . 静态代理的代理类是我们提前写好的
- 动态代理分为两类 : 一类是基于接口动态代理 , 一类是基于类的动态代理
	- 基于接口的动态代理----JDK动态代理
	- 基于类的动态代理--cglib
	- 现在用的比较多的是 javasist 来生成动态代理 . 百度一下javasist
	- 我们这里使用JDK的原生代码来实现，其余的道理都是一样的！

JDK的动态代理需要了解两个类

核心 : InvocationHandler 和 Proxy ， 打开JDK帮助文档看看

`InvocationHandler`是通过一个代理实例零调用处理程序实现的接口。 

```
Object invoke(Object proxy,
              Method method,
              Object[] args)
       throws Throwable
```

在代理实例上处理方法调用，并返回结果。当在与它关联的代理实例上调用方法时，该方法将在调用处理程序上调用此方法。 

- 参数 

	`proxy` -代理实例，调用的方法 

	`method ` -对接口方法的调用相应的 `method`代理实例的 `方法`实例。声明类的  `方法`对象将接口的方法声明，这可能是一个超接口代理接口，代理类继承的方法。 

	`args` -包含在方法上通过代理实例调用的实参的值对象的数组，或  `null`如果接口方法不需要参数。原始类型的实参是包裹在适当的原始包装类的实例，如  `java.lang.Integer`或 `java.lang.Boolean`。 

- 结果 

	在代理实例上的方法调用返回的值。如果声明的返回类型的接口方法是比较原始的类型，则该方法返回的值必须是相应的原始包装类的一个实例；否则，它必须是一个类型分配给声明的返回类型。如果此方法返回的值是  `null`和接口方法的返回类型是原始的，然后  `NullPointerException`将由法对代理实例调用抛出。如果该方法返回的值是不兼容的接口的方法声明的返回类型如上所述，一个  `ClassCastException`将由法对代理实例调用抛出。 

- 异常 

	`Throwable`  -把从方法上代理实例调用的例外。异常的类型必须是可转让或任何的异常类型的接口方法的 `throws`条款或对未检查的异常类型  `java.lang.RuntimeException`或  `java.lang.Error`宣布。如果用这种方法，是不可转让的任何异常类型的接口方法的  `throws`条款声明抛出检查异常，然后 `UndeclaredThrowableException`包含异常，通过该方法抛出将由法对代理实例调用抛出。 

- 另请参见： 

`UndeclaredThrowableException`



```java
package com.feige.dynamicproxy;

/**
 * @author feige<br />
 * @ClassName: Rent <br/>
 * @Description: <br/>
 * @date: 2021/4/8 19:07<br/>
 */
public interface Rent {

    /**
     * 租房
     */
    void rent();
}
```



```java
package com.feige.dynamicproxy;

/**
 * @author feige<br />
 * @ClassName: Host <br/>
 * @Description: <br/>
 * @date: 2021/4/8 19:08<br/>
 */
public class Host implements Rent {

    @Override
    public void rent() {
        System.out.println("房屋出租");
    }
}
```



```java
package com.feige.dynamicproxy;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

/**
 * @author feige<br />
 * @ClassName: ProxyInvocationHandler <br/>
 * @Description: <br/>
 * @date: 2021/4/8 21:36<br/>
 */
public class ProxyInvocationHandler implements InvocationHandler {

    private Object target;

    public ProxyInvocationHandler(Object target) {
        this.target = target;
    }

    /**
     * @description: 生成代理类，重点是第二个参数，获取要代理的抽象角色！之前都是一个角色，现在可以代理一类角色
     * @author: feige
     * @date: 2021/4/8 21:47
      * @param
     * @return: com.feige.dynamicproxy.Rent
     */
    public Object getProxy(){
        return Proxy.newProxyInstance(this.getClass().getClassLoader(),
                target.getClass().getInterfaces(),this);
    }


    /**
     * @description: 处理代理实例上的方法调用并返回结果
     * @author: feige
     * @date: 2021/4/8 21:46
     * @param proxy : 代理类 method :
     * @param	method	代理类的调用处理程序的方法对象
     * @param	args 参数
     * @return: java.lang.Object
     */
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        seeHouse();
 
        // 本质是通过反射实现的
        Object invoke = method.invoke(target, args);
        fare();
        return invoke;
    }

    public void seeHouse(){
        System.out.println("看房");
    }

    public void fare(){
        System.out.println("收中介费");
    }
}

```

```java
package com.feige.dynamicproxy;

/**
 * @author feige<br />
 * @ClassName: Client <br/>
 * @Description: <br/>
 * @date: 2021/4/8 21:42<br/>
 */
public class Client {

    public static void main(String[] args) {
        ProxyInvocationHandler handler = new ProxyInvocationHandler(new Host());
        Rent proxy = (Rent)handler.getProxy();
        proxy.rent();
    }
}
```

核心：**一个动态代理 , 一般代理某一类业务 , 一个动态代理可以代理多个类，代理的是接口！、**



```java
package com.feige.dynamicproxy;

/**
 * @description: 抽象角色：增删改查业务
 * @author: feige
 * @date: 2021/4/8 22:04
 */
public interface UserService {
    void add();
    void delete();
    void update();
    void query();
}
```





```java
package com.feige.dynamicproxy;

/**
 * @description: 真实对象，完成增删改查操作的人
 * @author: feige
 * @date: 2021/4/8 22:04
 */
public class UserServiceImpl implements UserService {

    @Override
    public void add() {
        System.out.println("增加了一个用户");
    }

    @Override
    public void delete() {
        System.out.println("删除了一个用户");
    }

    @Override
    public void update() {
        System.out.println("更新了一个用户");
    }

    @Override
    public void query() {
        System.out.println("查询了一个用户");
    }
}
```

添加一个日志方法

```java
package com.feige.dynamicproxy;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

/**
 * @author feige<br />
 * @ClassName: ProxyInvocationHandler <br/>
 * @Description: <br/>
 * @date: 2021/4/8 21:36<br/>
 */
public class ProxyInvocationHandler implements InvocationHandler {

    private Object target;

    public ProxyInvocationHandler(Object target) {
        this.target = target;
    }

    /**
     * @description: 生成代理类，重点是第二个参数，获取要代理的抽象角色！之前都是一个角色，现在可以代理一类角色
     * @author: feige
     * @date: 2021/4/8 21:47
      * @param
     * @return: com.feige.dynamicproxy.Rent
     */
    public Object getProxy(){
        return Proxy.newProxyInstance(this.getClass().getClassLoader(),
                target.getClass().getInterfaces(),this);
    }


    /**
     * @description: 处理代理实例上的方法调用并返回结果
     * @author: feige
     * @date: 2021/4/8 21:46
     * @param proxy : 代理类 method :
     * @param  method 代理类的调用处理程序的方法对象
     * @param  args 参数
     * @return: java.lang.Object
     */
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
  
        log(method.getName());
        // 本质是通过反射实现的
        Object invoke = method.invoke(target, args);
        return invoke;
    }


    public void log(String methodName){
        System.out.println("执行了"+methodName+"方法");
    }
}
```





```java
package com.feige.dynamicproxy;

/**
 * @author feige<br />
 * @ClassName: Client <br/>
 * @Description: <br/>
 * @date: 2021/4/8 21:42<br/>
 */
public class Client {

    public static void main(String[] args) {
        ProxyInvocationHandler proxyInvocationHandler = new ProxyInvocationHandler(new UserServiceImpl());
        UserService proxy1 = (UserService)proxyInvocationHandler.getProxy();
        proxy1.add();
    }
}
```



**动态代理的好处**

静态代理有的它都有，静态代理没有的，它也有！

- 可以使得我们的真实角色更加纯粹 . 不再去关注一些公共的事情 .
- 公共的业务由代理来完成 . 实现了业务的分工 ,
- 公共业务发生扩展时变得更加集中和方便 .
- 一个动态代理 , 一般代理某一类业务
- 一个动态代理可以代理多个类，代理的是接口！

# 2、什么是AOP

**AOP**（Aspect Oriented Programming）意为：**面向切面编程**，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

## Aop在Spring中的作用

**提供声明式事务；允许用户自定义切面**

- 横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志 , 安全 , 缓存 , 事务等等 ....

- 切面（ASPECT）：把通知应用到切入点过程

- 通知（Advice）：实际增强的逻辑部分称为通知，且分为以下五种类型：

	- 前置通知 （before）
	- 后置通知 （afterReturning）
	- 环绕通知 （aRound）
	- 异常通知 （afterThrowing）
	- 最终通知（after）

- 目标（Target）：被通知对象。

- 代理（Proxy）：向目标对象应用通知之后创建的对象。

- 切入点（PointCut）：实际被真正增强的方法称为切入点

- 连接点（JointPoint）：类里面哪些方法可以被增强，这些方法称为连接点

## AOP操作



1. Spring 框架一般都是基于 AspectJ 实现 AOP 操作，AspectJ 不是 Spring 组成部分，独立 AOP 框架，一般把 AspectJ 和 Spirng 框架一起使 用，进行 AOP 操作

2. 基于 AspectJ 实现 AOP 操作：1）基于 xml 配置文件实现 （2）基于注解方式实现（使用）
3. 引入相关jar包

~~~xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.10.RELEASE</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
~~~



4. 切入点表达式，如下：

（1）切入点表达式作用：知道对哪个类里面的哪个方法进行增强 
（2）语法结构： execution([权限修饰符] [返回类型] [类全路径] [方法名称] ([参数列表]) )
（3）例子如下：

~~~java
例 1：对 com.atguigu.dao.BookDao 类里面的 add 进行增强
execution(* com.atguigu.dao.BookDao.add(..))
例 2：对 com.atguigu.dao.BookDao 类里面的所有的方法进行增强
execution(* com.atguigu.dao.BookDao.* (..))
例 3：对 com.atguigu.dao 包里面所有类，类里面所有方法进行增强
execution(* com.atguigu.dao.*.* (..))
~~~



## 实操

#### 基于xml配置方式

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">

    <bean id="myProxy" class="com.feige.aop.service.UserServiceProxy"/>
    <bean id="userServiceImpl" class="com.feige.aop.service.UserServiceImpl"/>
    <aop:aspectj-autoproxy/>

    <aop:config>
        <aop:aspect ref="myProxy">
            <aop:pointcut id="p" expression="execution(* com.feige.aop.service.UserServiceImpl.*(..))"/>
            <aop:around method="aRound" pointcut-ref="p"/>
            <aop:before method="before" pointcut-ref="p"/>
            <aop:after-returning method="afterReturning" pointcut-ref="p"/>
            <aop:after method="after" pointcut-ref="p"/>
            <aop:after-throwing method="afterThrowing" pointcut-ref="p"/>
        </aop:aspect>
    </aop:config>

</beans>
```





```java
package com.feige.aop.service;

import org.aspectj.lang.ProceedingJoinPoint;

/**
 * @author feige<br />
 * @ClassName: UserServiceProxy <br/>
 * @Description: <br/>
 * @date: 2021/4/9 9:19<br/>
 */

public class UserServiceProxyXml {


    /**
     * @description: 前置通知
     * @author: feige
     * @date: 2021/4/9 9:31
     * @return: void
     */
    public void before(){
        System.out.println("before()前置通知");
    }

    /**
     * @description: 最终通知
     * @author: feige
     * @date: 2021/4/9 9:31
     * @return: void
     */
    public void after(){
        System.out.println("after()最终通知");
    }

    /**
     * @description: 后置通知
     * @author: feige
     * @date: 2021/4/9 9:32
     * @return: void
     */
    public void afterReturning(){
        System.out.println("afterReturning()后置通知");
    }

    /**
     * @description: 环绕通知
     * @author: feige
     * @date: 2021/4/9 9:32
     * @param  pjp
     * @return: java.lang.Object
     */
    public Object aRound(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("环绕之前.........");
        //被增强的方法执行
        pjp.proceed();
        System.out.println("环绕之后.........");
        return null;
    }

    /**
     * @description: 抛出异常
     * @author: feige
     * @date: 2021/4/9 9:33
     * @return: void
     */
    public void afterThrowing(){
        System.out.println("抛出异常");
    }
}
```



```java
package com.feige.aop.spring;

import com.feige.aop.service.UserService;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * @author feige<br />
 * @ClassName: SpringAopTest <br/>
 * @Description: <br/>
 * @date: 2021/4/8 22:25<br/>
 */
public class SpringAopTest {

    @Test
    public void test2(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext2.xml");
        UserService service = context.getBean(UserService.class);
        service.add();
    }
}
```

#### 基于注解配置方式

```java
package com.feige.aop.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

/**
 * @author feige<br />
 * @ClassName: AopConfig <br/>
 * @Description: <br/>
 * @date: 2021/4/8 22:36<br/>
 */
@Configuration
@EnableAspectJAutoProxy
@ComponentScan(basePackages = {"com.feige.aop"})
public class AopConfig {
}
```





```java
package com.feige.aop.service;

/**
 * @description: 抽象角色：增删改查业务
 * @author: feige
 * @date: 2021/4/8 22:04
 */
public interface UserService {
    void add();
    void delete();
    void update();
    void query();
}
```



```java
package com.feige.aop.service;

import org.springframework.stereotype.Service;

/**
 * @description: 真实对象，完成增删改查操作的人
 * @author: feige
 * @date: 2021/4/8 22:04
 */
@Service
public class UserServiceImpl implements UserService {

    @Override
    public void add() {
        //int a = 1/0;
        System.out.println("增加了一个用户");
    }

    @Override
    public void delete() {
        System.out.println("删除了一个用户");
    }

    @Override
    public void update() {
        System.out.println("更新了一个用户");
    }

    @Override
    public void query() {
        System.out.println("查询了一个用户");
    }
}
```





```java
package com.feige.aop.service;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

/**
 * @author feige<br />
 * @ClassName: UserServiceProxy <br/>
 * @Description: <br/>
 * @date: 2021/4/9 9:19<br/>
 */
@Aspect
@Component
public class UserServiceProxy {

    /**
     * @description: 相同切入点抽取
     * @author: feige
     * @date: 2021/4/9 9:31
     * @return: void
     */
    @Pointcut(value = "execution(* com.feige.aop.service.UserServiceImpl.*(..))")
    public void pointCut(){

    }

    /**
     * @description: 前置通知
     * @author: feige
     * @date: 2021/4/9 9:31
     * @return: void
     */
    @Before(value = "pointCut()")
    public void before(){
        System.out.println("before()前置通知");
    }

    /**
     * @description: 最终通知
     * @author: feige
     * @date: 2021/4/9 9:31
     * @return: void
     */
    @After(value = "pointCut()")
    public void after(){
        System.out.println("after()最终通知");
    }

    /**
     * @description: 后置通知
     * @author: feige
     * @date: 2021/4/9 9:32
     * @return: void
     */
    @AfterReturning(value = "pointCut()")
    public void afterReturning(){
        System.out.println("afterReturning()后置通知");
    }

    /**
     * @description: 环绕通知
     * @author: feige
     * @date: 2021/4/9 9:32
     * @param  pjp
     * @return: java.lang.Object
     */
    @Around(value = "pointCut()")
    public Object aRound(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("环绕之前.........");
        //被增强的方法执行
        pjp.proceed();
        System.out.println("环绕之后.........");
        return null;
    }

    /**
     * @description: 抛出异常
     * @author: feige
     * @date: 2021/4/9 9:33
     * @return: void
     */
    @AfterThrowing(value = "pointCut()")
    public void afterThrowing(){
        System.out.println("抛出异常");
    }
}
```





```java
package com.feige.aop.spring;

import com.feige.aop.service.UserService;
import org.junit.Test;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

/**
 * @author feige<br />
 * @ClassName: SpringAopTest <br/>
 * @Description: <br/>
 * @date: 2021/4/8 22:25<br/>
 */
public class SpringAopTest {

    @Test
    public void test1(){
        ApplicationContext context = new AnnotationConfigApplicationContext("com.feige.aop");
        UserService service = context.getBean(UserService.class);
        service.add();
    }
}
```

无异常时

~~~txt
环绕之前.........
before()前置通知
增加了一个用户
afterReturning()后置通知
after()最终通知
环绕之后.........
~~~

有异常时

~~~txt
环绕之前.........
before()前置通知
抛出异常
after()最终通知
~~~



## 总结

无异常时方法的执行循序

1.  环绕之前的方法
2.  前置通知
3.  义务方法
4.  后置通知
5.  最终通知
6.  环绕之后的方法

有异常时

1. 环绕之前的方法
2. 前置通知
3. 抛出异常
4. 最终通知