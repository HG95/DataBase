# NTH_VALUE函数

## NTH_VALUE 函数

这`NTH_VALUE()`是一个[窗口函数](https://www.begtut.com/mysql/mysql-window-functions.html)，允许您从有序行集中的第N行获取值。

以下显示了`NTH_VALUE()`函数的语法：

```sql
NTH_VALUE(expression, N)
FROM FIRST
OVER (
    partition_clause
    order_clause
    frame_clause
) 
```

`NTH_VALUE()`函数返回`expression`窗口框架第N行的值。如果第N行不存在，则函数返回`NULL`。N必须是正整数，例如1,2和3。

`FROM FIRST`指示`NTH_VALUE()`功能在窗口帧的第一行开始计算。

请注意，SQL标准支持`FROM FIRST`和`FROM LAST`。但是，MySQL只支持`FROM FIRST`。如果要模拟效果`FROM LAST`，则可以使用其中`ORDER BY`的`over_clause`相反顺序对结果集进行排序。

## NTH_VALUE() 函数示例

创建一个以`basic_pay`演示命名的新表。

```sql
CREATE TABLE basic_pays
(
    employee_name VARCHAR(50) NOT NULL,
    department    VARCHAR(50) NOT NULL,
    salary        INT         NOT NULL,
    PRIMARY KEY (employee_name, department)
);

INSERT INTO basic_pays(employee_name,
                       department,
                       salary)
VALUES ('Diane Murphy', 'Accounting', 8435),
       ('Mary Patterson', 'Accounting', 9998),
       ('Jeff Firrelli', 'Accounting', 8992),
       ('William Patterson', 'Accounting', 8870),
       ('Gerard Bondur', 'Accounting', 11472),
       ('Anthony Bow', 'Accounting', 6627),
       ('Leslie Jennings', 'IT', 8113),
       ('Leslie Thompson', 'IT', 5186),
       ('Julie Firrelli', 'Sales', 9181),
       ('Steve Patterson', 'Sales', 9441),
       ('Foon Yue Tseng', 'Sales', 6660),
       ('George Vanauf', 'Sales', 10563),
       ('Loui Bondur', 'SCM', 10449),
       ('Gerard Hernandez', 'SCM', 6949),
       ('Pamela Castillo', 'SCM', 11303),
       ('Larry Bott', 'SCM', 11798),
       ('Barry Jones', 'SCM', 10586);
```

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200604150401.png"/></center>

## `NTH_VALUE()`函数在结果集上

以下语句使用`NTH_VALUE()`函数查找薪水第二高的员工：

```sql
select employee_name,
       salary,
       nth_value(employee_name, 2) over (
           order by salary desc
           ) second_highest_salary
from basic_pays;
```

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200604151229.png"/></center>

## `NTH_VALUE()` 函数分区示例

以下查询查找每个部门中薪水第二高的员工：

```sql
select employee_name,
       department,
       salary,
       nth_value(employee_name, 2) over (
           partition by department
           order by salary desc
           range between unbounded preceding and unbounded following
           ) second_highest_salary
from basic_pays;
```

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200604152127.png"/></center>