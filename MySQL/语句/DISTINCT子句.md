# DISTINCT子句

## DISTINCT 去重

**使用DISTINCT过滤重复数据**

`DISTINCT` 关键字的主要作用就是对数据表中一个或多个字段重复的数据进行过滤，只返回其中的一条数据给用户。

```sql
SELECT DISTINCT <字段名> FROM <表名>;
```

其中，“字段名”为需要消除重复记录的字段名称，多个字段时用逗号隔开。

使用 DISTINCT 关键字时需要注意以下几点：

- `DISTINCT` 关键字只能在 `SELECT` 语句中使用。
- 在对一个或多个字段去重时，`DISTINCT` 关键字必须在所有字段的最前面。
- 如果 `DISTINCT` 关键字后有多个字段，则会对多个字段进行组合去重，也就是说，只有多个字段组合起来完全是一样的情况下才会被去重。

在 SQL 中，提供了`DISTINCT`关键字，用于删除查询结果中的重复值。例如：

```sql
SELECT DISTINCT
		first_name
	FROM employees;
```

`DISTINCT`位于`SELECT`之后，可以基于多个列值进行查重操作，通用语法如下：

```sql
SELECT DISTINCT
		column1,
		column2,
		...
	FROM table;
```

为了消除重复值，数据库系统需要对结果进行排序，然后扫描重复值；因此，大量数据的重复值处理会降低查询的速度。

除了`DISTINCT`之外，另一个关键字是`ALL`，它不会排除重复的结果，而是显示所有数据：

```sql
SELECT [ALL | DISTINCT]
		column1,
		column2,
		...
	FROM table;
```

如果不指定，默认值为`ALL`。



参考

- <a href="https://www.cnblogs.com/rainman/archive/2013/05/03/3058451.html" target="">SQL中distinct的用法</a> 