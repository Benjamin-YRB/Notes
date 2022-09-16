使用Junit时出现：java.lang.Exception: No tests found matching Method **Test2**(pojo.personTest) from org.junit.internal.requests.ClassRequest@69222c14 at org.junit.internal.requests.FilterRequest.getRunner(FilterRequest.java:40)
at com.intellij.junit4.JUnit4IdeaTestRunner.startRunnerWithArgs(JUnit4IdeaTestRunner.java:50)
at com.intellij.rt.junit.IdeaTestRunner$Repeater.startRunnerWithArgs(IdeaTestRunner.java:33)
at com.intellij.rt.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:220)
at com.intellij.rt.junit.JUnitStarter.main(JUnitStarter.java:53)

```java
@Test
    public void Test2(){
			//测试代码
    }
```

原因：方法名可能使用了大写的Test方法名，修改结果如下：

```java
@Test
    public void test2(){
			//测试代码
    }
```

### mysql连接驱动8.0.16出现bean名称冲突

原因不明，更换更高版本驱动后异常消失



#### Idea编译时出现GC overhead limit exceeded

![企业微信截图_f282c4cc-54d7-4add-80f4-4c54838c361f](/images/企业微信截图_f282c4cc-54d7-4add-80f4-4c54838c361f.png)

在maven设置中调整compiler的内存大小即可





### %23 -> #

前后端传值时请求行参数中含有%23 会变成# 导致模糊查询失败
这是由于为了在http中传输特殊字符和汉字使用的一种编码格式
解决方案：请求参数不要放在请求行中而是放在请求体即可







锐特框架分页查询 出现numberFormatException 

没传分页信息导致获取数据为null，导致数字转型异常



前后端联调传值问题：

​	后端获取不到前端的值，当contentType设置为"application/json"时，请求数据是放在请求体中,应该用requestBody接收

若contentType为默认的form-xxx啥的，请求数据在请求行中，应该用requestParam来接收

controller接收参数注解要和调用的service一致。

![image-20211020173043175](/images/image-20211020173043175.png)

![image-20211020173120391](/images/image-20211020173120391.png)







#### Cannot resolve com.sinoservices.chainwork:billing-module-api:ASD-1.0.2-SNAPSHOT

maven无法解析依赖，有可能是下载依赖时网络问题或其他原因导致本地仓库的包不完整，删除仓库中对应包之后重新拉取即可。





HashMap.values()无法直接强转成List，将values传入List构造器即可。



在系统出现数据不正常或某些操作失效时（如@DS切换数据源失效，查询不出数据时）记得查看自己的spring profile配置的是dev还是prod。





手写的service一直404，应该是有什么地方没有配置。

service的实现类加上Controller注解。





### mysql自增列是不是只能由主键自增，可否设置其他列为自增列？







groupingBy传入第二个收集器使用reducing归约收集器时，不是针对分组后的values单个list进行归约，而是类似于流的扁平化，将values中所有的list拼接起来之后进行整体的归约，归约结果也很奇怪，后续研究研究。

当时代码：

```java
Map<String, UserMachineQueryItem> result = data.stream().
  collect(Collectors.groupingBy(UserMachineQueryItem::getCreator,
          				Collectors.reducing(new UserMachineQueryItem(true), (u1, u2) -> {
					u1.setMonth1(u1.getMonth1() + u2.getMonth1());
					u1.setMonth2(u1.getMonth2() + u2.getMonth2());
					u1.setMonth3(u1.getMonth3() + u2.getMonth3());
					u1.setMonth4(u1.getMonth4() + u2.getMonth4());
					u1.setMonth5(u1.getMonth5() + u2.getMonth5());
					u1.setMonth6(u1.getMonth6() + u2.getMonth6());
					u1.setMonth7(u1.getMonth7() + u2.getMonth7());
					u1.setMonth8(u1.getMonth8() + u2.getMonth8());
					u1.setMonth9(u1.getMonth9() + u2.getMonth9());
					u1.setMonth10(u1.getMonth10() + u2.getMonth10());
					u1.setMonth11(u1.getMonth11() + u2.getMonth11());
					u1.setMonth12(u1.getMonth12() + u2.getMonth12());
					u1.setCreator(u2.getCreator());
					u1.setYearTime(u2.getYearTime());
					u1.setLevel(u2.getLevel());
					return u1;
})));
```



#### case when 

