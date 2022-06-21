## 获取配置文件中的组件

@ConfigurationProperties:批量注入配置文件中的属性，默认从全局配置文件中获取值

前提是这个组件必须是容器内的组件

``` java
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String name;
}
```

@Value：一个一个的注入，可接受字面量、#{key}从环境变量、配置文件中取值、#{SpEL}表达式

``` java
@Component
//@ConfigurationProperties(prefix = "person")
public class Person {
    @Value("张三")
    private String name;
}
```

### @ConfigurationProperties和@Value的比较

|                | @ConfigurationProperties | @Value     |
| -------------- | ------------------------ | ---------- |
| 功能           | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定       | 支持                     | 不支持     |
| SpEL           | 不支持                   | 支持       |
| JSR303数据校验 | 支持                     | 不支持     |
| 复杂类型封装   | 支持                     | 不支持     |

> 需要获取配置文件中某项值时，使用@Value注解
>
> 专门写了一个javaBean来和配置文件映射时使用@ConfigurationProperties注解

### @PropertySource和@ImportResource

@PropertySource:加载指定的配置文件

``` java
@PropertySource(value = {"classpath:person.properties"})
```

@ImportResource：导入Spring的配置文件，让其生效，该注解使用在配置类上

``` java
@Configuration
@ImportResource(locations = {"classpath:beans.xml"})
```

> 但是不推荐写xml之类的配置文件的方式来添加组件，推荐写配置类

``` java
@Configuration
public class MyAppConfig {
    
    @Bean//相当于<bean></bean>
    public HelloService helloService(){
        return  new HelloService();
    }
}
```

#### Controller中获取内置对象

