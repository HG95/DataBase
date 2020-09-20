# LAG、LEAD函数

## AG() 函数

`LAG()`函数是一个[窗口函数](https://www.begtut.com/mysql/mysql-window-functions.html)，返回分区中当前行之前的第N行的值。 如果不存在前一行，则返回NULL。

```sql
LAG(<expression>[,offset[, default_value]]) OVER (
    PARTITION BY expr,...
    ORDER BY expr [ASC|DESC],...
) 
```

- `expression`

`LAG()`函数返回`expression`当前行之前的行的值，其值为`offset` 其分区或结果集中的行数。

- `offset`

`offset`是从当前行返回的行数，以获取值。`offset`必须是零或文字正整数。如果`offset`为零，则`LAG()`函数计算`expression`当前行的值。如果未指定`offset`，则`LAG()`默认情况下函数使用一个。

- `default_value`

如果没有前一行，则`LAG()`函数返回`default_value`。例如，如果offset为2，则第一行的返回值为`default_value`。如果省略`default_value`，则默认`LAG()`返回函数`NULL`。

- `PARTITION BY` 子句

`PARTITION BY`子句将结果集中的行划分`LAG()`为应用函数的分区。如果省略`PARTITION BY`子句，`LAG()`函数会将整个结果集视为单个分区。

- `ORDER BY` 子句

`ORDER BY`子句指定在`LAG()`应用函数之前每个分区中的行的顺序。

`LAG()`函数可用于计算当前行和上一行之间的差异。

### LAG() 函数示例

返回特定年份和上一年度中每个产品系列的订单值：

```sql
WITH productline_sales AS (
    SELECT productline,
           YEAR(orderDate) order_year,
           ROUND(SUM(quantityOrdered * priceEach),0) order_value
    FROM orders
    INNER JOIN orderdetails USING (orderNumber)
    INNER JOIN products USING (productCode)
    GROUP BY productline, order_year
)
SELECT
    productline, 
    order_year, 
    order_value,
    LAG(order_value, 1) OVER (
        PARTITION BY productLine
        ORDER BY order_year
    ) prev_year_order_value
FROM 
    productline_sales; 
```

输出

```
+------------------+------------+-------------+-----------------------+
| productline      | order_year | order_value | prev_year_order_value |
+------------------+------------+-------------+-----------------------+
| Classic Cars     |       2013 |     1374832 |                  NULL |
| Classic Cars     |       2014 |     1763137 |               1374832 |
| Classic Cars     |       2015 |      715954 |               1763137 |
| Motorcycles      |       2013 |      348909 |                  NULL |
| Motorcycles      |       2014 |      527244 |                348909 |
| Motorcycles      |       2015 |      245273 |                527244 |
| Planes           |       2013 |      309784 |                  NULL |
| Planes           |       2014 |      471971 |                309784 |
| Planes           |       2015 |      172882 |                471971 |
| Ships            |       2013 |      222182 |                  NULL |
| Ships            |       2014 |      337326 |                222182 |
| Ships            |       2015 |      104490 |                337326 |
| Trains           |       2013 |       65822 |                  NULL |
| Trains           |       2014 |       96286 |                 65822 |
| Trains           |       2015 |       26425 |                 96286 |
| Trucks and Buses |       2013 |      376657 |                  NULL |
| Trucks and Buses |       2014 |      465390 |                376657 |
| Trucks and Buses |       2015 |      182066 |                465390 |
| Vintage Cars     |       2013 |      619161 |                  NULL |
| Vintage Cars     |       2014 |      854552 |                619161 |
| Vintage Cars     |       2015 |      323846 |                854552 |
+------------------+------------+-------------+-----------------------+
21 rows in set (0.04 sec)
```

##  LEAD 函数

###  LEAD() 函数概述

`LEAD()`函数是一个[窗口函数](https://www.begtut.com/mysql/mysql-window-functions.html)，返回分区中当前行之后的第N行的值。 如果不存在后续行，则返回NULL。

与`LAG()`函数类似，`LEAD()`函数对于计算同一结果集中当前行和后续行之间的差异非常有用。

```sql
LEAD(<expression>[,offset[, default_value]]) OVER (
    PARTITION BY (expr)
    ORDER BY (expr)
) 
```

- `expression`

`LEAD()`函数返回的值`expression`从`offset-th`有序分区排。

- `offset`

`offset`是从当前行向前行的行数，以获取值。

`offset`必须是一个非负整数。如果`offset`为零，则`LEAD()`函数计算`expression`当前行的值。

如果省略 `offset`，则`LEAD()`函数默认使用一个。

- `default_value`

如果没有后续行，则`LEAD()`函数返回`default_value`。例如，如果`offset`是1，则最后一行的返回值为`default_value`。

如果您未指定`default_value`，则函数返回 `NULL` 。

- `PARTITION BY` 子句

`PARTITION BY`子句将结果集中的行划分`LEAD()`为应用函数的分区。

如果`PARTITION BY`未指定子句，则结果集中的所有行都将被视为单个分区。

- `ORDER BY`子句

`ORDER BY`子句确定`LEAD()`应用函数之前分区中行的顺序。

### LEAD() 函数示例

查找每个客户的订单日期和下一个订单日期：

```sql
SELECT 
    customerName,
    orderDate,
    LEAD(orderDate,1) OVER (
        PARTITION BY customerNumber
        ORDER BY orderDate ) nextOrderDate
FROM 
    orders
INNER JOIN customers USING (customerNumber); 
```

输出：

```
+------------------------------------+------------+---------------+
| customerName                       | orderDate  | nextOrderDate |
+------------------------------------+------------+---------------+
| Atelier graphique                  | 2013-05-20 | 2014-09-27    |
| Atelier graphique                  | 2014-09-27 | 2014-11-25    |
| Atelier graphique                  | 2014-11-25 | NULL          |
| Signal Gift Stores                 | 2013-05-21 | 2014-08-06    |
| Signal Gift Stores                 | 2014-08-06 | 2014-11-29    |
| Signal Gift Stores                 | 2014-11-29 | NULL          |
| Australian Collectors, Co.         | 2013-04-29 | 2013-05-21    |
| Australian Collectors, Co.         | 2013-05-21 | 2014-02-20    |
| Australian Collectors, Co.         | 2014-02-20 | 2014-11-24    |
| Australian Collectors, Co.         | 2014-11-24 | 2014-11-29    |
| Australian Collectors, Co.         | 2014-11-29 | NULL          |
| La Rochelle Gifts                  | 2014-07-23 | 2014-10-29    |
| La Rochelle Gifts                  | 2014-10-29 | 2015-02-03    |
| La Rochelle Gifts                  | 2015-02-03 | 2015-05-31    |
| La Rochelle Gifts                  | 2015-05-31 | NULL          |
| Baane Mini Imports                 | 2013-01-29 | 2013-10-10    |
| Baane Mini Imports                 | 2013-10-10 | 2014-10-15    |
...
```



## 参考

- <a href="https://www.begtut.com/mysql/mysql-lead-function.html" target="_blank">MySQL LEAD 函数</a>
- <a href="https://www.begtut.com/mysql/mysql-lag-function.html" target="_blank">MySQL LAG() 函数</a> 

