# MySQL必知必会

## MySQL简介

### 什么是MySQL

​	MySQL是一种数据库软件——DBMS（数据库管理系统）。

#### 客户机-服务器软件

​	DBMS可分为两类：一类基于共享文件系统的DBMS，一类基于客户机-服务器的DBMS。前者用于桌面用途（包括Microsoft Access 和FileMaker），通常不用于高端或关键应用。

​	MySQL、Oracle以及Microsoft SQL Server等数据库是基于客户机-服务器的数据库。服务器部分是负责所有数据访问和处理的一个软件。这个软件运行在成为数据库服务器的计算机上。

​	与数据文件打交道的只有服务器软件。关于数据添加、删除、更新和查找的请求都有服务器软件弯沉。这些请求都来自运行客户机软件的计算机。客户机是与用户打交道的软件。

> 客户机和服务器软件可以分开安装，通过网络完成通信。为了使用MySQL，你需要访问运行MySQL服务器软件的计算机和发布命令到MySQL的客户机软件的计算机。
>
> - 服务器软件为MySQL DBMS。
> - 客户机可以是MySQL提供的工具、脚本语言、Web应用开发语言、程序设计语言等。

#### 使用MySQL

##### 了解数据库和表

​	数据库、表、列、用户、权限等信息被存储在数据库和表（内部表），一般不直接访问，可用SHOW命令来显示这些信息。

```sql
--返回可用数据库的一个列表。包含在这个列表中的可能是MySQL内部使用的数据库
SHOW DATABASES;

--获取数据库内的表
SHOW TABLES;

--获取一个表的列
SHOW COLUMNS from tableName;

SHOW Status;

SHOW CREATE DATABASE

SHOW CREATE TABLE

SHOW GRANTS

SHOW ERRORS

SHOW WARNINGS
```

### 检索数据

#### 去重

​	Distinct关键字去重

#### 限制结果

​	limit关键字限制返回的结果行数。

```sql
--限制返回不多于5行的结果
select * from tablename limit 5;
--限制指定开始和结束行的结果，下面例子返回第二个5行结果，即分页，limit关键字有两个数字的话，返回从第一个数字开始到第二个数据结束的行
select * from tablename limit 5，5;
```

#### 使用完全限定表名

​	因为在不同数据库（database）中，表可以重名，在开发中，若连接没有默认的数据库，有时需指定全限定表名，格式为：database.tableName

### 排序数据

##### ORDER BY子句

​	ORDER BY子句取一个或多个列，据此对输出进行排序。

```sql
select * from tbaleName order by col1,col2;
```

​	多个列排序按照子句中从左到右进行排序。若第一个列中没有相同的值，则不会按照第二列进行排序。

### 创建计算字段

​	当存储在表中的数据都不是应用程序需要的时候，要直接从数据库中检索出转换、计算或者格式化过的数据；而不是检索出数据交给应用程序处理。

​	计算字段是运行时在select语句内创建的。

##### 拼接字段

​	在select语句中可以使用Concat()函数来拼接两个或多个列：

```sql
select Concat(col1,col2) as '拼接字段' from tableName;
```

##### 清除列中多余空格



```sql
--清除右侧多余空格可以使用RTrim()函数来完成：
select RTrim(col) from tableName;

--清除左侧空格可以用LTrim()函数来完成
--清除两侧空格可用Trim()函数来完成
```

#### 使用函数处理数据

##### 函数

​	函数的可移植性没有sql语句强，不同的DBMS的函数可能不同

##### 文本处理函数

​	常用文本处理函数如下：

|    函数     |       说明        |
| :---------: | :---------------: |
|   left()    | 返回串左边的字符  |
|  length()   |      串长度       |
|  locate()   | 找出串的一个子串  |
|   Lower()   |       小写        |
|   LTrim()   |    左边去空格     |
|   Right()   |   串右边的字符    |
|   RTrim()   |    右边去空格     |
|  Soundex()  | 返回串的Soundex值 |
| SubString() |  返回子串的字符   |
|   Upper()   |       大写        |

​	上述Soundex函数是一个将任何文本转化为描述其语音表示的字母数字模式的算法。Soundex考虑了类似的发音字符和音节，使得能对串进行发音比较而不是字母比较。虽然Soundex不是SQL概念，但是MySQL提供对Soundex的支持。

##### 日期和时间梳理函数

​	日期和时间采用对象的数据类型和特殊的格式存储，以便能快速有效的排序或过滤并且节省物理空间。

​	常用日期和时间处理函数：

