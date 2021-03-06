# INSERT语句

## INSERT：插入数据（添加数据）

数据库与表创建成功以后，需要向数据库的表中插入数据。在 [MySQL](http://c.biancheng.net/mysql/) 中可以使用 INSERT 语句向数据库已有的表中插入一行或者多行元组数据。

基本语法

INSERT 语句有两种语法形式，分别是 INSERT…VALUES 语句和 INSERT…SET 语句。

##  INSERT…VALUES语句

INSERT VALUES 的语法格式为：

```sql
INSERT INTO <表名> [ <列名1> [ , … <列名n>] ]
VALUES (值1) [… , (值n) ];
```



语法说明如下。

- `<表名>`：指定被操作的表名。
- `<列名>`：指定需要插入数据的列名。若向表中的所有列插入数据，则全部的列名均可以省略，直接采用 INSERT<表名>VALUES(…) 即可。
- `VALUES` 或 `VALUE` 子句：该子句包含要插入的数据清单。数据清单中数据的顺序要和列的顺序相对应。

## INSERT…SET语句

语法格式为：

```sql
INSERT INTO <表名>
SET <列名1> = <值1>,
    <列名2> = <值2>,
    …
```



此语句用于直接给表中的某些列指定对应的列值，即要插入的数据的列名在 SET 子句中指定，col_name 为指定的列名，等号后面为指定的数据，而对于未指定的列，列值会指定为该列的默认值。

由 INSERT 语句的两种形式可以看出：

- 使用 `INSERT…VALUES` 语句可以向表中插入一行数据，也可以插入多行数据；
- 使用 `INSERT…SET` 语句可以指定插入行中每列的值，也可以指定部分列的值；
- `INSERT…SELECT `语句向表中插入其他表的数据。
- 采用` INSERT…SET `语句可以向表中插入部分列的值，这种方式更为灵活；
- `INSERT…VALUES` 语句可以一次插入多条数据。


在 MySQL 中，用单条 INSERT 语句处理多个插入要比使用多条 INSERT 语句更快。

当使用单条 INSERT 语句插入多行数据的时候，只需要将每行数据用圆括号括起来即可。

## 向表中的全部字段添加值

在 test_db 数据库中创建一个课程信息表 tb_courses，包含课程编号 course_id、课程名称 course_name、课程学分 course_grade 和课程备注 course_info，输入的 SQL 语句和执行结果如下所示。

```sql
mysql> CREATE TABLE tb_courses
    -> (
    -> course_id INT NOT NULL AUTO_INCREMENT,
    -> course_name CHAR(40) NOT NULL,
    -> course_grade FLOAT NOT NULL,
    -> course_info CHAR(100) NULL,
    -> PRIMARY KEY(course_id)
    -> );
Query OK, 0 rows affected (0.00 sec)
```

向表中所有字段插入值的方法有两种：一种是指定所有字段名；另一种是完全不指定字段名。

【实例 1】在 tb_courses 表中插入一条新记录，course_id 值为 1，course_name 值为“Network”，course_grade 值为 3，info 值为“Computer Network”。

在执行插入操作之前，查看 tb_courses 表的SQL语句和执行结果如下所示。

```sql
mysql> SELECT * FROM tb_courses;
Empty set (0.00 sec)
```

查询结果显示当前表内容为空，没有数据，接下来执行插入数据的操作，输入的 SQL 语句和执行过程如下所示。

```sql
mysql> INSERT INTO tb_courses
    -> (course_id,course_name,course_grade,course_info)
    -> VALUES(1,'Network',3,'Computer Network');
Query OK, 1 rows affected (0.08 sec)
mysql> SELECT * FROM tb_courses;
+-----------+-------------+--------------+------------------+
| course_id | course_name | course_grade | course_info      |
+-----------+-------------+--------------+------------------+
|         1 | Network     |            3 | Computer Network |
+-----------+-------------+--------------+------------------+
1 row in set (0.00 sec)
```

可以看到插入记录成功。在插入数据时，指定了 tb_courses 表的所有字段，因此将为每一个字段插入新的值。

INSERT 语句后面的列名称顺序可以不是 tb_courses 表定义时的顺序，即插入数据时，不需要按照表定义的顺序插入，只要保证值的顺序与列字段的顺序相同就可以。

【实例 2】在 tb_courses 表中插入一条新记录，course_id 值为 2，course_name 值为“Database”，course_grade 值为 3，info值为“MySQL”。输入的 SQL 语句和执行结果如下所示。

```sql
mysql> INSERT INTO tb_courses
    -> (course_name,course_info,course_id,course_grade)
    -> VALUES('Database','MySQL',2,3);
Query OK, 1 rows affected (0.08 sec)
mysql> SELECT * FROM tb_courses;
+-----------+-------------+--------------+------------------+
| course_id | course_name | course_grade | course_info      |
+-----------+-------------+--------------+------------------+
|         1 | Network     |            3 | Computer Network |
|         2 | Database    |            3 | MySQL            |
+-----------+-------------+--------------+------------------+
2 rows in set (0.00 sec)
```

使用 INSERT 插入数据时，允许列名称列表 column_list 为空，此时值列表中需要为表的每一个字段指定值，并且值的顺序必须和数据表中字段定义时的顺序相同。

【实例 3】在 tb_courses 表中插入一条新记录，course_id 值为 3，course_name 值为“[Java](http://c.biancheng.net/java/)”，course_grade 值为 4，info 值为“Jave EE”。输入的 SQL 语句和执行结果如下所示。

```sql
mysql> INSERT INTO tb_courses
    -> VLAUES(3,'Java',4,'Java EE');
Query OK, 1 rows affected (0.08 sec)
mysql> SELECT * FROM tb_courses;
+-----------+-------------+--------------+------------------+
| course_id | course_name | course_grade | course_info      |
+-----------+-------------+--------------+------------------+
|         1 | Network     |            3 | Computer Network |
|         2 | Database    |            3 | MySQL            |
|         3 | Java        |            4 | Java EE          |
+-----------+-------------+--------------+------------------+
3 rows in set (0.00 sec)
```

INSERT 语句中没有指定插入列表，只有一个值列表。在这种情况下，值列表为每一个字段列指定插入的值，并且这些值的顺序必须和 tb_courses 表中字段定义的顺序相同。

> 注意：虽然使用 INSERT 插入数据时可以忽略插入数据的列名称，若值不包含列名称，则 VALUES 关键字后面的值不仅要求完整，而且顺序必须和表定义时列的顺序相同。如果表的结构被修改，对列进行增加、删除或者位置改变操作，这些操作将使得用这种方式插入数据时的顺序也同时改变。如果指定列名称，就不会受到表结构改变的影响。

## 向表中指定字段添加值

为表的指定字段插入数据，是在 INSERT 语句中只向部分字段中插入值，而其他字段的值为表定义时的默认值。

【实例 4】在 tb_courses 表中插入一条新记录，course_name 值为“System”，course_grade 值为 3，course_info 值为“Operating System”，输入的 SQL 语句和执行结果如下所示。

```sql
mysql> INSERT INTO tb_courses
    -> (course_name,course_grade,course_info)
    -> VALUES('System',3,'Operation System');
Query OK, 1 rows affected (0.08 sec)
mysql> SELECT * FROM tb_courses;
+-----------+-------------+--------------+------------------+
| course_id | course_name | course_grade | course_info      |
+-----------+-------------+--------------+------------------+
|         1 | Network     |            3 | Computer Network |
|         2 | Database    |            3 | MySQL            |
|         3 | Java        |            4 | Java EE          |
|         4 | System      |            3 | Operating System |
+-----------+-------------+--------------+------------------+
4 rows in set (0.00 sec)
```

可以看到插入记录成功。如查询结果显示，这里的 course_id 字段自动添加了一个整数值 4。这时的 course_id 字段为表的主键，不能为空，系统自动为该字段插入自增的序列值。在插入记录时，如果某些字段没有指定插入值，MySQL 将插入该字段定义时的默认值。

##  INSERT INTO…FROM 语句复制表数据

INSERT INTO…SELECT…FROM 语句用于快速地从一个或多个表中取出数据，并将这些数据作为行数据插入另一个表中。

SELECT 子句返回的是一个查询到的结果集，INSERT 语句将这个结果集插入指定表中，结果集中的每行数据的字段数、字段的数据类型都必须与被操作的表完全一致。

在数据库 test_db 中创建一个与 tb_courses 表结构相同的数据表 tb_courses_new，创建表的 SQL 语句和执行过程如下所示。

```sql
mysql> CREATE TABLE tb_courses_new
    -> (
    -> course_id INT NOT NULL AUTO_INCREMENT,
    -> course_name CHAR(40) NOT NULL,
    -> course_grade FLOAT NOT NULL,
    -> course_info CHAR(100) NULL,
    -> PRIMARY KEY(course_id)
    -> );
Query OK, 0 rows affected (0.00 sec)
mysql> SELECT * FROM tb_courses_new;
Empty set (0.00 sec)
```

【实例 5】从 tb_courses 表中查询所有的记录，并将其插入 tb_courses_new 表中。输入的 SQL 语句和执行结果如下所示。

```sql
mysql> INSERT INTO tb_courses_new
    -> (course_id,course_name,course_grade,course_info)
    -> SELECT course_id,course_name,course_grade,course_info
    -> FROM tb_courses;
Query OK, 4 rows affected (0.17 sec)
Records: 4  Duplicates: 0  Warnings: 0
mysql> SELECT * FROM tb_courses_new;
+-----------+-------------+--------------+------------------+
| course_id | course_name | course_grade | course_info      |
+-----------+-------------+--------------+------------------+
|         1 | Network     |            3 | Computer Network |
|         2 | Database    |            3 | MySQL            |
|         3 | Java        |            4 | Java EE          |
|         4 | System      |            3 | Operating System |
+-----------+-------------+--------------+------------------+
4 rows in set (0.00 sec)
```

