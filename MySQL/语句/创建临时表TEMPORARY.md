# 创建临时表TEMPORARY



## 创建临时表

下面是创建临时表以及插入数据的例子

A、临时表再断开于mysql的连接后系统会自动删除临时表中的数据，但是这只限于用下面语句建立的表：

1)定义字段

```sql
CREATE TEMPORARY TABLE tmp_table (
      name VARCHAR(10) NOT NULL, 
      time date  NOT NULL
);
```

更高级点就是：

```sql
create temporary  TABLE `temtable` (
  `jws` varchar(100) character set utf8 collate utf8_bin NOT NULL,
  `tzlb` varchar(100) character set utf8 collate utf8_bin NOT NULL,
  `uptime` date NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1″
```

连编码方式都规定了。。呵呵，以防乱码啊。

2)直接将查询结果导入临时表

```sql
 CREATE TEMPORARY TABLE tmp_table 
 SELECT * 
 FROM table_name;
```

B、另外mysql也允许你在内存中直接创建临时表，因为是在内存中所有速度会很快，语法如下：

```sql
CREATE TEMPORARY TABLE tmp_table (
     name VARCHAR(10) NOT NULL,
     value INTEGER NOT NULL
  ) TYPE = HEAP
```

那如何将查询的结果存入已有的表呢？

1、可以使用A中第二个方法

2、使用`insert into temtable (select a,b,c,d from tablea);`

首先，临时表只在当前连接可见，当关闭连接时，Mysql会自动删除表并释放所有空间。因此在不同的连接中可以创建同名的临时表，并且操作属于本连接的临时表。

创建临时表的语法与创建表语法类似，不同之处是增加关键字 `TEMPORARY`，如：

```sql
 CREATE TEMPORARY TABLE 表名 (…. )
```

##  删除临时表

临时表可以手动删除：

```sql
DROP TEMPORARY TABLE IF EXISTS temp_tb;
```





参考

- <a href="https://my.oschina.net/airship/blog/630409" target="_blank">mysql创建临时表，将查询结果插入已有表中</a>