|     函数      |             说明             |
| :-----------: | :--------------------------: |
|   AddDate()   |    增加一个日期(天、周等)    |
|    AddTime    |   增加一个时间（时、分等）   |
|   CurDate()   |         返回当前日期         |
|   CurTime()   |         返回当前时间         |
|    Date()     |    返回日期时间的日期部分    |
|  DateDiff()   |       计算两个日期之差       |
|  Date_Add()   |    高度灵活的日期运算函数    |
| Date_Format() | 返回一个格式化的日期或时间串 |
|     Day()     |    返回一个日期的天数部分    |
|  DayofWeek()  |   返回一个日期对应的星期几   |
|    Hour()     |    返回一个时间的小时部分    |
|   Minute()    |    返回一个时间的分钟部分    |
|    Month()    |      返回一个日期的月份      |
|     Now()     |      返回当前日期和时间      |
|   Second()    |     返回一个时间的秒部分     |
|    Time()     |    返回一个日期的时间部分    |
|    Year()     |      返回一个日期的年份      |

​	MySQL使用的日期格式是yyyy-mm-dd，因此无论是插入还是更新还是条件过滤都要使用该格式来避免多义性。

```sql
--基本的日期比较很简单
select * from tableName where datetime1 = '2021-5-1';
```

​	但若表中列的数据类型是datetime时且时间部分不为'00:00:00'时，则该条件会过滤失败，解决方法是指示MySQL将给出的日期与列中的日期部分进行比较而不是比较列中整个日期值，因此，完成上述比较应当使用Date()函数。更可靠的select语句为：

```sql
--检索某一天的数据行
select * from tbaleName where Date(datetime1) = '2021-5-1';

--检索某年某月内的数据行
select * from tableName where Year(datetime1) = 2021 and month(datetime1) = 5;
```

##### 数值处理函数

​	仅处理数值数据，一般用于代数、三角函数或几何运算。以下为常用的数值处理函数：

|  函数  |    说明    |
| :----: | :--------: |
| Abs()  |   绝对值   |
| Cos()  |   余弦值   |
| Exp()  |   指数值   |
| Mod()  |    余数    |
|  Pi()  | 返回圆周率 |
| Rand() |   随机数   |
| Sin()  |    正弦    |
| Sqrt() |   平方根   |
| Tan()  |    正切    |

#### 汇总数据

##### 聚集函数

​	使用这些函数，MySQL查询可用于检索数据，以便分析和报表生成。这种类型的检索例子有以下几种：

- 确定表中行数
- 获得表中行组的和
- 找出表列（或所有行或某些特定的行）的最大值、最小值和平均值

> 聚集函数：运行在行组上，计算和返回单个值的函数。MySQL提供了5个聚集函数：

|  函数   |       说明       |
| :-----: | :--------------: |
|  AVG()  | 返回某列的平均值 |
| COUNT() |  返回某列的行数  |
|  MAX()  | 返回某列的最大值 |
|  MIN()  | 返回某列的最小值 |
|  SUM()  |  返回某列值之和  |

- AVG函数只能用于单个列，并且会忽略值为null的行
- COUNT函数用于计数，有两种使用方式：
  - COUNT(*)对表中行的数目进行计数，不管表列中包含的是空值（null）还是非空值
  - COUNT(column)对特定列中具有值的行进行计数，忽略null值。
- MAX()返回列中的最大值，要求指定列名，MAX函数一般用来找出最大的数值或者日期值，但MySQL允许将它用来返回任意列中的最大值，包括文本列的最大值。也忽略null值。
- MIN函数类似于MAX函数。

##### 聚集不同的值

​	以上五个函数可以指定聚集全部的值还是只聚集不同的值，若在函数中使用了DISTINCT关键字，如：

```sql
--只会计算不同值的平均值
select AVG(distinct col) from tableName;
```

> 若指定列名，则DISTINCT只能用于COUNT()，不能用于COUNT(*)，类似的，DISTINCT必须使用列名，不能用于计算或表达式。DISTINCT用于MAX和MIN没有意义。

#### 分组数据

​	分组数据可以汇总表内容的子集，这涉及两个select子句：GROUP BY和HAVING。

​	SQL聚集函数可以用来汇总数据，但这些计算都是在表的所有数据或条件匹配的特定数据行上进行的，分组允许把数据分成多个逻辑组，以便对每个组进行聚集计算。如：

```sql
select col,COUNT(*) as numOfCol from tableName GROUP by col;
```

​	上述SQL语句按照col列进行分组，然后计算每个组中有多少数据行。

​	GROUP BY子句有一些重要的规定：

