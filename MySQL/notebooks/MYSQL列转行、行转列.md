# MYSQL列转行、行转列

## MYSQL列转行

用 `union / union all`函数

<img src=".\img\image-20200824172618414.png" alt="image-20200824172618414" style="zoom:80%;" />

**数据导入**

```sql
create table a1
(
    name    varchar(20),
    english int,
    maths   int,
    music   int
);
insert into a1
values ("Jim", 90, 88, 99);

select * from a1;
```

```sql
select name, 'english' as subject, english as score
from a1
union
select name, 'maths' as subject, maths as subject
from a1
union
select name, 'music' as subject, music as subject
from a1;
```

<img src=".\img\image-20200826200703052.png" alt="image-20200826200703052" style="zoom:80%;" />



```sql
select name, 'english' as subject, english as score
from a1
```

select 可以将表中没有的字段筛选出来，如本题中的 ` 'english' as subject` , 注意引号的使用。



## MYSQL行转列

用 `max`函数

```sql
create table tb(
    姓名 varchar(10),
    课程 varchar(3),
    分数 int(3)

);

insert into tb values ('张三','语文', 74),
                      ('张三','数学', 83),
                      ('张三','物理', 93),
                      ('李四','语文', 74),
                      ('李四','数学', 84),
                      ('李四','物理', 94);
```

<img src=".\img\image-20200826201042293.png" alt="image-20200826201042293" style="zoom:80%;" />

想要显示的结果是 ：姓名 语文 数学 物理 这种形式的（就是行转列）

```sql
SELECT `姓名`,
       MAX(CASE `课程` WHEN '语文' THEN `分数` ELSE 0 END) '语文',
       MAX(CASE `课程` WHEN '数学' THEN `分数` ELSE 0 END) '数学',
       MAX(CASE `课程` WHEN '物理' THEN `分数` ELSE 0 END) '物理'
FROM TB
GROUP BY `姓名`;
```

<img src=".\img\image-20200826201212843.png" alt="image-20200826201212843" style="zoom:80%;" />







参考

- <a href="https://hg1227.github.io/2020/03/24/MySQL-case-when-与-max()-等聚合函数联合使用/" target="_blank">MySQL case when 与 max() 聚合函数联合使用</a> 