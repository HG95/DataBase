# LAST_VALUE

## LAST_VALUE() 函数

`LAST_VALUE()`函数是一个[窗口函数](https://www.begtut.com/mysql/mysql-window-functions.html)，允许您选择有序行集中的最后一行。

以下显示了`LAST_VALUE()`函数的语法：

```sql
LAST_VALUE (expression) OVER (
    [partition_clause]
    [order_clause]
    [frame_clause]
) 
```

`LAST_VALUE()`函数返回`expression`有序行集的最后一行的值。

`OVER`有三个子句：`partition_clause`，`order_clause`，和`frame_clause`。

### `partition_clause`

`partition_clause`语法如下：

```sql
PARTITION BY expr1, expr2, ... 
```

`PARTITION BY`子句分配结果集成由一个或多个表达式指定多个分区`expr1`，`expr2`等`LAST_VALUE()`函数被独立地施加到每个分区。

### `order_clause`

`order_clause`语法如下：

```sql
ORDER BY  expr1 [ASC|DESC],... 
```

`ORDER BY`子句指定`LAST_VALUE()`函数运行的分区中行的逻辑顺序。

###  `frame_clause`

`frame_clause`定义了所述当前分区的所述子集`LAST_VALUE()`函数应用。

## LAST_VALUE() 函数示例

使用的数据仍然是上面建立的 table

#### 1）MySQL `LAST_VALUE()`在整个查询结果示例中

```sql
select employee_name,
       hours,
       last_value(employee_name) over (
           order by hours
           range between
               unbounded preceding and
               unbounded following
           ) highest_overtime_employee
from overtime;
```

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200604142527.png"/></center>

在此示例中，`ORDER BY`子句将结果集中行的逻辑顺序指定为从低到高的小时。

默认帧规范如下：

```sql
RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW 
```

这意味着框架从第一行开始，到结果集的当前行结束。

因此，为了获得加班时间最长的员工，我们将框架规格更改为以下内容：

```sql
RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING 
```

这表示框架从第一行开始，到结果集的最后一行结束。

#### 2）MySQL `LAST_VALUE()`上的分区示例

以下语句查找每个部门加班时间最长的员工：

```sql
select employee_name,
       department,
       hours,
       last_value(employee_name) over (
           partition by department
           order by hours
           range between
               unbounded preceding and
               unbounded following

           ) most_overtime_employee
from overtime;
```

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200604143033.png"/></center>

在这个例子中，首先，`PARTITION BY`子句按部门划分了员工。然后，`ORDER BY`子句通过加班从低到高命令每个部门的员工。

在这种情况下，帧规范是整个分区。结果，`LAST_VALUE()`函数选择了每个分区中的最后一行，分区是加班时间最高的员工。

