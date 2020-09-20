# 日期函数timestampdiff

`TIMESTAMPDIFF`函数的语法。

```sql
TIMESTAMPDIFF(unit,begin,end);
```

`TIMESTAMPDIFF`函数返回`begin-end`的结果，其中`begin`和`end`是[DATE](http://www.yiibai.com/mysql/date.html)或[DATETIME](http://www.yiibai.com/mysql/datetime.html)表达式。

`TIMESTAMPDIFF`函数允许其参数具有混合类型，例如，`begin`是`DATE`值，`end`可以是`DATETIME`值。 如果使用`DATE`值，则`TIMESTAMPDIFF`函数将其视为时间部分为`“00:00:00”`的`DATETIME`值。

`unit`参数是确定(`end-begin`)的结果的单位，表示为整数。 以下是有效单位：

- MICROSECOND
- SECOND
- MINUTE
- HOUR
- DAY
- WEEK
- MONTH
- QUARTER
- YEAR

## TIMESTAMPDIFF函数示例

以下示例将以月份值的形式返回`2018-01-01`和`2018-06-01`的差值：

```sql
mysql> SELECT TIMESTAMPDIFF(MONTH, '2018-01-01', '2018-06-01') result;
+--------+
| result |
+--------+
|      5 |
+--------+
1 row in set
```

如果您希望看到差值，只需要将`unit`参数从`MONTH`更改为`DAY`，如下所示：

```sql
mysql> SELECT TIMESTAMPDIFF(DAY, '2010-01-01', '2010-06-01') result;
+--------+
| result |
+--------+
|    151 |
+--------+
1 row in set

```

以下语句返回两个`DATETIME`值(以分钟为单位)的差异值：

```sql
mysql> SELECT TIMESTAMPDIFF(MINUTE, '2018-01-01 10:00:00', '2018-01-01 10:45:00') result;
+--------+
| result |
+--------+
|     45 |
+--------+
1 row in set

```

## TIMESTAMPDIFF函数计算年龄

```sql
CREATE TABLE persons
(
    id            INT AUTO_INCREMENT PRIMARY KEY,
    full_name     VARCHAR(255) NOT NULL,
    date_of_birth DATE         NOT NULL
);
INSERT INTO persons(full_name, date_of_birth)
VALUES ('John Doe', '1990-01-01'),
       ('David Taylor', '1989-06-06'),
       ('Peter Drucker', '1985-03-02'),
       ('Lily Minsu', '1992-05-05'),
       ('Mary William', '1995-12-01');
```

使用`TIMESTAMPDIFF`来计算`persons`表中每个人的年龄：

```sql
select full_name,
       date_of_birth,
       timestampdiff(YEAR,date_of_birth,now()) 年龄
from persons;
```

<img src=".\img\image-20200904104606673.png" alt="image-20200904104606673" style="zoom:80%;" />

我们计算到今天的年龄，所以今天对应的就是 `end`; 



开始时间和结束时间的顺序不同，可能会产生负值；

```sql
select full_name,
       date_of_birth,
       timestampdiff(YEAR,now(),date_of_birth) 年龄
from persons;
```

<img src=".\img\image-20200904104814443.png" alt="image-20200904104814443" style="zoom:80%;" />