- GROUP BY子句可以包含任意数目的列。这使得能对分组进行嵌套，为数据分组提供更细致的控制。

- 若在GROUP BY子句中嵌套了分组，数据将在最后规定的分组上进行汇总，即建立分组时，所有的列都一起计算。

- GROUP BY子句中列出的每个列都必须是检索列或有效的表达式（但不能是聚集函数）。若在select中使用表达式，则必须在GROUP BY子句中指定相同的表达式，而不能使用别名，如：

  - ```sql
    select col1+col2 , COUNT(*) from tableName GROUP BY col1+col2
    ```

- 除聚集计算语句外，SELECT语句中的每个列都必须在GROUP BY子句中给出。

- 若分组列中具有null值，则null也作为一个分组返回，若列中有多个null值，它们将分为一组。

- GROUP BY子句必须出现在WHERE子句之后，ORDER BY子句之前。

#### 过滤分组

​	如想列出至少有两个订单的所有顾客，这种情况下只能基于完整的分组而不是单个数据行进行过滤。

​	此时where发挥不了作用，要使用HAVING子句，where子句是基于表中存在的检索列进行数据行的过滤，HAVING子句除了具有where的功能之外，还可以对select语句中的聚集函数分组进行过滤，如：

```sql
select cust_id,count(*) as orders from orders group by cust_id having count(*) > 2;
```

> **HAVING和WHERE的差别**
>
> WHERE在数据分组之前过滤，HAVING在数据分组之后进行过滤，WHERE排除的行不在分组中，可以改变分组值从而影响HAVING子句中基于这些值过滤掉的分组。

#### 分组和排序

​	ORDER BY和GROUP BY的区别：

|                   ORDER BY                   |                        GROUP BY                        |
| :------------------------------------------: | :----------------------------------------------------: |
|                排序产生的输出                |            分组行，但输出可能不是分组的顺序            |
| 任意列都可以使用（甚至非选择的列也可以使用） | 只能使用选择列或表达式列，而且必须使用每个选择列表达式 |
|                  不一定需要                  |     若与聚集函数一起使用列（或表达式），则必须使用     |

> ​	有时GROUP BY分组的数据是按照分组顺序输出的，但并不总是这样，所以在使用GROUP BY时应该与ORDER BY搭配使用确保排序顺序

#### SELECT子句顺序

|   子句   |        说明        |       是否必须使用       |
| :------: | :----------------: | :----------------------: |
|  SELECT  | 要返回的列或表达式 |            是            |
|   FROM   |  从中检索数据的表  | 仅在从表中选择数据时使用 |
|  WHERE   |      行级过滤      |            否            |
| GROUP BY |      分组说明      |  仅在按组计算聚集时使用  |
|  HAVING  |      组级过滤      |            否            |
| ORDER BY |    输出排序顺序    |            否            |
|  LIMIT   |    要检索的行数    |            否            |

### 子查询

​	SubQuery，嵌套在其他查询中的查询，当要查询多个表并且表之间有数据关联性时，可以使用子查询。下面给出例子：

```sql
SELECT cust_name 
from customers 
where cusr_id IN (SELECT cust_id 
                  from orders 
                  where order_num in(SELECT order_num 
                                     from orderitems 
                                     where prod_name = 'apple'))
```

​	为了执行上述语句，MySQL需要执行3条SELECT语句，并且是由内向外开始执行。

> **列必须匹配**
>
> ​	在where子句中使用子查询，应该保证SELECT语句具有与where子句中相同数目的列。
>
> **子查询与性能**
>
> ​	上述代码有效，但并不是性能最佳的方法。

##### 使用子查询作为计算字段

​	子查询不单单只能在where子句后面，也可以作为select语句后的查询列。假如要显示customers表中每个客户的订单总数，订单与相应的客户id存储在orders表中。

​	为达到上述目的，要执行两个步骤：（1）从customer表中检索客户列表。（2）对于检索出的客户，统计其在orders表中的订单数目。下例代码对客户10001的订单进行计数：

```sql
select COUNT(*) from orders where cust_id = '10001';
```

​	为了对每个客户执行count(*)计算，应将其作为一个子查询，如：

```sql
select cust_name,cust_state,(select COUNT(*) from orders where orders.cust_id = customers.cust_id) as orders 
from customers order by cust_name;
```

### 高级联结

#### 自联结

​	查出商品表中某个商品的生厂商的其他商品

```sql
select p2.* from product p1 inner join product p2 on p1.vent_id = p2.vent_id and p1.product_name = '某个商品';
```

