# Mysql

###  in和exist的区别

```sql
select * from a where a.id in (select a_id from b)
```

若a表的数据远大于