# spring事务

特性：（ACID）

1. 原子性
2. 一致性
3. 隔离性
4. 持久性



## 事务操作

1. 开启事务
2. 业务操作
3. 完成操作，提交事务
4. 若出现异常则事务回滚

### spring事务管理介绍

1. 一般将事务添加到service层（业务逻辑层）
2. 在spring中进行事务管理操作
   1. 两种方式：编程式事务管理和声明式事务管理（使用）
3. 声明式事务管理
   1. 基于注解方式（简单方便）
   2. 基于xml配置文件方式
4. 在Spring进行声明式事务管理，底层使用AOP原理
5. Spring事务管理API
   1.  提供一个接口，代表事务管理器，这个接口针对不同的框架提供不同的实现类
   2. ![image-20200922105225246](/Users/yangxiansheng/笔记/images/image-20200922105225246.png)

### 声明式事务管理与使用

1. 在spring配置文件中配置数据源和事务管理器

```xml
<!--    配置数据源-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://3306/mybatis"/>
        <property name="username" value="root"/>
        <property name="password" value="aA982244"/>
    </bean>

<!--    创建事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
      
        <property name="dataSource" ref="dataSource"/>
    </bean>
```

2.在spring配置文件，开启事务注解

​	1.引入名称空间tx 

​	2.开启事务注解

```xml
<tx:annotation-driven transaction-manager="transactionManager"/>
```



3.在service类上（方法上也可以）添加事务注解

```java
@Transactional
public class UserService{}
```

### 声明式事务管理参数配置

1. 在注解@Transactional注解里配置事务的相关

![image-20200922124823694](/Users/yangxiansheng/笔记/images/image-20200922124823694.png)

2.propagation:事务传播行为

1. 多事务方法直接进行（互相）调用，这个过程中事务是如何进行管理的；

如：有事务方法调用没有事务的方法、无事务方法调用有事务方法和有事务方法调用有事务方法

1. 一共有七种事务传播行为，最为常用的就是前两种



![image-20200922175724242](/Users/yangxiansheng/笔记/images/image-20200922175724242.png)

- **（默认）REQUIRED**：如果add方法本身有事务，调用update方法之后，update使用当前add方法里面的事务，如果add本身没有事务，调用update方法之后，创建新事务
- **REQUIRED_NEW**：使用add方法调用update方法，无论add是否有事务，都创建新的事务 
- SUPPORTS：如果有事务在运行，当前的方法就在这个事务内运行，否则它可以不运行在事务中
- NOT_SUPPORTED：当前的方法不应该运行在事务中，如果有运行的事务，将它挂起
- MANDATORY：当前的方式必须运行在事务内部，如果没有正在运行的事务，抛出异常
- NEVER：当前的方法不应该运行在事务中，如果有运行的事务，就抛出异常
- NESTED：如果有事务在运行，当前的方法就应该在这个事务的嵌套事务内运行，否则，就启动一个新的事务，并在它自己的事务内运行

```java
@Service
@Transactional(propagation = Propagation.REQUIRED)//默认传播行为
```



3.isolation：事务隔离级别

1. 事务的隔离性，多事务操作之间不会产生影响，不考虑隔离性会产生很多问题

2. 三个问题：脏读、不可重复读、虚（幻）读

   1. 脏读：一个未提交的事务读取到了另一个未提交事务的数据

   ![image-20200922181722284](/Users/yangxiansheng/笔记/images/image-20200922181722284.png)

   2. 不可重复读：一个未提交事务读取到另一提交事务修改数据
   3. 虚（幻）读：一个未提交事务读取到另一提交事务添加数据

3. 通过设置事务的隔离级别解决以上问题

|                  | 脏读 | 不可重复读 | 幻读 |
| ---------------- | ---- | ---------- | ---- |
| READ UNCOMMITTED | 有   | 有         | 有   |
| READ COMMITTED   | 无   | 有         | 有   |
| REPEATABLE READ  | 无   | 无         | 有   |
| SERIALIZABLE     | 无   | 无         | 无   |

> Mysql默认的事务隔离级别是可重复读

4.timeout：超时时间

1. 事务需要在一定时间内提交，如果不提交，则回滚
2. 默认值是-1，即没有超时时间

5:readOnly：是否只读

1. 读：查询操作，写：增删改操作
2. readOnly默认值：false，可以进行CURD
3. 设置为true后只能进行查询操作

6.rollbackFor：回滚

1.  设置出现哪些异常进行事务回滚


7：noRollbackFor：不回滚

1.  设置出现哪些异常不进行事务回滚



### 完全注解开发声明式事务管理

1. 创建配置类替代xml配置文件