#### 外部联结

​	有时联结返回的结果中需要返回没有关联行的那些行，如：

- 对每个客户下了多少订单计数，以及那些从没有下过单的用户。
- 列出所有产品以及订购数量，包括没人订购的产品。
- 计算平均销售规模，包括至今没有下单的那些客户。

```sql
select * from customers left outer join orders on customers.cust_id = orders.cust_id
```

#### 使用带聚集函数的联结

​	聚集函数用来汇总数据，可以与联结一起使用，如：检索每个客户及每个客户所下的订单数：

```sql
-- 使用inner join 将客户表和订单表关联，group by子句按照客户分组，函数count计算每个客户的订单
select c.name,c.id,count(o.id) as orderNum from customer c inner join orders on c.id = o.cust_id group by c.id

-- 使用left join将没有订单的客户也查询出来
select c.name,c.id,count(o.id) as orderNum from customer c left join orders o on c.id = o.cust_id group by c.id
```

### 组合查询

​	将多个select语句的查询结果合并成一个结果集返回就是组合查询，使用union关键字进行组合。

​	有两种基本情况要用到组合查询：

- 在单个查询中从不同的表返回类似结构的数据
- 对单个表执行多个查询，按单个查询返回数据

> 组合查询和多个where条件：
>
> ​	多数情况下，组合相同表的两个查询完成的工作与具有多个where子句条件的单挑查询完成的工作相同，这两种方式在不同的查询中性能不同，应该都试一下以确定特定的查询那种方式更好。

#### union的使用

​	只需要给每条select语句质检放上关键字union即可。

##### 规则

- UNION必须由两条或以上的select语句组成，语句质检用关键字union分隔
- union中的每个查询必须包含相同的列、表达式或聚集函数（顺序不要求相同）
- 列数据类型必须兼容：类型不必完全相同，但必须是DBMS可以隐含地转换的类型（例如，不同的数值类型或不同的日期类型）。

##### 包含或取消重复的行

​	Union关键字自动去除重复的行，若想不去重可使用union ALL

> union all作为union的一种形式，他完成where子句完成不了的工作。如果确实需要每个条件匹配行全部出现（包括重复行），则必须使用union all而不是where。

##### 对组合查询结果排序

​	只能出现一条order by子句并且是在最后一条select语句后面，不允许使用多条order by语句。

### 全文本搜索

> ##### 	并非所有的引擎都支持全文本搜索
>
> MySQL最常用的两个数据库引擎为MyISAM和InnoDB，前者支持全文本搜索，而后者不支持

​	LIKE关键字使用通配操作符匹配文本，能够查找包含特殊值或部分值的行、使用正则表达式可以编写查找所需行的非常复杂的匹配模式。

​	上述这些机制很有用，但存在几个很重要的限制。

- 性能——通配符和正则表达式通常要求MySQL尝试匹配表中所有的行（而且这些搜索极少使用索引）。因此，由于被搜索的行数不断增加，这些搜索可能会非常耗时。

- 明确控制——使用通配符和正则表达式很难明确地控制匹配什么不匹配什么例如，指定一个词必须匹配，一个词必须不匹配，而一个词仅在第一个词确实匹配的情况下才可以匹配或者才可以不匹配。

- 智能化的结果——虽然基于通配符和正则表达式的搜索提供了非常灵活的搜索，但它们都不能提供一种智能化的选择结果的方法。例如，一个特殊词的搜索将会返回包含该词的所有行，而不区分包含单个匹配的行和包含多个匹配的行（按照可能是更好的匹配来排列它们）。类似，一个特殊词的搜索将不会找出不包含该词但包含其他相关词的行。

  ​	所有的这些限制以及更多的限制都可以使用全文本搜索来解决。在使用全文本搜索时，MySQL不需要分别查看每个行，不需要分别分析和处理每个词。MySQL创建指定列中各词的一个索引，搜索可以针对这些词进行。这样，MySQL可以快速有效的决定哪些词匹配（那些行包括他们），哪些词不匹配，他们的匹配频率等等。

  #### 使用全文索引

  ​	为了使用全文索引，必须索引被搜索的列，而且要随着数据的改变不断的重新索引。在对标列进行适当设计之后，MySQL会自动的进行所有的索引和重新索引。

  ​	在索引之后，select可与match()和Against()一起使用以实际执行搜索。

  ##### 启动全文本搜索支持

  ​	一般在创建表时启用全文本搜索。CREATE TABLE语句接受FULLTEXT子句。如：

  ```sql
  CREATE TABLE Product_Note
  (
  	id int NOT NULL AUTO_INCREMENT,
    prod_id char(10) NOT NULL,
    note_date datetime NOT NULL,
    note_text text NULL,
    PRIMARY KEY(id),
    FULLTEXT(note_text)
  )ENGINE=MyISAM;
  ```

