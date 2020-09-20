# DELETE语句

## DELETE：删除数据

## 删除单个表中的数据

使用 `DELETE` 语句从单个表中删除数据，语法格式为：

```sql
DELETE FROM <表名> [WHERE 子句] [ORDER BY 子句] [LIMIT 子句]
```

语法说明如下：

- `<表名>`：指定要删除数据的表名。
- `ORDER BY` 子句：可选项。表示删除时，表中各行将按照子句中指定的顺序进行删除。
- `WHERE` 子句：可选项。表示为删除操作限定删除条件，若省略该子句，则代表删除该表中的所有行。
- `LIMIT` 子句：可选项。用于告知服务器在控制命令被返回到客户端前被删除行的最大值。


注意：在不使用 WHERE 条件的时候，将删除所有数据。

## 删除表中的全部数据

【实例 1】删除 tb_courses_new 表中的全部数据，输入的 SQL 语句和执行结果如下所示。

```sql
mysql> DELETE FROM tb_courses_new;
Query OK, 3 rows affected (0.12 sec)
mysql> SELECT * FROM tb_courses_new;
Empty set (0.00 sec)
```

## 根据条件删除表中的数据

【实例 2】在 tb_courses_new 表中，删除 course_id 为 4 的记录，输入的 SQL 语句和执行结果如下所示。

```sql
mysql> DELETE FROM tb_courses
    -> WHERE course_id=4;
Query OK, 1 row affected (0.00 sec)
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

由运行结果可以看出，course_id 为 4 的记录已经被删除。



## 案例

## 196. 删除重复的电子邮箱 [简单]

编写一个 SQL 查询，来删除 Person 表中所有重复的电子邮箱，重复的邮箱里只保留 Id 最小 的那个。


| Id | Email            |
| ---- | ---- |
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |

Id 是这个表的主键。
例如，在运行你的查询语句之后，上面的 Person 表应返回以下几行:

| Id | Email     |
| ---- | ---- |
| 1  | john@example.com |
| 2  | bob@example.com  |





提示：

执行 SQL 之后，输出是整个 Person 表。
使用 delete 语句。

### 解题

方法一

```sql
DELETE p1 FROM Person p1,
    Person p2
WHERE
    p1.Email = p2.Email AND p1.Id > p2.Id
```

分析：

1、`DELETE p1`

在 `DELETE`官方文档中，给出了这一用法，比如下面这个DELETE语句👇

```sql
DELETE t1 
FROM t1 
LEFT JOIN t2 
ON t1.id=t2.id 
WHERE t2.id IS NULL;
```



这种 `DELETE` 方式很陌生，竟然和 `SELETE` 的写法类似。它涉及到 t1 和 t2 两张表，`DELETE t1`表示要删除 `t1`的一些记录，具体删哪些，就看 `WHERE`条件，满足就删；这里删的是 t1 表中，跟 t2 匹配不上的那些记录。