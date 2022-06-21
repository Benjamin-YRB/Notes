- ### Mybatis的工作流程


1、加载配置初始化：加载配置文件，将SQL的配置信息家在城一个个MappedStatement对象（包括了传入参数映射配置、执行的SQL语句，结果映射配置），存储在内存中。

2、接收调用请求：调用Mybatis提供的API，传入参数：SQL的ID和传入参数对象，将请求传递给下层的请求处理层处理。

3、处理操作请求：API接口层请求传来，传入参数：为SQL的ID和传入对象，处理过程：

​	3.1、根据SQL的ID查找对应的MappedStatement对象。

​	3.2、根据传入参数对象解析MappedStatement对象，得到最终要执行的SQL语句和执行传入参数。

​	3.3、获取数据库连接，根据得到的最终SQL语句和执行参数到数据库执行，并得到执行结果。

​	3.4、根据MappedStatement对象中的结果映射配置对得到的结果进行转换并得到最终的处理结果。

​	3.5、释放连接资源。

​	3.6、返回处理结果将最终的处理结果返回

- ### Mybatis基本要素

  - mybatis-config.xml 全局配置文件
  - Mapper.xml 核心映射文件
  - SQLSessionFactory、Sqlsession接口

- 个人的收获：
  
  - 在配置文件中<configration>标签中可以通过<properties resource="xxx.properties">来加载外部属性文件，属性无需写死在xml文件中，直接修改静态的xxx.properties文件即可修改配置。



- #### 全局配置文件

  - 包含了Mybatis的基础设置和属性信息，其文档结构如下：
    - configuration配置
      - properties属性
      - settings设置
      - typeAliases类型别名
      - typeHandlers类型处理器
      - objectFactory对象工厂
      - plugins插件
      - Environments环境
      - mappers映射器







<bind>标签



Mybatis 踩坑日记

```xml
<collection ofType才是关联集合类型 不是javatype 报错 argument type mismatch></collection>

```