> ##### 不要在导入数据时使用FULLTEXT
>
> ​	更新索引要花时间，若正在导入一个新表，此时不应该启用FULLTEXT索引，应该先导入所有数据，然后再修改表定义FULLTEXT索引，这样有助于更快导入数据

##### 进行全文本搜索

​	在索引之后，使用两个函数Match()和Against()执行全文本搜索，其中Match()指定被搜索的列，Against()指定要使用的搜索表达式。

​	如：

```sql
-- 此select语句检索单个列note_text。由于where子句，一个全文本搜索被执行。Match(note_text)指示MySQL针对指定的列机型搜索，Against('rabbit')指定词rabbit作为搜索文本。
select note_text 
from Product_Note
where Match(note_text) Against('rabbit');
```

> ##### 使用完整的Match()说明 
>
> ​	传递给Match()的值必须与FULLTEXT()定义中的完全相同，如果指定多个列，则必须列出它们（而且次序正确）。
>
> ##### 搜索不区分大小写
>
> ​	除非使用BINARY方式

​	上述SQL使用LIKE子句也可以完成，但是和全文本搜索的区别在于，全文本搜索会返回有意义的排序，会根据词汇的出现频率进行排序，Match(note_text) Against('rabbit')会建立一个计算列，此列包含全文本搜索计算出的等级值。等级值由MySQL根据行中词的数目、唯一词的数目、整个索引中词的总数以及包含该词的行的数目计算出来。

> 全文本索引提供了简单like搜索不提供的功能。而且，由于数据是索引的，全文本搜索还很快。

#### 插入语句

##### AUTO_INCREMENT

> **确定AUTO_INCREMENT的值**
>
> ​	执行插入操作时，MySQL会对该列进行自动增量，但若是在程序中执行插入操作之后需要知道新插入的行的ID，则可以使用last_insert_id()函数获得这个值，如
>
> ```sql
> select last_insert_id();
> ```
>
> ​	可以返回最后一个自动增长的值。

#### 引擎类型

​	MySQL多种具体管理和处理数据的内部引擎。当你使用select、update等语句的时候，这些引擎在内部处理你的请求，不同的引擎有不同的特性。

- InnoDB是一个可靠的事务处理引擎，它不支持全文本搜索。
- MEMORY在功能上等同于MyISAM，但由于数据存储在内存而不是磁盘，因此书读很快，很适合做临时表。
- MyISAM是一个性能极高的引擎，支持全文本搜索，但不支持事务。

> 引擎可以混用，即不同的表可以在创建时指定不同的引擎。但要注意外键不能跨引擎

#### 视图

​	视图是虚拟的表，只包含使用时动态检索数据的查询。

##### 为什么使用视图

- 重用SQL语句。
- 简化复杂的SQL操作，在编写查询后，可以方便的重用而不必知道它的查询细节。
- 使用表的组成部分而不是整个表。
- 保护数据。可以给用户授予表的特定部分的访问权限而不是整个表的访问权限。
- 更改数据格式和标识。视图可返回与底层表的表示和格式不同的数据。
- 隐藏复杂的SQL。

​	在视图创建后，可以用与表基本相同的方式来使用它们。可SELECT，过滤和排序数据，将视图联结到其他视图或表，甚至能添加和更新数据（添加和更新数据存在某些限制）

> **性能问题**
>
> 因为视图不包含数据，所以每次使用视图时，都必须处理查询执行时所需的任一个检索。如果使用多个联结和过滤床架了复杂的视图或者嵌套了视图，可能会发现性能下降的很厉害，因此在部署使用了大量视图的应用之前，应该先进行测试。

#####  视图的规则和限制

- 视图命名必须唯一
- 对于可以创建的视图数目没有限制。
- 为了创建视图，必须拥有足够的访问权限。
- 视图可以嵌套，即可以利用从其他视图中检索出数据的查询语句来构造一个视图。
- ORDER BY可以用在视图中，但如果从该视图检索数据的SELECT中也含有ORDER BY，那么该视图中的ORDER BY将被覆盖。
- 视图不能索引，也不能有关联的触发器或默认值。
- 视图可以和表一起使用，例如可以编写一条联结表和视图的select语句。

##### 更新视图

