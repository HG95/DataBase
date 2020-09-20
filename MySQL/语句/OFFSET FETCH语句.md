# OFFSET FETCH语句

## 语法

The following illustrates the syntax of the `OFFSET` and `FETCH` clauses:

```sql
ORDER BY column_list [ASC |DESC]
OFFSET offset_row_count {ROW | ROWS}
FETCH {FIRST | NEXT} fetch_row_count {ROW | ROWS} ONLY
```

In this syntax:

- The `OFFSET` clause specifies the number of rows to skip before starting to return rows from the query. The `offset_row_count` can be a constant, variable, or parameter that is greater or equal to zero.
- The `FETCH` clause specifies the number of rows to return after the `OFFSET` clause has been processed. The `offset_row_count` can a constant, variable or scalar that is greater or equal to one.
- The `OFFSET` clause is mandatory while the `FETCH` clause is optional. Also, the `FIRST` and `NEXT` are synonyms(同义词) respectively so you can use them interchangeably(可互换). Similarly, you can use the `FIRST` and `NEXT` interchangeably.



The following illustrates the `OFFSET` and `FETCH` clauses:



<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200819104719.png" alt="SQL-Server-OFFSET-FETCH" style="zoom:80%;" />

Note that you must use the `OFFSET` and `FETCH` clauses with the `ORDER BY` clause. Otherwise, you will get an error.

We will use the `products` table from the [sample database](https://www.sqlservertutorial.net/sql-server-sample-database/) for the demonstration.

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200819104714.png" alt="image-20200819102301431" style="zoom:80%;" />

The following query returns all products from the `products` table and sorts the products by their list prices and names:

```sql
SELECT
    product_name,
    list_price
FROM
    production.products
ORDER BY
    list_price,
    product_name;
```

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200819104638.png" alt="image-20200819102348108" style="zoom:80%;" />

To skip the first 10 products and return the rest, you use the `OFFSET` clause as shown in the following statement:

```sql
SELECT
    product_name,
    list_price
FROM
    production.products
ORDER BY
    list_price,
    product_name 
OFFSET 10 ROWS;
```

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200819104644.png" alt="image-20200819102426688" style="zoom:80%;" />

To skip the first 10 products and select the next 10 products, you use both `OFFSET` and `FETCH` clauses as follows:

```sql
SELECT
    product_name,
    list_price
FROM
    production.products
ORDER BY
    list_price,
    product_name 
OFFSET 10 ROWS 
FETCH NEXT 10 ROWS ONLY;
```

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200819104652.png" alt="image-20200819102511437" style="zoom:80%;" />

To get the top 10 most expensive products you use both `OFFSET` and `FETCH` clauses:

```sql
SELECT
    product_name,
    list_price
FROM
    production.products
ORDER BY
    list_price DESC,
    product_name 
OFFSET 0 ROWS 
FETCH FIRST 10 ROWS ONLY;
```

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200819104657.png" alt="image-20200819102547101" style="zoom:80%;" />

In this example, the `ORDER BY` clause sorts the products by their list prices in descending order. Then, the `OFFSET` clause skips zero row and the `FETCH` clause fetches the first 10 products from the list.



## [分页实现：Offset-Fetch](https://www.cnblogs.com/ljhdo/p/4861263.html)

Offset-Fetch子句要求结果集是有序的，因此，只能用于order by 子句中，语法如下：

```sql
ORDER BY order_by_expression [ ASC | DESC ]  [ ,...n ] [ <offset_fetch> ]
<offset_fetch> ::=
{ 
    OFFSET { integer_constant | offset_row_count_expression } ROWS
    [ FETCH NEXT {integer_constant | fetch_row_count_expression } ROWS ONLY ]
}
```

关键字解析：

- **Offset子句**：用于指定跳过（Skip）的数据行；
- **Fetch子句**：该子句在Offset子句之后执行，表示在跳过（Sikp）指定数量的数据行之后，返回一定数据量的数据行；
- **执行顺序**：Offset子句必须在Order By 子句之后执行，Fetch子句必须在Offset子句之后执行；



**分页实现的思路：**

1. **在分页实现中，使用Order By子句，按照指定的columns对结果集进行排序；**
2. **使用Offset子句跳过前N页：Offset (@PageIndex-1)\*@RowsPerPage rows；**
3. **使用Fetch子句呈现当前Page：Fetch next @RowsPerPage rows only；**



### 使用order-offset-fetch分页

**创建示例数据**

```sql
use tempdb
go
create table dbo.dt_test
(
    id int,
    code int
)
go
insert into dbo.dt_test(id,code)
values(1,1),(2,2),(3,1),(4,2),(5,1),(6,2)
```

**使用Offset子句跳过指定数目的数据行**

```sql
select * 
from dbo.dt_test
order by id
offset 2 rows
```

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200819104704.png" alt="image-20200819104217978" style="zoom:80%;" />



**使用Offset-Fetch子句跳过指定数目的数据行之后，返回指定数目的数据行**

```sql
select * 
from dbo.dt_test
order by id
offset 2 rows
fetch next 2 rows only
```

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200819104708.png" alt="image-20200819104249669" style="zoom:80%;" />



**修改成分页的通用格式**

```sql
--分页的索引，页码从1开始
declare @PageIndex int
--每页显示的行数
declare @Size int

set @PageIndex=1
set @Size=100

select * 
from dbo.dt_test
order by id
offset (@PageIndex - 1) * @Size rows
fetch next @Size rows only
```



## 排序（order by）

`order by`子句的语法是：ORDER BY order_by_expression ，用于按照指定字段进行排序，通常有3种写法：

- select子句中列的name，或alias，排序子句（order by）的执行顺序在select子句之后，可以使用列的Alias进行排序；
- 表达式，按照表达式的计算结果进行排序；
- select子句中列的序号，从1开始，此处的数值是序号，不建议使用；

上述三种写法都会对查询结果集进行排序，返回的结果集是有序的，但是，如果这样写，在order by子句中使用一个常量：

```sql
order by (select 1) 
```

该子句中的 1 不是列的序号，而是常量，SQL Server按照结果集的原始顺序返回，order by子句不对结果集排序。







参考

- <a href="https://www.sqlservertutorial.net/sql-server-basics/sql-server-offset-fetch/" target="">SQL Server OFFSET FETCH</a> 
- <a href="https://www.cnblogs.com/ljhdo/p/4861263.html" target="">分页实现：Offset-Fetch</a> 