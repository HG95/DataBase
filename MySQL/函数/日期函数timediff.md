# 日期函数timediff

## TIMEDIFF函数介绍

`TIMEDIFF`返回两个[TIME](http://www.yiibai.com/mysql/time.html)或[DATETIME](http://www.yiibai.com/mysql/datetime.html)值之间的差值。 请参阅`TIMEDIFF`函数的以下语法。

```sql
TIMEDIFF(dt1, dt2);
```

`TIMEDIFF`函数接受两个必须为相同类型的参数，即`TIME`或`DATETIME`。 `TIMEDIFF`函数返回表示为时间值的`dt1 - dt2`的结果。

因为`TIMEDIFF`函数返回`TIME`值，所以其结果被限制在从`-838:59:59`到`838:59:59`的`TIME`值范围内。

请注意，`TIMEDIFF`函数接受`TIME`或`DATETIME`类型的值。要比较两个`DATE`或`DATETIME`值之间的差异，可以使用[DATEDIFF](http://www.yiibai.com/mysql/datediff.html)函数。

## TIMEDIFF函数示例

让我们举一个例子来计算两个时间值之间的差异。

```sql
mysql> SELECT TIMEDIFF('12:00:00','10:00:00') diff;
+----------+
| diff     |
+----------+
| 02:00:00 |
+----------+
1 row in set
SQL
```

在这个例子中，我们计算了`12:00:00`和`10:00:00`之间的差值为：`02:00:00`。

以下示例计算两个`DATETIME`值之间的差异值：

```sql
mysql> SELECT TIMEDIFF('2010-01-01 01:00:00', '2010-01-02 01:00:00') diff;
+-----------+
| diff      |
+-----------+
| -24:00:00 |
+-----------+
1 row in set
SQL
```

如果任一参数为`NULL`，`TIMEDIFF`函数将返回`NULL`。

```sql
mysql> SELECT TIMEDIFF('2010-01-01',NULL) diff;
+------+
| diff |
+------+
| NULL |
+------+
1 row in set, 1 warning (0.00 sec)
SQL
```

如果传递两个不同类型的参数，一个是`DATETIME`，另一个是`TIME`，`TIMEDIFF`函数也返回`NULL`。

```sql
mysql> SELECT TIMEDIFF('2010-01-01 10:00:00','10:00:00') diff;
+------+
| diff |
+------+
| NULL |
+------+
1 row in set
SQL
```

## TIMEDIFF函数和截断的不正确的时间值

请考虑以下示例：

```sql
mysql> SELECT TIMEDIFF('2009-03-01 00:00:00', '2009-01-01 00:00:00') diff;
+-----------+
| diff      |
+-----------+
| 838:59:59 |
+-----------+
1 row in set, 1 warning (0.00 sec)
SQL
```

可以看到，有一个警告。下面来看看看使用`SHOW WARNINGS`语句是什么。

```sql
mysql> SHOW WARNINGS;
+---------+------+----------------------------------------------+
| Level   | Code | Message                                      |
+---------+------+----------------------------------------------+
| Warning | 1292 | Truncated incorrect time value: '1416:00:00' |
+---------+------+----------------------------------------------+
1 row in set
SQL
```

所以结果应该是`1416`小时，但是如前所述，`TIMEDIFF`函数的结果是一个`TIME`值，范围是从`-838:59:59`到`838:59:59`。 因此，MySQL会截断结果。

要解决此问题，您需要使用`TIMESTAMPDIFF`函数，如下所示：

```sql
mysql> SELECT TIMESTAMPDIFF(HOUR, '2018-01-01 00:00:00', '2018-03-01 00:00:00') diff;
+------+
| diff |
+------+
| 1416 |
+------+
1 row in set
```

