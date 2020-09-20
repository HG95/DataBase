# SELECT语句

## **SELECT：数据表查询语句**

```sql
SELECT EXPR,...FROM tbl_name

SELECT select_expr [,select_expr...]
[
FROM tbl_references
[WHERE where_condition]
[GROUP BY {col_name | position} [ASC | DESC],...]
[HAVING where_condition]
[ORDER BY {col_name | expo | position}  [ASC | DESC],...]
[LIMIT {[offset,] row_count | row_count OFFSET offset}]
]
```

其中，各条子句的含义如下：

- `{*|<字段列名>}`包含星号通配符的字段列表，表示所要查询字段的名称。
- `<表 1>，<表 2>…`，表 1 和表 2 表示查询数据的来源，可以是单个或多个。
- `WHERE <表达式>`是可选项，如果选择该项，将限定查询数据必须满足该查询条件。
- `GROUP BY< 字段 >`，该子句告诉 MySQL 如何显示查询出来的数据，并按照指定的字段分组。
- `[ORDER BY< 字段 >]`，该子句告诉 MySQL 按什么样的顺序显示查询出来的数据，可以进行的排序有升序（ASC）和降序（DESC），默认情况下是升序。
- `[LIMIT[，]]`，该子句告诉 MySQL 每次显示查询出来的数据条数。

查询表达式的每个表达式表示想要查找的一列，必须有至少一个。多个列之间以英文逗号分开， 查询 id, username 两列。

```sql
select id, username from users;
```

在使用多表连接时，可能会出现不同的表中存在名称相同的字段，如果直接写字段，分不清到底是哪张数据表的字段。在字段名前加上数据表可以分辨出隶属于哪张数据表。

```sql
select users,id, users, username from users;
```

星号`*`号表示所有的列。`tbl_name.*` 可以表示命名表的所有列 .

```sql
select * from users;
```

查询表达式可以使用 [AS] alias_name 为其赋予别名，别名可用于 `GROUP BY`, `ORDER BY`, `HAVING` 字句 .

```sql
select id AS userId, username AS uname from users;
```

