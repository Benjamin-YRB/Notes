# 什么是aop

面向切面编程

作用：对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间耦合度降低，提高程序的可重用性，提高了开发的效率



举例：在不修改原本代码的基础上添加新的功能



## AOP术语

1、连接点

​	类里面的那些方法可以被增强，这些方法称为连接点

2、切入点

​	实际被真正增强的方法，称为切入点

3、通知（增强）

​	3.1、实际增强的逻辑部分称为通知（增强）

​	3.2、通知有多种类型：

- 前置通知
- 后置通知
- 环绕通知
- 异常通知
- 最终通知（类似finally）

4、切面

​	4.1、把通知应用到切入点过程



### AOP操作

1、spring框架一般基于AspectJ实现AOP操作

​	**AspectJ是什么**

​		不是spring的组成部分，独立的AOP框架，一般把它和spring一起使用进行AOP操作

2、基于AspectJ实现AOP操作

​	2.1、基于xml配置文件实现

​	2.2、基于注解方式实现（建议使用）



3、引入AOPi相关依赖

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-aspects -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>5.2.9.RELEASE</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.6</version>
    <scope>runtime</scope>
</dependency>

<!-- https://mvnrepository.com/artifact/cglib/cglib -->
<dependency>
    <groupId>cglib</groupId>
    <artifactId>cglib</artifactId>
    <version>3.3.0</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.aopalliance/com.springsource.org.aopalliance -->
<dependency>
    <groupId>org.aopalliance</groupId>
    <artifactId>com.springsource.org.aopalliance</artifactId>
    <version>1.0.0</version>
</dependency>


```

4、切入点表达式

​	4.1、切入点表达式作用：知道对那个类里面的那个方法进行增强

​	4.2、语法结构：![image-20200920103208994](/Users/yangxiansheng/笔记/images/image-20200920103208994.png)

 	4.3、表达式示例：

- 对com.spring.dao.BookDao类里面的add方法进行增强

  ​		execution(* com.spring.dao.BookDao.add(..))

- 对com.spring.dao.BookDao类里面的所有方法进行增强

  ​		execution(* com.spring.dao.BookDao.*(..))

- 对com.spring.dao包下的所有类的所有方法进行增强

  ​		execution(* com.spring.dao.***.***(..))

  

# AOP注解实现方法增强

### 1、创建类，类中定义方法

### 2、创建增强类，（编写增强逻辑）

2.1、在增强类中，创建方法，让不同方法代表不同通知

### 3、进行通知的配置

​	3.1、在spring配置文件中开启注解扫描

```xml
  <context:component-scan base-package="com.spring.aopannotaion"></context:component-scan>
```

​	3.2、使用注解创建User和UserProxy对象

​	3.3、在增强类上添加注解@Aspect

```java
@Aspect//生成代理对象
public class UserProxy 
```

​	3.4、在spring配置文件中开启生成代理对象 

```xml
 <aop:aspectj-autoproxy/>
```

### 4、配置不同类型的通知

1. 在增强类中，在作为通知方法上面添加通知类型注解，并且在注解中使用切入点表达式进行配置

```java
@Component
@Aspect//生成代理对象
public class UserProxy {
    //前置通知
    @Before(value = "execution(* com.spring.aopannotaion.User.add(..))")
    public void before(){
        System.out.println("before......");
    }
    //最终通知
    @After("execution(* com.spring.aopannotaion.User.add(..))")
    public void after(){
        System.out.println("after.......");
    }

    //异常通知
    @AfterThrowing("execution(* com.spring.aopannotaion.User.add(..))")
    public void afterThrowing(){
        System.out.println("afterThrowing.......");
    }
    //后置通知（返回通知）
    @AfterReturning("execution(* com.spring.aopannotaion.User.add(..))")
    public void afterReturning(){
        System.out.println("afterReturning.......");
    }

    //环绕通知
    @Around("execution(* com.spring.aopannotaion.User.add(..))")
    public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("环绕之前.......");
        //被增强的方法执行
        proceedingJoinPoint.proceed();
        System.out.println("环绕之后.......");
    }
}
```

### 5、相同切入点抽取

```java
    @Pointcut(value = "execution(* com.spring.aopannotaion.User.add(..))")
    public void pointCutDemo(){
      
    }
    //前置通知
    @Before(value = "pointCutDemo()")//直接调用抽取出来的切入点方法名
    public void before(){
        System.out.println("before......");
    }
```

### 6、有多个增强类对同一个方法进行增强，设置增强类优先级

1. 在增强类上添加注解@Order（数字类型值），数字越小优先级越高

```java
    //最终通知
    @Order(3)
    @After("execution(* com.spring.aopannotaion.User.add(..))")
    public void after(){
        System.out.println("person after.......");
    }


    //最终通知
    @Order(4)
    @After("execution(* com.spring.aopannotaion.User.add(..))")
    public void after(){
        System.out.println("after.......");
    }
```

![image-20200922093559110](/Users/yangxiansheng/笔记/images/image-20200922093559110.png)

### 7、完全注解开发

​	配置类替代xml配置文件

```java
@Configuration
@ComponentScan(basePackages = "com.spring.aopannotaion")
@EnableAspectJAutoProxy(proxyTargetClass = true)
public class aopConfig {
}
```

