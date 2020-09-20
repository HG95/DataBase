# GROUP_CONCAT函数

`GROUP_CONCAT(expr) `函数会从 expr 中连接所有非 NULL 的字符串。如果没有非 NULL 的字符串，那么它就会返回 NULL。

MySQL 的 GROUP_CONCAT 函数详解



```sql
group_concat([DISTINCT] 要连接的字段(可以是多个) 
             [Order BY ASC/DESC 排序字段] 
             [Separator '分隔符'])
```

Separator 是一个字符串值，它被用于插入到结果值中。缺省为一个逗号 (",")，可以通过指定 SEPARATOR "" 完全地移除这个分隔符。





## 1 使用示例

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200603220119.png"/></center>

```sql
select GROUP_CONCAT(c_id) id
from score
where s_id = '01';
```

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200603220202.png"/></center>


【2】可以使用 `DISTINCT` 过滤重复的值，也可以加入 `ORDER BY` 对值进行排序，还可以使用 `SEPARATOR` 指定分隔符：

```sql
select GROUP_CONCAT(c_id separator ' ') id
from score
where s_id = '01';
```

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200603220631.png"/></center>

如果将多个字段进行连接，会首先将这几个字段连接，然后再用指定的分隔符连接前面拼接好的字符串

```sql
select GROUP_CONCAT(c_id,s_id separator '-') id
from score
where s_id = '01';
```

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200603221810.png"/></center>


## 2 最大值限制

`GROUP_CONCAT() `是有最大长度限制的，默认值是 1024。

可以通过 `group_concat_max_len` 参数进行动态设置。参数范围可以是 Global 或 Session。

设置语法如下：

```sql
SET [GLOBAL |SESSION] group_concat_max_len= val
```

示例

```sql
SET SESSION group_concat_max_len=18446744073709551615;
SELECT GROUP_CONCAT(a.REGION_ID) FROM t_region a;
```