​	视图一般和SELECT语句使用，一般情况下可以更新（即可以对视图使用INSERT、UPDATE和DELECT），但特定情况下不能更新视图。

​	更新一个视图将更新其基表，即对基表增加或者删除行。但并非所有的视图都是可更新的，若视图定义中存在以下操作，则不能进行视图的更新：

- 分组（使用GROUP BY和HAVING）
- 联结
- 子查询
- 并
- 聚集函数（Min()、Count()、Sum()等）
- DISTINCT
- 计算列

> 上述情况对于MySQL 5以来是正确的，未来可能取消一些限制，但是视图应该是用于检索数据而不是更新。

#### 存储过程

​	MySQL5添加了对存储过程的支持。

​	一般使用的大多数SQL番禺局都是针对一个或多个表的单条语句，但并非所有的操作都这么简单，经常会有一个完整的操作需要多条语句才能完成。例如下面的场景：

- 为了处理订单，需要核对以保证库存中有相应的物品。
- 若库存中有物品，这些物品需要预定以便不将它们再卖给别人，并且要减少可用的物品数量以反映正确的库存量。
- 库存中没有的物品需要订购，这需要与供应商进行某种交互。
- 关于那些物品可以入库（并且可以立即发货）和哪些物品退订，需要通知相应的客户。

​	执行这个处理需要针对徐对标的多条MySQL语句，此外，需要执行的具体语句及其顺序也不是固定的，它们可能会根据物品在库存中那些不在而变化。

​	存储过程简单来说，就是为以后的使用而保存的一条或多条MySQL语句的集合，可将其视为批文件，虽然存储过程的作用不仅限于批处理。

##### 使用存储过程的理由

- 通过把处理封装在容易使用的单元中，简化复杂的操作。
- 由于不需要反复建立一系列处理步骤，这保证了数据的完整性。
- 简化对变动的管理。如果表名、列名或业务逻辑等有变化，只需要更改存储过程的代码，使用人员不需要知道这些变化。
- 提高性能。因为使用存储过程比使用单独的SQL语句更快。
- 存在一些只能用在单个请求中的MySQL元素和特性，存储过程可以使它们来编写更强更灵活的代码。

> 换句话说，使用存储过程即简单高效还安全、高性能。

##### 存储过程的缺陷

- 一般来说，存储过程的编写比基本SQL复杂，需要更高的技能和更丰富的经验。

#### 存储过程的使用

##### 执行存储过程

​	MySQL称存储过程的执行为调用，因此MySQL执行存储过程的语句为CALL，CALL接受存储过程的名字以及需要传递给它的任意参数，如下例子：

```sql
CALL productpricing(@pricelow,@procehigh,@priceaverage);
-- 执行名为productpricing的存储过程，它计算并返回产品的最低、最高和平均价格，存储过程可以显示结果，也可以不显示结果。
```

##### 创建存储过程

```sql
CREATE PROCEDURE productpricing()
BEGIN
	SELECT Avg(prod_price) as average
	from products
END;
-- 创建名为productpricing的存储过程，若接受参数，则在括号()中列举。BEGIN和END用来限定存储过程体
```

​	在MySQL处理上述代码时，将创建一个新的存储过程，但是并没有调用这个存储过程。

> **MySQL命令行客户机的分隔符**
>
> 如果你使用的是mysql命令行 实用程序，应该仔细阅读此说明。
>
> 默认的MySQL语句分隔符为;(正如你已经在迄今为止所使用 的MySQL语句中所看到的那样)。mysql命令行实用程序也使 用;作为语句分隔符。如果命令行实用程序要解释存储过程自 身内的;字符，则它们最终不会成为存储过程的成分，这会使 存储过程中的SQL出现句法错误。
>
> 解决办法是临时更改命令行实用程序的语句分隔符，如下所示:
>
> ```sql
> DELIMITER //
> 
> CREATE PROCEDURE productpricing()
> BEGIN
> 	SELECT Avg(prod_price) as average
> 	from products
> END//
> 
> DELIMITER ;
> ```
>
> 其中，DELIMITER //告诉命令行实用程序使用//作为新的语 句结束分隔符，可以看到标志存储过程结束的END定义为END //而不是END;。这样，存储过程体内的;仍然保持不动，并且 正确地传递给数据库引擎。最后，为恢复为原来的语句分隔符，

##### 使用存储过程

```sql
CALL productpricing();
```

删除存储过程

```sql
drop PROCEDURE productpricing;

-- 或者当存在时才删除
DROP PROCEDURE IF EXISTS productpricing;
```

##### 使用参数

