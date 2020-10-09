# 注解

# 1、什么是注解

**百度百科介绍：**

​	注解，可以看作是对 一个 类/方法 的一个扩展的模版，每个 类/方法 按照注解类中的规则，来为 类/方法 注解不同的参数，在用到的地方可以得到不同的 类/方法 中注解的各种参数与值

注解也就是Annotation,相信不少人也和我之前一样以为和注释和doc一样，是一段辅助性的文字，其实注解不是这样的。

从JDK5开始，java增加了对元数据（描述数据属性的信息）的支持。其实说白就是代码里的特殊标志，这些标志可以在编译，类加载，运行时被读取，并执行相应的处理，以便于其他工具补充信息或者进行部署

# 2、基本的Annotation

java提供了5个基本的注解，分别是

- `@Override`：限定父类重写方法，放在子类的方法上，表示子类确实重写了父类的方法

- `@Deprecated`：标示已过时

- `@SuppressWarnings`：抑制编译器警告

- `@SafeVarargs`：堆污染“警告”

- `@FunctionalInterface`：函数式接口

# 3、元注解

元注解顾名思义我们可以理解为注解的注解，它是作用在注解中，方便我们使用注解实现想要的功能。元注解分别有

- `@Retention `

  - 表示注解存在阶段是保留在源码（编译期），字节码（类加载）或者运行期（JVM中运行）。在`@Retention`注解中使用枚举`RetentionPolicy`来表示注解保留时期

     - `@Retention(RetentionPolicy.SOURCE)`：注解仅存在于源码中，在class字节码文件中不包含
     - `@Retention(RetentionPolicy.CLASS)`： 默认的保留策略，注解会在class字节码文件中存在，但运行时无法获得
     - `@Retention(RetentionPolicy.RUNTIME)`： **注解会在class字节码文件中存在，在运行时可以通过反射获取到（常用）**

- `@Target`

	- 表示我们的注解作用的范围，可以是类，方法，方法参数变量等，同样也是通过枚举类`ElementType`表达作用类型

		- `@Target(ElementType.TYPE)`**： 作用接口、类、枚举、注解(常用)**

		- `@Target(ElementType.FIELD)` **：作用属性字段、枚举的常量(常用)**

		- `@Target(ElementType.METHOD)` **：作用方法(常用)**

		- `@Target(ElementType.PARAMETER) `**：作用方法参数(常用)**

		- `@Target(ElementType.CONSTRUCTOR)` ：作用构造函数

		- `@Target(ElementType.LOCAL_VARIABLE)`：作用局部变量

		- `@Target(ElementType.ANNOTATION_TYPE)`：作用于注解（@Retention注解中就使用该属性）

		- `@Target(ElementType.PACKAGE)` ：作用于包

		- `@Target(ElementType.TYPE_PARAMETER) `：作用于类型泛型，即泛型方法、泛型类、泛型接口 （jdk1.8加入）

		- `@Target(ElementType.TYPE_USE)` ：类型使用.可以用于标注任意类型除了 class （jdk1.8加入）

- `@Document `

	- **将注解中的元素包含到 Javadoc 中去（常用）**

- `@Inherited`

	- **注解修饰了一个父类，如果他的子类没有被其他注解修饰，则它的子类也继承了父类的注解**

# 4、自定义注解

~~~java
package com.feige.annotation;


import java.lang.annotation.*;

//写入javadoc
@Documented
//jvm运行阶段还有效，可以通过反射获取
//runtime>class>sources
@Retention(RetentionPolicy.RUNTIME)
//可以作用在类和方法上
@Target({ElementType.METHOD,ElementType.TYPE})
//子类可以继承父类的注解
@Inherited
public @interface  MyAnnotation {
    //注解的参数：参数类型+参数名()
    String name() default "";//注解参数如果没有默认值，就必须给参数赋值
    int age() default 0;
}

~~~

如何使用

~~~java
package com.feige.annotation;


@MyAnnotation
public class AnnotationTest {

    @MyAnnotation(name = "feige",age = 20)
    public void test(){

    }
}

~~~

