# RIGHT JOIN语句

### 右连接

右外连接又称为右连接，右连接是左连接的反向连接。使用 **`RIGHT OUTER JOIN`** 关键字连接两个表，并使用 `ON `子句来设置连接条件。

右连接的语法格式如下：

```sql
SELECT <字段名> FROM <表1> RIGHT OUTER JOIN <表2> <ON子句>
```



语法说明如下。

- 字段名：需要查询的字段名称。
- <表1><表2>：需要右连接的表名。
- `RIGHT OUTER JOIN`：右连接中可以省略 OUTER 关键字，只使用关键字 `RIGHT JOIN`。
- ON 子句：用来设置右连接的连接条件，不能省略。


与左连接相反，右连接以“表2”为基表，“表1”为参考表。右连接查询时，可以查询出“表2”中的所有记录和“表1”中匹配连接条件的记录。**如果“表2”的某行在“表1”中没有匹配行，那么在返回结果中，“表1”的字段值均为空值（NULL）**。

“右连接”，表1右连接表2，以右为主，表示以表2为主，关联查询表1的数据，查出表2所有数据以及表1和表2有交集的数据

<img src=".\img\image-20200816213915138.png" alt="image-20200816213915138" style="zoom:80%;" />

从score表中找出，没有在subjects表中出现过的记录

<img src=".\img\image-20200816213958798.png" alt="image-20200816213958798" style="zoom:80%;" />

例 2

在 tb_students_info 表和 tb_course 表中查询所有课程，包括没有学生的课程，SQL 语句和运行结果如下。

```sql
mysql> SELECT s.name,c.course_name FROM tb_students_info s RIGHT OUTER JOIN tb_course c 
    -> ON s.`course_id`=c.`id`;
+--------+-------------+
| name   | course_name |
+--------+-------------+
| Dany   | Java        |
| Green  | MySQL       |
| Henry  | Java        |
| Jane   | Python      |
| Jim    | MySQL       |
| John   | Go          |
| Lily   | Go          |
| Susan  | C++         |
| Thomas | C++         |
| Tom    | C++         |
| NULL   | HTML        |
+--------+-------------+
11 rows in set (0.00 sec)
```

可以看到，结果显示了 11 条记录，名称为 HTML 的课程目前没有学生，因为对应的 tb_students_info 表中并没有该学生的信息，所以该条记录只取出了 tb_course 表中相应的值，而从 tb_students_info 表中取出的值为 NULL。

> 多个表左/右连接时，在 ON 子句后连续使用 LEFT/RIGHT OUTER JOIN 或 LEFT/RIGHT JOIN 即可。

使用外连接查询时，一定要分清需要查询的结果，是需要显示左表的全部记录还是右表的全部记录，然后选择相应的左连接和右连接。