​	一般，存储过程并不显示结果，而是把结果返回给你指定的变量。下面创建一个使用参数的存储过程。

```sql
CREATE PROCEDURE productpricing(
	OUT pl DECIMAL(8,2),
  OUT ph DECIMAL(8,2),
  OUT pa DECIMAL(8,2)
)
BEGIN
	SELECT Min(price) INTO pl from products;
	SELECT Max(price) INTO ph from products;
	SELECT Avg(price) INTO pa from products;
END;
```

​	上述存储过程接受三个输出类型的参数，每个参数都必须具有指定的类型。OUT关键字指出相应的参数用来从存储过程传出一个值返回给调用者。MySQL支持IN（传递给存储过程）、OUT（从存储过程中传出）和INOUT（对存储过程传入和传出）类型的参数。

​	存储过程的代码位于BEGIN和END语句内，是一系列SELECT语句，用来检索值然后保存到相应的变量（通过指定INTO关键字）。

> **参数的数据类型**	存储过程的参数允许的数据类型与表中使用的数据类型相同。PS：记录集不是允许的类型，因此，不能通过一个参数返回多个行和列。

​	为了调用上述存储过程，必须指定3个变量名，如下所示：

```sql
CALL productpricing(@low,@high,@average);
-- 用上述3个变量来接受存储过程返回的结果
```

> **变量名** 所有MySQL变量都必须以@开始。
>
> ```sql
> -- 查询变量，多个用,隔开
> SELECT @low,@high,@average
> ```

​	下面是另一个例子，使用IN和OUT参数。

```sql
CREATE PROCEDURE ordertotal(
	IN orderNum INT,
  OUT orderTotal DECIMAL(8,2)
)
BEGIN
	SELECT Sum(item_price * quantity)
	FROM orderitems
	WHERE order_num = orderNum
	INTO orderTotal;
END;

-- 调用上述存储过程
CALL ordertotal(20005,@total);
-- 查询存储过程结果
SELECT @total;
```

##### 建立智能存储过程

​	上述所有涉及到的存储过程基本上都是封装MySQL简单的SELECT语句，这些工作使用单独的SELECT语句都可以完成，只有在存储过程中包含业务规则和智能处理时，才是存储过程的正确使用方式。

​	考虑场景：获得与之前一样的订单合计，但需要对合计增加营业税，不过只针对某些顾客，那么将要做以下几件事情：

- 获得合计；
- 把营业税有条件的添加到合计；
- 返回合计（带税或不带税）；

上述完整存储过程如下：

```sql
-- Name：ordertotal
-- Parameters：onumber = order number
-- taxable = 0 if not taxable,1 if taxable
-- ototal = order total variable
CREATE PROCEDURE ordertotal(
	IN onumber INT,
  IN taxable BOOLEAN,
  OUT ototal DECIMAL(8,2)
) COMMENT 'Obtain order total,optionally adding tax'
BEGIN
	-- Declare variable for total
	DECLARE total DECIMAL(8,2);
	-- Declare tax percentage
	DECLARE taxrate INT DEFAULT 6;
	
	-- Get the order total
	SELECT Sum(item_price * quantity)
	FROM orderitems
	WHERE order_num = onumber
	INTO ototal;
	
	-- Is this taxable?
	IF taxable THEN
		SELECT total + (total / 100 * taxrate) INTO total;
	END IF;
	
	-- Finally ,save to out variable
	SELECT total INTO ototal;
END
```

​	上述存储过程添加了逻辑判断处理，是一个更高级功能更强的一个存储过程。

> **IF语句** 这个例子给出了MySQL的IF语句的基本用法。
>
> IF语句还支持ELSEIF和ELSE子句（前者还是用THEN子句，后者不使用）

#### 检查存储过程

```sql
SHOW CREATE PROCEDURE ordertotal;
```

#### 事务处理

​	事务处理（Transaction processing）可以用来维护数据库的完整性，它保证成批的MySQL操作要么完全执行，要么完全不执行。

​	事务术语：

- 事务（Transaction）指一组SQL语句；
- 回退（rollback）指撤销指定SQL语句的过程；
- 提交（commit）指将为存储的SQL语句结果写入数据库表；
- 保留点（savepoint）指事务处理中设置的临时占位符（place-holder），你可以对它发布回退（与回退整个事务处理不同）。

#### 控制事务处理

​	管理事务处理的关键在于将SQL语句组分解成为逻辑块，并明确规定数据何时应该回退，何时不应该回退。

​	MySQL使用下面的语句来标识事务的开始：

```sql
START TRANSACTION
```

