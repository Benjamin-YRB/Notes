# Mysql

#### 关键字

##### REGEXP

where子句中可以使用REGEXP关键字来对列使用正则表达式

```sql
SELECT *
FROM customers
WHERE LAST_NAME REGEXP 'name'
--上述条件相当于 LAST_NAME LIKE '%name%'

WHERE LAST_NAME REGEXP '^name'
--表示name开头，相当于 LAST_NAME LIKE 'name%'

WHERE LAST_NAME REGEXP 'name$'
--表示name结尾，相当于 LAST_NAME LIKE '%name'

WHERE LAST_NAME REGEXP 'name$|^name|name'
--也可以使用管道|匹配多个表达式

WHERE LAST_NAME REGEXP '[abc]e'
-- 多种搜索模式，上述语句匹配包含‘ae’、‘be’、‘ce’的行

WHERE LAST_NAME REGEXP '[abc]e'

```

