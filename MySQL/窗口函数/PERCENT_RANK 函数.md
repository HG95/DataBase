# PERCENT_RANK 函数

## PERCENT_RANK 函数

这`PERCENT_RANK()`是一个[窗口函数](https://www.begtut.com/mysql/mysql-window-functions.html)，用于计算分区或结果集中行的[百分位数](https://en.wikipedia.org/wiki/Percentile_rank)。

以下显示了`PERCENT_RANK()`函数的语法：

```sql
PERCENT_RANK()
    OVER (
        PARTITION BY expr,...
        ORDER BY expr [ASC|DESC],...
    ) 
```

`PERCENT_RANK()`函数返回一个从0到1的数字。

对于指定的行，`PERCENT_RANK()`计算行的[等级](https://www.begtut.com/mysql/mysql-window-functions/mysql-rank-function/)减1，除以评估的分区或查询结果集中的行数减1：

```sql
(rank - 1) / (total_rows - 1) 
```

在此公式中，`rank`是指定行的等级，`total_rows`是要计算的行数。

`PERCENT_RANK()`对于分区或结果集中的第一行，函数始终返回零。重复的列值将接收相同的`PERCENT_RANK()`值。

与其他窗口函数类似，`PARTITION BY`子句将行分配到分区中，`ORDER BY`子句指定每个分区中行的逻辑顺序。`PERCENT_RANK()`为每个有序分区独立计算函数。

两个`PARTITION BY`和`ORDER BY`子句都是可选项。但是，它`PERCENT_RANK()`是一个顺序敏感函数，因此，您应始终使用`ORDER BY`子句。

## PERCENT_RANK() 函数示例

创建一个名为新表`productLineSales`基础上的`orders`，`orderDetails`以及`products`从表中[示例数据库](https://www.begtut.com/mysql/mysql-sample-database.html)：

```sql
CREATE TABLE productLineSales
SELECT
    productLine,
    YEAR(orderDate) orderYear,
    quantityOrdered * priceEach orderValue
FROM
    orderDetails
        INNER JOIN
    orders USING (orderNumber)
        INNER JOIN
    products USING (productCode)
GROUP BY
    productLine ,
    YEAR(orderDate); 
```

`productLineSales`表存储销售数据的摘要，包括产品系列，订单年份和订单值。

```
+------------------+-----------+------------+
| productLine      | orderYear | orderValue |
+------------------+-----------+------------+
| Vintage Cars     |      2013 |    4080.00 |
| Classic Cars     |      2013 |    5571.80 |
| Trucks and Buses |      2013 |    3284.28 |
| Trains           |      2013 |    2770.95 |
| Ships            |      2013 |    5072.71 |
| Planes           |      2013 |    4825.44 |
| Motorcycles      |      2013 |    2440.50 |
| Classic Cars     |      2014 |    8124.98 |
| Vintage Cars     |      2014 |    2819.28 |
| Trains           |      2014 |    4646.88 |
| Ships            |      2014 |    4301.15 |
| Planes           |      2014 |    2857.35 |
| Motorcycles      |      2014 |    2598.77 |
| Trucks and Buses |      2014 |    4615.64 |
| Motorcycles      |      2015 |    4004.88 |
| Classic Cars     |      2015 |    5971.35 |
| Vintage Cars     |      2015 |    5346.50 |
| Trucks and Buses |      2015 |    6295.03 |
| Trains           |      2015 |    1603.20 |
| Ships            |      2015 |    3774.00 |
| Planes           |      2015 |    4018.00 |
+------------------+-----------+------------+
21 rows in set (0.02 sec)
```

## `PERCENT_RANK()`在查询结果集上使用

以下查询按订单值查找每个产品系列的百分位数排名：

```sql
WITH t AS (
    SELECT
        productLine,
        SUM(orderValue) orderValue
    FROM
        productLineSales
    GROUP BY
        productLine
)
SELECT
    productLine,
    orderValue,
    ROUND(
       PERCENT_RANK() OVER (
          ORDER BY orderValue
       )
    ,2) percentile_rank
FROM
    t; 
```

在这个例子中：

- 首先，我们使用公用表表达式按产品线汇总订单值。
- 其次，我们用它`PERCENT_RANK()`来计算每种产品的订单价值的百分等级。此外，我们使用`ROUND()`函数将值舍入为2十进制，以获得更好的表示。

这是输出：

```
+------------------+------------+-----------------+
| productLine      | orderValue | percentile_rank |
+------------------+------------+-----------------+
| Trains           |    9021.03 |            0.00 |
| Motorcycles      |    9044.15 |            0.17 |
| Planes           |   11700.79 |            0.33 |
| Vintage Cars     |   12245.78 |            0.50 |
| Ships            |   13147.86 |            0.67 |
| Trucks and Buses |   14194.95 |            0.83 |
| Classic Cars     |   19668.13 |            1.00 |
+------------------+------------+-----------------+
7 rows in set (0.01 sec)
```

以下是输出中的一些分析：

- 订单价值`Trains`并不比任何其他产品线更好，后者用零表示。
- `Vintage Cars` 表现优于50％的其他产品。
- `Classic Cars` 表现优于任何其他产品系列，因此其百分比等级为1或100％

## `PERCENT_RANK()`在分区上使用

以下语句按年度中的订单值返回产品系列的百分位数排名：

```sql
SELECT
    productLine,
    orderYear,
    orderValue,
    ROUND(
    PERCENT_RANK()
    OVER (
        PARTITION BY orderYear
        ORDER BY orderValue
    ),2) percentile_rank
FROM
    productLineSales; 
```

这是输出：

```sql
+------------------+-----------+------------+-----------------+
| productLine      | orderYear | orderValue | percentile_rank |
+------------------+-----------+------------+-----------------+
| Motorcycles      |      2013 |    2440.50 |            0.00 |
| Trains           |      2013 |    2770.95 |            0.17 |
| Trucks and Buses |      2013 |    3284.28 |            0.33 |
| Vintage Cars     |      2013 |    4080.00 |            0.50 |
| Planes           |      2013 |    4825.44 |            0.67 |
| Ships            |      2013 |    5072.71 |            0.83 |
| Classic Cars     |      2013 |    5571.80 |            1.00 |
| Motorcycles      |      2014 |    2598.77 |            0.00 |
| Vintage Cars     |      2014 |    2819.28 |            0.17 |
| Planes           |      2014 |    2857.35 |            0.33 |
| Ships            |      2014 |    4301.15 |            0.50 |
| Trucks and Buses |      2014 |    4615.64 |            0.67 |
| Trains           |      2014 |    4646.88 |            0.83 |
| Classic Cars     |      2014 |    8124.98 |            1.00 |
| Trains           |      2015 |    1603.20 |            0.00 |
| Ships            |      2015 |    3774.00 |            0.17 |
| Motorcycles      |      2015 |    4004.88 |            0.33 |
| Planes           |      2015 |    4018.00 |            0.50 |
| Vintage Cars     |      2015 |    5346.50 |            0.67 |
| Classic Cars     |      2015 |    5971.35 |            0.83 |
| Trucks and Buses |      2015 |    6295.03 |            1.00 |
+------------------+-----------+------------+-----------------+
21 rows in set (0.01 sec)
```

