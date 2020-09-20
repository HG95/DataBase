# FIRST_VALUE

## FIRST_VALUE() 函数概述

`FIRST_VALUE()`是一个[窗口函数](https://www.begtut.com/mysql/mysql-window-functions.html)，允许您选择窗口框架，分区或结果集的第一行。

以下说明了`FIRST_VALUE()`函数的语法：

```sql
FIRST_VALUE(expression) OVER (
        [partition_clause]
        [order_clause]
        [frame_clause]
) 
```

## 表达

`FIRST_VALUE()`函数返回`expression`窗口框架第一行的值。

`OVER`条款由三个条款：`partition_clause`，`order_clause`，和`frame_clause`。

`partition_clause`

`partition_clause`子句将结果集的行划分为函数独立应用的分区。`partition_clause`语法如下：

```sql
PARTITION BY expr1, expr2, ... 
```

`order_clause`

`order_clause`子句指定`FIRST_VALUE()`函数操作的每个分区中行的逻辑顺序。以下显示了以下语法`order_clause`：

```sql
ORDER BY expr1 [ASC|DESC], expr2 [ASC|DESC], ... 
```

`frame_clause`

`frame_clause`定义当前分区的子集（或帧）

## FIRST_VALUE() 函数示例

以下语句创建一个名为`overtime`的新表，并为演示插入示例数据：

```sql
CREATE TABLE overtime (
    employee_name VARCHAR(50) NOT NULL,
    department VARCHAR(50) NOT NULL,
    hours INT NOT NULL,
    PRIMARY KEY (employee_name , department)
);

INSERT INTO overtime(employee_name, department, hours)
VALUES('Diane Murphy','Accounting',37),
('Mary Patterson','Accounting',74),
('Jeff Firrelli','Accounting',40),
('William Patterson','Finance',58),
('Gerard Bondur','Finance',47),
('Anthony Bow','Finance',66),
('Leslie Jennings','IT',90),
('Leslie Thompson','IT',88),
('Julie Firrelli','Sales',81),
('Steve Patterson','Sales',29),
('Foon Yue Tseng','Sales',65),
('George Vanauf','Marketing',89),
('Loui Bondur','Marketing',49),
('Gerard Hernandez','Marketing',66),
('Pamela Castillo','SCM',96),
('Larry Bott','SCM',100),
('Barry Jones','SCM',65); 
```

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200604140927.png"/></center>

1）`FIRST_VALUE()`在整个查询结果集示例中使用MySQL 函数

以下语句获取员工姓名，加班时间和加班时间最少的员工：

```sql
select employee_name,
       hours,
       first_value(employee_name) over (
           order by hours
           ) least_over_time
from overtime;
```

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200604141257.png"/></center>

在此示例中，`ORDER BY`子句按结果集对行中的行进行了按小时排序，并`FIRST_VALUE()`选择了第一行，表示加班时间最短的员工。

2）`FIRST_VALUE()`在分区示例中使用MySQL

以下语句查找每个部门加班时间最少的员工：

```sql
select employee_name,
       department,
       hours,
       first_value(employee_name) over (
           partition by department
           order by hours
           )least_over_time
from overtime;
```

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200604141742.png"/></center>

在这个例子中：

- 首先，`PARTITION BY`条款将员工按部门划分为分区。换句话说，每个分区由属于同一部门的员工组成。
- 其次，`ORDER BY`子句指定了每个分区中的行顺序。
- 第三，`FIRST_VALUE()`按小时排序对每个分区进行操作。它返回了每个分区中的第一行，分区是部门内加班时间最少的员工。