```sql
SELECT 
    t.creator,
    IFNULL(MAX(CASE t.create_time
        WHEN 1 THEN t.QTY_EA
    END),0) AS month1,
    IFNULL(MAX(CASE t.create_time
        WHEN 2 THEN t.QTY_EA
    END),0) AS month2,
    IFNULL(MAX(CASE t.create_time
        WHEN 3 THEN t.QTY_EA
    END),0) AS month3,
    IFNULL(MAX(CASE t.create_time
        WHEN 4 THEN t.QTY_EA
    END),0) AS month4,
    IFNULL(MAX(CASE t.create_time
        WHEN 5 THEN t.QTY_EA
    END),0) AS month5,
    IFNULL(MAX(CASE t.create_time
        WHEN 6 THEN t.QTY_EA
    END),0) AS month6,
    IFNULL(MAX(CASE t.create_time
        WHEN 7 THEN t.QTY_EA
    END),0) AS month7,
    IFNULL(MAX(CASE t.create_time
        WHEN 8 THEN t.QTY_EA
    END),0) AS month8,
    IFNULL(MAX(CASE t.create_time
        WHEN 9 THEN t.QTY_EA
    END),0) AS month9,
    IFNULL(MAX(CASE t.create_time
        WHEN 10 THEN t.QTY_EA
    END),0) AS month10,
    IFNULL(MAX(CASE t.create_time
        WHEN 11 THEN t.QTY_EA
    END),0) AS month11,
    IFNULL(MAX(CASE t.create_time
        WHEN 12 THEN t.QTY_EA
    END),0) AS month12
FROM
    (SELECT 
        ep.ebpj_wh_name AS creator,
            MONTH(wah.create_time) AS create_time,
            YEAR(wah.create_time) AS year_time,
            SUM(wad.QTY_ASN_EA) AS QTY_EA
    FROM
        wm_asn_header wah
    LEFT JOIN wm_asn_detail wad ON wah.asn_no = wad.ASN_NO
    LEFT JOIN es_user eu ON wah.CREATOR = eu.ESUS_ID
    LEFT JOIN eb_project ep ON wah.project_id = ep.EBPJ_ID
    WHERE
        asn_type IN ('YHJRK')
    GROUP BY creator,month(wah.create_time)) t
    group by t.creator
```

#### 打印GC日志

-XX:+PrintGCDetails





#### 导入3000条以上数据，某个区间没有导入成功。

那个区间插入有问题，异常回滚了。





wifi链接显示受限制或者无连接，查看tcp/ip设置是否动态获取ip地址，查询dns服务器配置是否正确。





#### idea启动tomcat无效：Unable to ping server at localhost:1099

java虚拟机启动参数配置错误，去掉或者重新检查参数。









byte数组转String：

```java
byte[] bytes = "中文".getBytes();
String result = new String(bytes,StandardCharsets.UTF_8);
```





#### 多线程下使用SimpleDateFormat转换异常

时间点有可能会是1970年

SimpleDateFormat中有Calendar类，calendar类内部有状态因此有多线程问题。



#### httpclient发送delete请求无法携带请求体

重写一个delete方法继承post方法的父类即可。

![image-20220426135434198](images\image-20220426135434198.png)



#### idea启动tomcat在多少毫秒部署完成后卡住 不运行项目

解决方案：尝试调整一下tomcat的虚拟机内存参数。







#### 老Web项目变更依赖

在新加的jar包右键 add as Library





#### 记一次sql优化经历

多条件or查询有可能使索引失效，可以使用union拆开or条件查询 走单独的索引即可。



#### 初始方案

##### 安卓listView每滚动一次都会执行一次adapter中onBindITemView方法，然后我在item中的一个输入框添加了addTextChangedListener文本监听器来同步我修改item的值，每次滚动都会刷新item，从底部拉到顶，末尾的item的输入框的值会覆盖第一个item输入框的值--莫名其妙的BUG



#### 方案二

##### setOnFocusChangeListener添加了焦点改变事件无用，只有在获取焦点时生效，当焦点在文本框时直接点击签收按钮，不会触发失去焦点事件，无法同步修改datalist中的值。

#### --解决方案

方案二改进： 签收按钮事件中移除文本框焦点  用以触发文本框失焦事件







请求进来后卡住不往下走了  后续请求也无法响应，这时确认一下本机CPU资源













#### spring ioc容器根据bean名称找不到bean



解决过程：检查xml配置的包扫描路径，没有问题，

![image-20220610175225121](images\image-20220610175225121.png)

更换bean注册方式，提示已存在同类型bean，说明原配置没有问题注册成功。

原因：获取bean时名称错误，注册的bean 名称开头是两个大写字母如：PHService，获取时也是PHService而非PhService。









#### 美的项目，拖车任务下发之后，隔一段时间任务柜上商品就会被逻辑删除，没有相关操作记录。

原因：其他人写的更新sql，没有条件也不判空 ，全表更新为逻辑删除