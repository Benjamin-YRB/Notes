# Mybatis缓存

简介：暂时存储的一些数据；加快系统的查询速度等

CPU：

​	主频：4-2.7GHZ

​	一级缓存（4MB）；二级缓存（16MB）

## Mybatis的缓存机制

Map：能保存查询出的一些数据；

- 一级缓存：线程级别的缓存，本地缓存，SqlSession级别的缓存，在当前会话可以使用
- 二级缓存：全局范围缓存，除去当前线程、SqlSession之外，其他也能使用改缓存。



### 一级缓存

Mybatis中SqlSession级别的缓存，默认存在；

机制：在一次SqlSession会话中，查询过的数据Mybatis会保存在一个缓存中（Map），再次获取一样的数据时不会再次查询而是直接返回结果，并且引用的对象一样



> 一级缓存失效的几种情况：
>
> - 使用不同的sqlSession会话查询同一个数据
>   - 一级缓存可以类比成SqlSession内部的缓存，不同的SqlSession一级缓存不同，因此不通用
> - 同一个方法，传入不同的参数，若缓存中没有这个新参数的结果，则会再查一次
> - 任何的增删改操作都会清空当前缓存，因此缓存失效
> - 手动清空当前SqlSession的缓存

每次查询，先看一级缓存中有没有，若没有再去查询数据，每个SqlSeesion拥有自己的一级缓存。



### 二级缓存

全局作用域缓存，默认不开启，需手动配置，namespace级别的缓存

**PS：**二级缓存在SqlSession关闭或提交之后才会生效

> - Mybatis提供二级缓存的接口以及实现，缓存实现要求POJO实现Serializable接口

使用步骤：

1. 全局配置文件中开启二级缓存

```xml
<setting name="cacheEnabled" value="true"/>
```

2. 在要开启二级缓存的mapper中配置缓存即可使用缓存

```xml
<cache></cache>
```

> 1. 不会出现一级缓存和二级缓存中有同一个数据。
>    1. 二级缓存中何时有数据？；一级缓存关闭就有了
>    2. 一级缓存中何时有数据？，二级缓存中没有此数据，就查一级缓存，若还是没有再去查数据库，数据库查询结果放在一级缓存。
> 2. 任何时候都是先看二级缓存，在看一级缓存，若都没有再去查数据库

缓存原理

![image-20201007081037601](/Users/yangxiansheng/笔记/images/image-20201007081037601.png)

缓存相关的设置

- select标签可以在标签体内设置是否使用二级缓存

```xml
<select id="findOne" resultMap="personResultMap" useCache="false">
        select * from person where id = #{id}
</select>
```

> useCache="false"这个属性只对二级缓存有影响，默认是true

- sql标签的flushCache属性
  - 增删改默认flushCache = true，sql执行后。同时清空一级和二级缓存
  - 查询默认flushCache = false
- sqlSession.clearCache();
  - 只用来清除一级缓存
- 当某个作用域（一级缓存sqlSession、二级缓存nameSpace）进行了增删改操作后，默认将该作用域下所有select中的缓存清空

### Mybatis整合第三方缓存

例如：ehCache：专业的java进程内缓存框架

1. 导包

```xml
<!-- https://mvnrepository.com/artifact/net.sf.ehcache/ehcache-core -->
<dependency>
    <groupId>net.sf.ehcache</groupId>
    <artifactId>ehcache-core</artifactId>
    <version>2.6.11</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.mybatis.caches/mybatis-ehcache -->
<dependency>
    <groupId>org.mybatis.caches</groupId>
    <artifactId>mybatis-ehcache</artifactId>
    <version>1.2.1</version>
</dependency>

<!--还依赖sl4j-->
<!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-api -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.8.0-beta4</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-log4j12 -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>1.8.0-beta4</version>
    <scope>test</scope>
</dependency>

```

