# ANY和ALL运算符



`ANY`和`ALL`运算符与`WHERE`或`HAVING`子句一起使用。

如果任何子查询值满足条件，则`ANY`运算符返回`true`。

如果所有子查询值满足条件，则`ALL`运算符返回`true`。

## ANY 语法

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name operator ANY
(SELECT column_name FROM table_name WHEREcondition); 
```

## ALL 语法

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name operator ALL
(SELECT column_name FROM table_name WHERE condition); 
```

> **注意：**所述的*操作符*必须是标准比较运算符（=，<>，=，>，> =，<，或<！=）。

## **示例**

我们新建两个表Test1和Test2

Test1表结构如下：

| Num  |
| ---- |
| 2    |
| 3    |
| 4    |

Test2表结构如下：

| Num  |
| ---- |
| 1    |
| 2    |
| 3    |
| 4    |
| 5    |



## **ALL使用示例**

示例1

```sql
SELECT Num FROM Test2
WHERE Num > ALL (SELECT Num FROM Test1)
```

结果为：

| Num  |
| ---- |
| 5    |

从上面的结果我们可以看出，只有Test2中的5才是大于Test1中的所有数。

示例2

```sql
SELECT Num  FROM Test2
WHERE Num < ALL (SELECT Num FROM Test1)
```

结果为：

| Num  |
| ---- |
| 1    |

从上面的结果我们可以看出，只有Test2中的1才是小于Test1中的所有数。

## **ANY/SOME使用示例**

示例

```sql
SELECT Num FROM Test2
WHERE Num > ANY (SELECT Num FROM Test1)

SELECT Num FROM Test2
WHERE Num > SOME (SELECT Num FROM Test1)
```



他们的结果均为：

| Num  |
| ---- |
| 3    |
| 4    |
| 5    |

从上面的结果我们可以看出，ANY和SOME是等价的，而且Test2中的任何一个数都满足大于Test1中的数。比如Test2中的3就大于2

## "=ANY"与"IN"相同

示例

```sql
SELECT Num FROM Test2
WHERE Num = ANY (SELECT Num FROM Test1)

SELECT Num FROM Test2
WHERE Num IN (SELECT Num FROM Test1)
```

他们的结果均为：

| Num  |
| ---- |
| 2    |
| 3    |
| 4    |

表示Test1中的任何一个数都存在于Test2中

## "<>ALL"与"NOT IN"相同

示例

```sql
SELECT Num FROM Test2
WHERE Num <> ALL (SELECT Num FROM Test1)

SELECT Num FROM Test2
WHERE Num NOT IN (SELECT Num FROM Test1)
```



他们的结果均为：

| Num  |
| ---- |
| 1    |
| 5    |

表示Test2中的结果都不存在与Test1中

这三个关键字不常用，但是如果遇到了知道是什么意思，怎么用就好了。



参考

- <a href="https://zhuanlan.zhihu.com/p/94659348" target="_blank">SQL中的ALL、ANY和SOME的用法介绍</a>