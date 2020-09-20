# WHERE语句

## WHERE：条件查询数据

在 MySQL 中，如果需要有条件的从数据表中查询数据，可以使用 `WHERE`关键字来指定查询条件。

使用 WHERE 关键字的语法格式如下：

1

```sql
WHERE 查询条件
```

查询条件可以是：

- 带比较运算符和逻辑运算符的查询条件
- 带 `BETWEEN AND`关键字的查询条件
- 带`IS NULL`关键字的查询条件
- 带 `IN`关键字的查询条件
- 带 `LIKE`关键字的查询条件

## 单一条件的查询语句

单一条件指的是在 WHERE 关键字后只有一个查询条件。

在 tb_students_info 数据表中查询身高为 170cm 的学生姓名，SQL 语句和运行结果如下。

1

```sql
mysql> SELECT name,height FROM tb_students_info
```

2

```sql
    -> WHERE height=170;
```

## 多条件的查询语句

在 WHERE 关键词后可以有多个查询条件，这样能够使查询结果更加精确。多个查询条件时用逻辑运算符 ` AND（&&）`、`OR（||）`或 ` XOR` 隔开。

- AND：记录满足所有查询条件时，才会被查询出来。
- OR：记录满足任意一个查询条件时，才会被查询出来。
- XOR：记录满足其中一个条件，并且不满足另一个条件时，才会被查询出来。

在 tb_students_info 表中查询 age 大于 21，并且 height 大于等于 175 的学生信息，SQL 语句和运行结果如下。

1

```sql
mysql> SELECT name,age,height FROM tb_students_info 
	-> WHERE age>21 AND height>=175;
```



**注意事项**：

- 同时使用 `order by` 和 `where` 子句时，应该让  `order by` 位于`where`  之后。