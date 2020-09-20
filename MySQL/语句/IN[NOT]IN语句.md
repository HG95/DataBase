# IN[NOT] IN语句

## MySQL 子查询

子查询是 MySQL 中比较常用的查询方法，通过子查询可以实现多表查询。子查询指将一个查询语句嵌套在另一个查询语句中。子查询可以在 `SELECT`、`UPDATE` 和  `DELETE` 语句中使用，而且可以进行多层嵌套。在实际开发时，子查询经常出现在 `WHERE` 子句中。

子查询在 WHERE 中的语法格式如下：

```sql
WHERE <表达式> <操作符> (子查询)
```



其中，操作符可以是比较运算符和 `IN`、`NOT IN`、`EXISTS`、`NOT EXISTS `等关键字。

## 1）IN \| NOT IN

当表达式与子查询返回的结果集中的某个值相等时，返回 TRUE，否则返回 FALSE；若使用关键字 NOT，则返回值正好相反。

`IN` 操作符用来指定条件范围，范围中的每个条件都可以进行匹配。`IN` 取一组由逗号分割、括在圆括号中的和合法值。

```sql
-- 42、查询每门功成绩最好的前两名
select t.c_id, t.s_score, t.r
from (
         select *, row_number() over (partition by c_id order by s_score desc ) r
         from score
     ) t
where t.r in (1, 2);
```



## 2）EXISTS \| NOT EXISTS

用于判断子查询的结果集是否为空，若子查询的结果集不为空，返回 TRUE，否则返回 FALSE；若使用关键字 NOT，则返回的值正好相反。

例 1

使用子查询在 tb_students_info 表和 tb_course 表中查询学习 Java 课程的学生姓名，SQL 语句和运行结果如下。

```sql
mysql> SELECT name FROM tb_students_info 
    -> WHERE course_id IN 
    ->(SELECT id FROM tb_course WHERE course_name = 'Java');
+-------+
| name  |
+-------+
| Dany  |
| Henry |
+-------+
2 rows in set (0.01 sec)
```

结果显示，学习 Java 课程的只有 Dany 和 Henry。上述查询过程也可以分为以下 2 步执行，实现效果是相同的。

1）**首先单独执行内查询**，查询出 tb_course 表中课程为 Java 的 id，SQL 语句和运行结果如下。

```sql
mysql> SELECT id FROM tb_course 
    -> WHERE course_name = 'Java';
+----+
| id |
+----+
|  1 |
+----+
1 row in set (0.00 sec)
```

可以看到，符合条件的 id 字段的值为 1。

2）**然后执行外层查询**，在 tb_students_info 表中查询 course_id 等于 1 的学生姓名。SQL 语句和运行结果如下。

```sql
mysql> SELECT name FROM tb_students_info 
    -> WHERE course_id IN (1);
+-------+
| name  |
+-------+
| Dany  |
| Henry |
+-------+
2 rows in set (0.00 sec)
```

习惯上，外层的 SELECT 查询称为父查询，圆括号中嵌入的查询称为子查询（子查询必须放在圆括号内）。MySQL 在处理上例的 SELECT 语句时，执行流程为：先执行子查询，再执行父查询。

例 2

与例 1 类似，在 SELECT 语句中使用 NOT IN 关键字，查询没有学习 Java 课程的学生姓名，SQL 语句和运行结果如下。

```sql
mysql> SELECT name FROM tb_students_info 
    -> WHERE course_id NOT IN (SELECT id FROM tb_course WHERE course_name = 'Java');
+--------+
| name   |
+--------+
| Green  |
| Jane   |
| Jim    |
| John   |
| Lily   |
| Susan  |
| Thomas |
| Tom    |
| LiMing |
+--------+
9 rows in set (0.01 sec)
```

可以看出，运行结果与例 1 刚好相反，没有学习 Java 课程的是除了 Dany 和 Henry 之外的学生。

例 3

使用`=`运算符，在 tb_course 表和 tb_students_info 表中查询出所有学习 Python 课程的学生姓名，SQL 语句和运行结果如下。

结果如下。

```sql
mysql> SELECT name FROM tb_students_info
    -> WHERE course_id = 
    ->(SELECT id FROM tb_course WHERE course_name = 'Python');
+------+
| name |
+------+
| Jane |
+------+
1 row in set (0.00 sec)
```

结果显示，学习 Python 课程的学生只有 Jane。

例 4

使用`<>`运算符，在 tb_course 表和 tb_students_info 表中查询出没有学习 Python 课程的学生姓名，SQL 语句和运行结果如下。