##### 使用ROLLBACK

​	ROLLBACK命令可以用来回退（撤销）MySQL语句，如下例：

```sql
select * from house;
START TRANSACTION;
delete from house;
select * from house;
ROLLBACK;
```

​	显然，ROLLBACK只能在一个事务处理内使用（在执行一条START TRANSACTION命令之后）。

> **那些语句可以回退？** 事务管理用来管理INSERT、UPDATE和DELECT语句。不能回退CREATE或DROP操作，若在事务中执行回退，它们不会被撤销。

##### 使用COMMIT

​	一般的MySQL语句都是直接针对数据库执行和编写的，这就是所谓的隐含提交（implicit commit），即提交（写或保存）操作是自动进行的。

​	但是在事务处理块中，提交不会隐含的进行，为了进行明确的提交，使用COMMIT语句，如下所示：

```sql
START TRANSACTION;
-- some insert or other update operation
DELETE FROM house where id = 1;
DELETE FROM user where id = 2;
COMMIT;
```

​	上述两条删除语句中若有某一条删除失败则整个事务不会提交，会被自动撤销。

> **隐含事务关闭**  当COMMIT或ROLLBACK语句执行之后，事务会自动关闭，即之后的操作会变成默认的隐含提交而不再是需要手动提交。

##### 使用保留点

​	简单的ROLLBACK和COMMIT语句可以撤销或写入整个事务处理。但是，对简单的事务处理才能这样做，更复杂的事务处理可能需要部分提交或回退。

​	为了支持回退部分事务处理，必须能在事务处理中合适的位置放置占位符，这样如果需要回退，可以回退到某个占位符。

​	这些占位符成为保留点，为了创建占位符，可使用如下语句：

```sql
SAVEPOINT delete1;
```

​	每个保留点都取唯一标识，在回退时要指定回退的保留点。

```sql
ROLLBACK TO delete1;
```

> **保留点越多越好**
>
> ​	保留点越多就越可以按照自己的意愿来进行回退。
>
> **释放保留点**
>
> ​	保留点在事务处理完成（执行一条commit或者rollback）后自动释放，MySQL5.0之后可以用RELEASE SAVEPOINT明确的释放保留点。

#### 更改默认的提交行为

​	MySQL默认提交所有更改，可使用下面的语句进行取消更改：

```sql
SET autocommit=0;

-- PS:关闭安全更新模式
SET SQL_SAFE_UPDATES=0;
```

### 改善性能

​	当出现性能问题时，性能不良的数据库（以及数据库查询）通常是最常见的原因。

​	下面给出的意见只是大部分性能问题的原因，仅供参考

- MySQL有特定的硬件建议，生产环境建议遵循该硬件建议，包括所有的DBMS，一般来说应该运行在自己的专用服务器上。
- MySQL用一系列的默认设置预先设置好的，通常没有什么问题，但过一段时间后可能需要调整内存分配、缓冲区大小等（可使用SHOW VARIABLES和SHOW STATUS来查看当前设置。
- MySQL作为一个多用户多线程的DBMS，通常同时执行多个任务，若这些任务中的某一个执行缓慢，则所有的请求都会执行缓慢，如果遇到性能不良，可以使用SHOW PROCESSLIST显示所有的活动进程，还可以使用KILL命令终结某个特定的进程。
- 总是有不知一种方法编写同一条SELECT语句，应该试验联结、并、子查询等等，找出最佳的方式。
- 使用 EXPLAIN语句让MySQL解释它将如何执行一条SELECT语句。
- 一般来说，存储过程执行的比一条一条地执行其中各条MySQL语句快。
- 应该总是使用正确的数据类型。
- 只检索你要的列。
- 有的操作（包括INSERT）支持一个可选的DELAYED关键字，如果使用该关键字，将把控制立即返回给调用程序，并且一旦有可能就实际执行该操作。
- 在导入数据时，应该关闭自动提交，还可以删除索引，等到导入之后再重建索引
- 必须索引数据库表以改善检索的性能。分析使用的SELECT语句以找出重复的where和order by子句，若一个简单的WHERE子句返回结果要花很长时间，则可以断定其中使用的列（或几个列）就是需要索引的对象。
- 使用过多的or条件有可能降低性能，可以尝试使用多条SELECT语句和连接它们的union语句来改善性能。
- 索引改善数据检索的性能，但损害数据插入、删除和更新的性能。若有一些表经常更新但是查询很少，则不应当对该表进行索引（可根据需要添加和删除）。
- 以上每条规则在某些条件写都会被打破。









