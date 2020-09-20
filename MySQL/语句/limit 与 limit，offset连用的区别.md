# limit 与 limit，offset连用的区别

```sql
 select * 
 from table 
 limit 2,1;        
```

含义是跳过2条取出1条数据，limit后面是从第2条开始读，读取1条信息，即读取第3条数据

```sql
select * 
from table 
limit 2 
offset 1;   
```

含义是从第1条（不包括）数据开始取出2条数据，limit后面跟的是2条数据，offset后面是从第1条开始读取，即读取第2,3条

使用查询语句的时候，经常要返回前几条或者中间某几行数据，这个时候怎么办呢？不用担心，已 经为我们提供了这样一个功能。
 LIMIT 子句可以被用于强制 SELECT 语句返回指定的记录数。LIMIT 接受一个或两个数字参数。参数必须是一个整数常量。
如果给定两个参数，第一个参数指定第一个返回记录行的偏移量，第二个参数指定返回记录行的最大数目。

```sql
SELECT * 
FROM table   
LIMIT [offset,] rows | rows 
OFFSET offset
```

这是两个参数，第一个是偏移量，第二个是数目

```sql
select * from employee limit 3, 7; // 返回4-11行
select * from employee limit 3,1; // 返回第4行
```



一个参数

```sql
select * from employee limit 3; // 返回前3行
```



参考

- <a href="https://blog.csdn.net/AinUser/article/details/72803175" target="">sql 中 limit 与 limit，offset连用的区别</a>