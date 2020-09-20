# LIKE语句

## LIKE：模糊查询

在 MySQL 中，`LIKE` 关键字主要用于搜索匹配字段中的指定内容。其语法格式如下：

```sql
[NOT] LIKE  '字符串'
```

其中：

- `NOT` ：可选参数，字段中的内容与指定的字符串不匹配时满足条件。
- 字符串：指定用来匹配的字符串。“字符串”可以是一个很完整的字符串，也可以包含通配符。

LIKE 关键字支持百分号“`%`”和下划线“`_`”通配符。

## 带有`%`通配符的查询

“`%`”是 MySQL 中最常用的通配符，它能代表任何长度的字符串，字符串的长度可以为 0。例如，`a%b`表示以字母 a 开头，以字母 b 结尾的任意长度的字符串。该字符串可以代表 ab、acb、accb、accrb 等字符串。

在 tb_students_info 表中，查找所有以字母“T”开头的学生姓名，SQL 语句和运行结果如下。

```sql
mysql> SELECT name FROM tb_students_info
    -> WHERE name LIKE 'T%';
```

## 带有“_”通配符的查询

“`_`”只能代表单个字符，字符的长度不能为 0。例如，`a_b`可以代表 acb、adb、aub 等字符串。

在 tb_students_info 表中，查找所有以字母“y”结尾，且“y”前面只有 4 个字母的学生姓名，SQL 语句和运行结果如下。

```sql
mysql> SELECT name FROM tb_students_info
    -> WHERE name LIKE '____y';
```



## 方括号`[]` 通配符

方括号 `[]` 通配符用来指定一个字符集，他必须匹配指定位置(通配符的位置) 的**一个字符**

例如，找出所有名字以 `J` 或 `M` 起头的联系人，

```sql
select *
from customers
where cust_contact like '[JM]%'
```

`[JM]` 匹配方括号中任意一个字符

此通配符可以用前缀字符 `^`(脱字号) 来否定

例如，查询匹配以` J` 和`M` 之外的任意字符起头的联系人。 

```sql
select *
from customers
where cust_contact like '[^JM]%'
```

## LIKE 区分大小写

默认情况下，LIKE 关键字匹配字符的时候是不区分大小写的。如果需要区分大小写，可以加入 `BINARY` 关键字。

在 tb_students_info 表中，查找所有以字母“t”开头的学生姓名，区分大小写和不区分大小写的 SQL 语句和运行结果如下。

```sql
mysql> SELECT name FROM tb_students_info WHERE name LIKE 't%';
+--------+
| name   |
+--------+
| Thomas |
| Tom    |
+--------+
2 rows in set (0.00 sec)

mysql> SELECT name FROM tb_students_info WHERE name LIKE BINARY 't%';
Empty set (0.01 sec)
```

## 使用通配符的注意事项和技巧

下面是使用通配符的一些注意事项：

- **注意大小写**。**MySQL 默认是不区分大小写的**。如果区分大小写，像“Tom”这样的数据就不能被“t%”所匹配到。
- **注意尾部空格**，尾部空格会干扰通配符的匹配。例如，“T% ”就不能匹配到“Tom”。
- **注意 NULL**。“%”通配符可以到匹配任意字符，但是不能匹配 NULL。也就是说 “%”匹配不到 tb_students_info 数据表中值为 NULL 的记录。


下面是一些使用通配符要记住的技巧。

- 不要过度使用通配符，如果其它操作符能达到相同的目的，应该使用其它操作符。因为 MySQL 对通配符的处理一般会比其他操作符花费更长的时间。
- 在确定使用通配符后，除非绝对有必要，否则不要把它们用在字符串的开始处。把通配符置于搜索模式的开始处，搜索起来是最慢的。
- 仔细注意通配符的位置。如果放错地方，可能不会返回想要的数据。