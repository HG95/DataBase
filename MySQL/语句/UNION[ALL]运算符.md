# UNION[ALL]运算符

## UNION 运算符

`UNION`运算符用于组合两个或多个 `SELECT`语句的结果集。

- `UNION`中的每个SELECT语句必须具有相同的列数
- 列还必须具有类似的数据类型
- 每个SELECT语句中的列也必须具有相同的顺序, 列名必须相同。
-  `UNION` 本身是去重的。

```sql
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2; 
```

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200819090906.png" alt="image-20200819090901857" style="zoom:80%;" />





## UNION ALL语法

`UNION`运算符默认情况下仅选择不同的值。要允许重复值，请使用`UNION ALL`：

```sql
SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2; 
```

**注意：**结果集中的列名通常等于UNION中`第一个`SELECT语句中的列名。

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200819091235.png" alt="image-20200819091021673" style="zoom:80%;" />



## UNION与WHERE

以下SQL语句从“Customers”和“Suppliers”表中返回 Country = 'Germany'（仅限不同的值）：

```sql
SELECT City, Country FROM Customers
WHERE Country='Germany'
UNION
SELECT City, Country FROM Suppliers
WHERE Country='Germany'
ORDER BY City; 
```

## UNION ALL使用WHERE

以下SQL语句从“Customers”和“Suppliers”表中返回 Country = 'Germany'（也是重复值）：

```sql
SELECT City, Country FROM Customers
WHERE Country='Germany'
UNION ALL
SELECT City, Country FROM Suppliers
WHERE Country='Germany'
ORDER BY City; 
```

