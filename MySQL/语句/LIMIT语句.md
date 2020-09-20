# LIMIT语句

## LIMIT: 限制查询结果的条数

限制查询结果的条数

SQL 标准中对查询结果进行限制的`OFFSET`子句和`FETCH`子句

```sql
[LIMIT {[offset,] row_count | row_count OFFSET offset}]
```

## 指定初始位置

> LIMIT 初始位置，记录数

其中，“初始位置”表示从哪条记录开始显示；“记录数”表示显示记录的条数。**第一条记录的位置是 0**，第二条记录的位置是 1。后面的记录依次类推。

## 不指定初始位置

> LIMIT 记录数

其中，“记录数”表示显示记录的条数。如果“记录数”的值小于查询结果的总数，则会从第一条记录开始，显示指定条数的记录。如果“记录数”的值大于查询结果的总数，则会直接显示查询出来的所有记录。

限制查询结果 (`LIMIT`) 默认情况下，返回所有查找到的结果

如果 `LIMIT` 后面只有一个数字，表示从第一条开始返回，并返回相应数字个数的记录

```sql
select * from usres limit 2;
```

从第一条开始，返回两条记录。

##  `LIMIT` 和 `OFFSET`组合使用

>  LIMIT 记录数 OFFSET 初始位置

参数和 LIMIT 语法中参数含义相同，“初始位置”指定从哪条记录开始显示；“记录数”表示显示记录的条数。

`SELECT` 语句默认从0开始编号，如果想从第三条开始返回，则需要 offset 参数和 row_count 参数一起使用

```sql
select * from usres limit 2,2;

SELECT first_name, last_name, salary
  FROM employees
 ORDER BY salary DESC
 LIMIT 5, 10; -- return from 5th to 14th
 -- LIMIT 10 OFFSET 5;
```

## Top-N 查询

由于不同数据库的实现存在较大差异, 先以 Oracle 12c 语法为例

```sql
SELECT first_name, last_name, salary
	FROM employees
	ORDER BY salary DESC
	FETCH FIRST 10 ROWS ONLY;
```

以上查询返回薪水最高的前 10 位员工。首先，`ORDER BY`子句定义了按照薪水从高到低排序；然后`FETCH`子句指定了只返回前 10 条记录。

## 分页查询

考虑另一个场景，假如应用提供了分页显示的功能，每页显示 10 条记录，点击下一页时，需要显示第 11 到第 20 条记录。

```sql
SELECT first_name, last_name, salary
  FROM employees
 ORDER BY salary DESC
OFFSET 10 ROWS
 FETCH FIRST 10 ROWS ONLY;
```





SQL 标准中的完整定义：

```sql
SELECT column1, column2, ...
  FROM table
[WHERE conditions]
[ORDER BY column1 ASC, column2 DESC, ...]
[OFFSET m {ROW | ROWS}]
[FETCH { FIRST | NEXT } [ num_rows | n PERCENT ] { ROW | ROWS } { ONLY | WITH TIES }];

```

其中，`OFFSET` 表示偏移量，即从第 m+1 行开始返回；如果不指定，从第 1 行开始返回。

`FETCH` 用于指定返回多少行，`FIRST` 和`NEXT` 等价；num_rows 表示行数，n PERCENT 表示即按照百分比指定行数，`ROW` 和`ROWS` 等价；`ONLY` 和`WITH TIES` 的差别在于，如果最后存在更多排名相同的数据行，`WITH TIES`会返回更多的数据。

以下查询按照百分比返回前10%的数据：

```sql
SELECT first_name, last_name, salary
	FROM employees
	ORDER BY salary DESC
	FETCH FIRST 10 PERCENT ROWS ONLY;
```

