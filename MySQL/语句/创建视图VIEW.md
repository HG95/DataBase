# 创建视图VIEW

Q：什么是视图？视图是干什么用的？

A：

　　视图（view）是一种虚拟存在的表，是一个逻辑表，本身并不包含数据。作为一个select语句保存在数据字典中的。

　　通过视图，可以展现基表的部分数据；视图数据来自定义视图的查询中使用的表，使用视图动态生成。

基表：用来创建视图的表叫做基表base table

Q：为什么要使用视图？

A：因为视图的诸多优点，如下

　　1）简单：使用视图的用户完全不需要关心后面对应的表的结构、关联条件和筛选条件，对用户来说已经是过滤好的复合条件的结果集。

　　2）安全：使用视图的用户只能访问他们被允许查询的结果集，对表的权限管理并不能限制到某个行某个列，但是通过视图就可以简单的实现。

　　3）数据独立：一旦视图的结构确定了，可以屏蔽表结构变化对用户的影响，源表增加列对视图没有影响；源表修改列名，则可以通过修改视图来解决，不会造成对访问者的影响。

总而言之，使用视图的大部分情况是为了保障数据安全性，提高查询效率。



## 创建视图

```sql
CREATE [OR REPLACE] [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
    VIEW view_name [(column_list)]
    AS select_statement
   [WITH [CASCADED | LOCAL] CHECK OPTION]
```

1）`OR REPLACE`：表示替换已有视图

2）`ALGORITHM`：表示视图选择算法，默认算法是 UNDEFINED(未定义的)：MySQL自动选择要使用的算法 ；merge合并；temptable临时表

3）`select_statement`：表示`select`语句

4）`[WITH [CASCADED | LOCAL] CHECK OPTION]`：表示视图在更新时保证在视图的权限范围之内

　　cascade是默认值，表示更新视图的时候，要满足视图和表的相关条件

　　local表示更新视图的时候，要满足该视图定义的一个条件即可

TIPS：推荐使用 `WHIT [CASCADED|LOCAL] CHECK OPTION`选项，可以保证数据的安全性 



### 基本格式：

```sql
create view <视图名称>[(column_list)]

as select语句

with check option;
```

### 1、在单表上创建视图

```sql
mysql> create view v_F_players(编号,名字,性别,电话)
    -> as
    -> select PLAYERNO,NAME,SEX,PHONENO from PLAYERS
    -> where SEX='F'
    -> with check option;
Query OK, 0 rows affected (0.00 sec)

mysql> desc v_F_players;
+--------+----------+------+-----+---------+-------+
| Field  | Type     | Null | Key | Default | Extra |
+--------+----------+------+-----+---------+-------+
| 编号    | int(11)  | NO   |     | NULL    |       |
| 名字    | char(15) | NO   |     | NULL    |       |
| 性别    | char(1)  | NO   |     | NULL    |       |
| 电话    | char(13) | YES  |     | NULL    |       |
+--------+----------+------+-----+---------+-------+
4 rows in set (0.00 sec)

mysql> select * from  v_F_players;
+--------+-----------+--------+------------+
| 编号    | 名字      | 性别    | 电话        |
+--------+-----------+--------+------------+
|      8 | Newcastle | F      | 070-458458 |
|     27 | Collins   | F      | 079-234857 |
|     28 | Collins   | F      | 010-659599 |
|    104 | Moorman   | F      | 079-987571 |
|    112 | Bailey    | F      | 010-548745 |
+--------+-----------+--------+------------+
5 rows in set (0.02 sec)
```

### 1、在多表上创建视图

```sql
mysql> create view v_match
    -> as 
    -> select a.PLAYERNO,a.NAME,MATCHNO,WON,LOST,c.TEAMNO,c.DIVISION
    -> from 
    -> PLAYERS a,MATCHES b,TEAMS c
    -> where a.PLAYERNO=b.PLAYERNO and b.TEAMNO=c.TEAMNO;
Query OK, 0 rows affected (0.03 sec)

mysql> select * from v_match;
+----------+-----------+---------+-----+------+--------+----------+
| PLAYERNO | NAME      | MATCHNO | WON | LOST | TEAMNO | DIVISION |
+----------+-----------+---------+-----+------+--------+----------+
|        6 | Parmenter |       1 |   3 |    1 |      1 | first    |
|       44 | Baker     |       4 |   3 |    2 |      1 | first    |
|       83 | Hope      |       5 |   0 |    3 |      1 | first    |
|      112 | Bailey    |      12 |   1 |    3 |      2 | second   |
|        8 | Newcastle |      13 |   0 |    3 |      2 | second   |
+----------+-----------+---------+-----+------+--------+----------+
5 rows in set (0.04 sec)
```

视图将我们不需要的数据过滤掉，将相关的列名用我们自定义的列名替换。视图作为一个访问接口，不管基表的表结构和表名有多复杂。

 　如果创建视图时不明确指定视图的列名，那么列名就和定义视图的select子句中的列名完全相同；

　　如果显式的指定视图的列名就按照指定的列名。

注意：显示指定视图列名，要求视图名后面的列的数量必须匹配select子句中的列的数量。







参考

- <a href="https://www.cnblogs.com/geaozhang/p/6792369.html" target="_blank">深入解析MySQL视图VIEW</a>

