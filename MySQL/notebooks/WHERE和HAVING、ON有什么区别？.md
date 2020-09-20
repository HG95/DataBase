# WHERE和HAVING、ON有什么区别？

SQL 提供了多种对数据进行过滤的方式，包括 `WHERE`、`HAVING`以及`ON`子句等。虽然它们都能够实现类似的功能，但是你知道它们之间的区别吗？







## WHERE 和 HAVING、ON 有什么区别？

### WHERE 与 HAVING



`WHERE`与`HAVING`的根本区别在于：

- `WHERE`子句在`GROUP BY`分组和聚合函数**之前**对数据行进行过滤；
- `HAVING`子句对`GROUP BY`分组和聚合函数**之后**的数据行进行过滤。

因此，`WHERE`子句中不能使用聚合函数。



### WHERE 与 ON

当查询涉及多个表的关联时，我们既可以使用`WHERE`子句也可以使用`ON`子句指定连接条件和过滤条件。这两者之间的主要区别在于：

- 对于内连接（inner join）查询，`WHERE`和`ON`中的过滤条件等效；
- 对于外连接（outer join）查询，`ON`中的过滤条件在连接操作之前执行，`WHERE`中的过滤条件（逻辑上）在连接操作之后执行。

对于内连接查询而言，以下三个语句的结果相同：

```sql
-- 语句 1
select d.dept_name, e.emp_name, e.sex, e.salary 
from employee e, department d
where e.dept_id = d.dept_id
and e.emp_id = 10;
dept_name|emp_name|sex|salary |
---------|--------|---|-------|
研发部   |廖化    |男  |6500.00|

-- 语句 2
select d.dept_name, e.emp_name, e.sex, e.salary 
from employee e
join department d on (e.dept_id = d.dept_id and e.emp_id = 10);
dept_name|emp_name|sex|salary |
---------|--------|---|-------|
研发部   |廖化    |男  |6500.00|

-- 语句 3
select d.dept_name, e.emp_name, e.sex, e.salary 
from employee e
join department d on (e.dept_id = d.dept_id)
where e.emp_id = 10;
dept_name|emp_name|sex|salary |
---------|--------|---|-------|
研发部   |廖化    |男  |6500.00|

```

语句 1 在`WHERE`中指定连接条件和过滤条件；语句 2 在`ON`中指定连接条件和过滤条件；语句 3 在`ON`中指定连接条件，在`WHERE`中指定其他过滤条件。

尽管如此，仍然建议将两个表的连接条件放在`ON`子句中，将其他过滤条件放在`WHERE`子句中；这样语义更加明确，更容易阅读和理解。对于上面的示例而言，推荐使用语句 3 的写法。

对于外连接而言，连接条件只能用`ON`子句表示，因为`WHERE`子句无法表示外连接的语义。

<br>