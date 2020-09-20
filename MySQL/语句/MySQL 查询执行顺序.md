# MySQL 查询执行顺序

## SQL 查询执行顺序

示例

```sql
SELECT DISTINCT t1.column1 AS alias1, t2.col2, aggregate_function(column3)
  FROM table1 t1
  JOIN table2 t1 ON t1.column1 = t2.column1
 WHERE conditions
 GROUP BY t1.column1, t2.col2
HAVING group_condition
 ORDER BY t1.column1 ASC, t2.col2 DESC
OFFSET m ROWS
 FETCH NEXT num_rows ROWS ONLY;

```

以上就是 SQL 语法中常见的各个子句（当然不是全部子句）的书写顺序，但是它们的逻辑执行顺序却与此不同：

1. 首先，`FROM`和`JOIN`是SQL执行的第一步，它们从逻辑上决定了接下来要操作的数据集；

2. 其次，执行`WHERE`子句，对上一步的数据集进行过滤，保留满足条件的行；需要注意的是，此时还没有执行`SELECT`子句，`WHERE`条件中不能引用`SELECT`列表中的列别名（alias1）或者聚合函数
3. 接下来，基于`GROUP BY`子句指定的表达式进行分组，分组条件有多少不同的取值，操作后的结果就有多少行；
4. 然后，基于分组结果执行聚合函数 `aggregate_function`，对于每个分组取值，生成一个函数结果；如果没有分组子句，基于所有结果执行一次聚合操作；
5. 如果查询中使用了`GROUP BY`，可以使用 `HAVING` 针**对分组后的结果进一步进行过滤**，通常是利用聚合函数的值进行过滤，如果是其他过滤条件，可以在`WHERE`子句中提前指定，避免无谓的操作；
6. 接下来，`SELECT`子句可以选择要显示的列。如果存在`GROUP BY`子句，`SELECT`列表只能引用分组所使用的列，或者聚合函数；如果不存在`GROUP BY`子句，可以引用`FROM`和`JOIN`指定的表中的任何列；
7. 如果在`SELECT`之后指定了`DISTINCT`关键字，需要针对上一步的结果集进行去重操作，过滤掉所有重复的值；
8. 应用`ORDER BY`子句对上一步的结果进行最终的排序操作。如果存在`GROUP BY`子句或者`DISTINCT`关键字，只能使用`SELECT`列表中出现的字段进行排序；如果不存在`GROUP BY`子句和`DISTINCT`，可以使用`FROM`和`JOIN`指定的表中的任何列；
9. 最后，`OFFSET`和`FETCH`（或者`LIMIT`、`TOP`）限定了返回的行。




参考：

<https://www.cnblogs.com/xiaohuochai/p/6081482.html>