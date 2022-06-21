# Spring源码

## 第1章 Spring整体架构和环境搭建

### 1.1Spring的整体架构

![111732_x0O8_1249631](/Users/yangxiansheng/笔记/images/111732_x0O8_1249631.png)

这些模块被总结为以下几个部分：

1、**Core Container**

​	Core Container包含有Core、Beans、Context和Expression Language模块。

​	Core和Beans模块是框架的基础部分，提供Ioc和依赖注入特性。这里的基础概念是BeanFactory，它提供对Factory模式的经典实现来消除对程序性单例模式的需要。并真正允许你从程序逻辑中分理处依赖关系和配置。

- Core模块主要包含Spring框架基本的核心工具类，是Spring其他组件的基本核心，也可以自己使用。
- Beans模块是所有应用都会使用到的，它包含访问配置文件、创建和管理Bean以及进行IOC（Inversion of control）或者DI（Dependency Injection）操作相关的所有类。
- Context模块构建于Core和Beans模块基础之上，提供了一种类似于JNDI注册器的框架式的对象访问方法，Context模块继承了Beans模块的特性，为Spring核心提供了大量扩展，添加了对国际化、事件传播、资源加载和对Context的透明创建的支持。ApplicationContext接口是Context模块的关键。
- Expression Language模块提供了一个强大的表达式语言用于在运行时查询和操纵对象。它是JSP2.1规范中定义的unifed expression language的一个扩展，该语言支持设置、获取属性的值，属性的分配，方法的调用，访问数组上下文（accession the context of arrays）、容器和索引器、逻辑和算术运算符、命名变量以及从Spring的IOC容器中根据名称检索对象，它也支持list投影、选择和一般的list聚合。

2、**Data Access/Integration**

​	Data Access/Integration层包含有JDBC、ORM、OXM、JMS和Transaction模块，其中：

- JDBC模块提供了一个JDBC抽象层，它可以消除冗长的JDBC编码和解析数据库厂商特有的错误代码，这个模块包含了Spring对JDBC数据访问进行封装的所有类。
- ORM模块为流行的对象-关系映射API，如JPA、JDO、Hibernate、Ibatis等，提供了一个交互层，利用ORM封装包，可以混合使用所有Spring提供的特性进行O/R映射。

Spring框架插入了如干个ORM框架，从而提供了ORM的对象管理工具，其中包括JDO、Hibernate、iBatisSQL Map。所有这些都遵从Spring的通用事务和DAO异常层次结构。

- OXM模块提供了一个对Object/XML映射实现的抽象层，Object/XML映射实现包括JAXB、Castor、XMLBeans、JiBX和XStream。
- JMS（Java Message Services）模块主要包含一些制造和消费信息的特性。
- Transaction模块支持编程和声明性的事务管理，这些事务类必须实现特定的接口，并且对所有的POJO都适用。

**3、Web**

​	Web上下文模块建立在应用程序上下文模块至上，为基于Web的应用程序提供了上下文。所以，Spring框架支持与Struts的集成。Web模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。Web层包含了Web，Web-Servlet、Web-Struts和Web-Porlet模块，具体说明如下。

- Web模块：提供了基础的面向web的集成特性。例如：多文件上传、使用servlet listeners初始化Ioc容器以及一个面向Web的应用上下文。它还包括Spring远程支持中Web的相关部分。
- Web-Servlet模块web.servlet.jar:该模块包含Spring的model-view-controller（MVC）实现。Spring的MVC框架使得模型范围内的代码和Web forms之间能够清楚的分开来，并与Spring框架的其他特性集成在一起。

**4、AOP**

​	AOP模块提供了一个符合AOP联盟标准的面向切面编程的实现，它让你可以定义例如方法拦截器和切点，从而将逻辑代码分开，降低它们之间的耦合性。利用source-level的元数据功能，还可以将各种行为信息合并到你的代码中。

​	通过配置管理特性，SpringAOP模块直接将面向切面的编程功能集成到Spring框架中，所以可以很容易的使Spring框架管理的任何对象支持aop。Spring AOP模块为基于Spring的应用程序中的对象提供了事务管理服务。通过使用Spring AOP可以将声明式事务集成到应用程序中。

- Aspects模块提供了对AspectJ的集成支持。
- Instrumentation模块提供了class instrumentation支持和classloader实现，使得可以在特定的应用服务器上使用。

**5、Test**

​	Test模块支持使用JUnit和TestNG对Spring组件进行测试。

## 第2章 容器的基本实现

### 2.1容器的基本用法

​	bean是Spring中最核心的东西，先看看bean的定义。

```java
public class MyTestBean{
  private String set = "test";
  public String getStr(){
    return str;
  }
  public void setStr(String str){
    this.str = str;
  }
}
```

​	很普通，Spring的目的就是让bean成为一个

纯粹的POJO。接下来看配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmls="http://www.Springframework.org/schema/beans"
       xmls:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.Springframework.org/schema/beans http://www.Springframework.org/schema/beans/Spring-beans.xsd">
	<bean id="myTestBean" class="bean.MyTestBean"></bean>
</beans>
```

接下来代码测试：

```java
public class BeanFactoryTest{
  @Test
  public void testSimpleLoad(){
    BeanFactory bf = new XMLBeanFactory(new ClassPathResource("beanFactory.xml"));
    MyTestBean bean = (MyTestBeanv)bf.getBean("myTestBean");
    assertEquals("str",bean.getStr());
  }
}
```

​	但是在开发中很少直接使用BeanFactory，一般都使用ApplicationContext。

### 2.2功能分析

​	上述代码中，一共执行了以下几步：

1. 读取配置文件beanFactory.xml。

2. 根据beanFactory.xml中的配合找到对应的类的配置，并实例化。

3. 调用实例化之后的实例。

   要想完成上述功能，至少需要三个类：

1. ConfigReader：用于读取及验证配置文件，然后放在内存中。
2. ReflectionUtil：用于根据配置文件中的配置进行反射实例化，上述代码中，我们根据bean.MyTestBean进行实例化。
3. App：用于完成整个逻辑的串联。