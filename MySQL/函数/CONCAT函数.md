# CONCAT函数

## `CONCAT()`函数

CONCAT（）函数用于将多个字符串连接成一个字符串。
使用数据表Info作为示例，其中SELECT id,name FROM info LIMIT 1;的返回结果为

```sql
+----+--------+
| id | name   |
+----+--------+
|  1 | BioCyc |
+----+--------+
```

1、语法及使用特点：

```sql
CONCAT(str1,str2,…)
```



返回结果为连接参数产生的字符串。**如有任何一个参数为NULL ，则返回值为 NULL**。可以有一个或多个参数。

2、使用示例：
`SELECT CONCAT(id, ‘，’, name) AS con FROM info LIMIT 1;`返回结果为

```sql
+----------+
| con      |
+----------+
| 1,BioCyc |
+----------+
```

`SELECT CONCAT(‘My’, NULL, ‘QL’);` 返回结果为

```sql
+--------------------------+
| CONCAT('My', NULL, 'QL') |
+--------------------------+
| NULL                     |
+--------------------------+
```



3、如何指定参数之间的分隔符