```sql
mysql> SELECT name FROM tb_students_info
    -> WHERE course_id <> 
    ->(SELECT id FROM tb_course WHERE course_name = 'Python');
+--------+
| name   |
+--------+
| Dany   |
| Green  |
| Henry  |
| Jim    |
| John   |
| Lily   |
| Susan  |
| Thomas |
| Tom    |
| LiMing |
+--------+
10 rows in set (0.00 sec)
```

可以看出，运行结果与例 3 刚好相反，没有学习 Python 课程的是除了 Jane 之外的学生。

例 5

查询 tb_course 表中是否存在 id=1 的课程，如果存在，就查询出 tb_students_info 表中的记录，SQL 语句和运行结果如下。

```sql
mysql> SELECT * FROM tb_students_info
    -> WHERE EXISTS
    ->(SELECT course_name FROM tb_course WHERE id=1);
+----+--------+------+------+--------+-----------+
| id | name   | age  | sex  | height | course_id |
+----+--------+------+------+--------+-----------+
|  1 | Dany   |   25 | 男   |    160 |         1 |
|  2 | Green  |   23 | 男   |    158 |         2 |
|  3 | Henry  |   23 | 女   |    185 |         1 |
|  4 | Jane   |   22 | 男   |    162 |         3 |
|  5 | Jim    |   24 | 女   |    175 |         2 |
|  6 | John   |   21 | 女   |    172 |         4 |
|  7 | Lily   |   22 | 男   |    165 |         4 |
|  8 | Susan  |   23 | 男   |    170 |         5 |
|  9 | Thomas |   22 | 女   |    178 |         5 |
| 10 | Tom    |   23 | 女   |    165 |         5 |
| 11 | LiMing |   22 | 男   |    180 |         7 |
+----+--------+------+------+--------+-----------+
11 rows in set (0.01 sec)
```

由结果可以看到，tb_course 表中存在 id=1 的记录，因此 EXISTS 表达式返回 TRUE，外层查询语句接收 TRUE 之后对表 tb_students_info 进行查询，返回所有的记录。

EXISTS 关键字可以和其它查询条件一起使用，条件表达式与 EXISTS 关键字之间用 AND 和 OR 连接。

例 6

查询 tb_course 表中是否存在 id=1 的课程，如果存在，就查询出 tb_students_info 表中 age 字段大于 24 的记录，SQL 语句和运行结果如下。

```sql
mysql> SELECT * FROM tb_students_info
    -> WHERE age>24 AND 
    ->EXISTS(SELECT course_name FROM tb_course WHERE id=1);
+----+------+------+------+--------+-----------+
| id | name | age  | sex  | height | course_id |
+----+------+------+------+--------+-----------+
|  1 | Dany |   25 | 男   |    160 |         1 |
+----+------+------+------+--------+-----------+
1 row in set (0.01 sec)
```

结果显示，从 tb_students_info 表中查询出了一条记录，这条记录的 age 字段取值为 25。内层查询语句从 tb_course 表中查询到记录，返回 TRUE。外层查询语句开始进行查询。根据查询条件，从 tb_students_info 表中查询 age 大于 24 的记录。

在完成较复杂的数据查询时，经常会使用到子查询，编写子查询语句时，要注意如下事项。

## 子查询语句可以嵌套在 SQL 语句中任何表达式出现的位置

在 SELECT 语句中，子查询可以被嵌套在 SELECT 语句的列、表和查询条件中，即 `SELECT` 子句，`FROM` 子句、`WHERE` 子句、`GROUP BY` 子句和 `HAVING` 子句。

前面已经介绍了 WHERE 子句中嵌套子查询的使用方法，下面是子查询在 SELECT 子句和 FROM 子句中的使用语法。

嵌套在 SELECT 语句的 SELECT 子句中的子查询语法格式如下。

```sql
SELECT (子查询) FROM 表名;
```



提示：子查询结果为单行单列，但不必指定列别名。

嵌套在 SELECT 语句的 FROM 子句中的子查询语法格式如下。

```sql
SELECT * FROM (子查询) AS 表的别名;
```



注意：必须为表指定别名。一般返回多行多列数据记录，可以当作一张临时表。

### 只出现在子查询中而没有出现在父查询中的表不能包含在输出列中

多层嵌套子查询的最终数据集只包含父查询（即最外层的查询）的 SELECT 子句中出现的字段，而子查询的输出结果通常会作为其外层子查询数据源或用于数据判断匹配。

常见错误如下：

```sql
SELECT * FROM (SELECT * FROM result);
```



这个子查询语句产生语法错误的原因在于主查询语句的 FROM 子句是一个子查询语句，因此应该为子查询结果集指定别名。正确代码如下。

```sql
SELECT * FROM (SELECT * FROM result) AS Temp;
```

