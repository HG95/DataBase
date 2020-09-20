# LEFT JOIN语句

## 左连接

左外连接又称为左连接，使用` LEFT OUTER JOIN `关键字连接两个表，并使用 `ON` 子句来设置连接条件。

左连接的语法格式如下：

```sql
SELECT <字段名> 
FROM <表1> 
LEFT OUTER JOIN <表2> 
<ON子句>
```



语法说明如下。

- 字段名：需要查询的字段名称。
- <表1><表2>：需要左连接的表名。
- `LEFT OUTER JOIN`：左连接中可以省略 OUTER 关键字，只使用关键字 `LEFT JOIN`。
- `ON` 子句：用来设置左连接的连接条件，不能省略。


上述语法中，“表1”为基表，“表2”为参考表。左连接查询时，可以查询出“表1”中的所有记录和“表2”中匹配连接条件的记录。**如果“表1”的某行在“表2”中没有匹配行，那么在返回结果中，“表2”的字段值均为空值（NULL）**。

<img src=".\img\image-20200816213534191.png" alt="image-20200816213534191" style="zoom:80%;" />

<img src=".\img\image-20200816213624600.png" alt="image-20200816213624600" style="zoom:80%;" />



## 案例

```sql
select *
from pd
where user_id=1;
```

<img src=".\img\image-20200909095244269.png" alt="image-20200909095244269" style="zoom:80%;" />

```sql
select *
from (select convert(log_time, date) day1,
             user_id,
             oprt_type
      from pd) a
         left join (select convert(log_time, date) day2,
                           user_id,
                           oprt_type
                    from pd) b
                   on b.user_id = a.user_id
where a.user_id=1;
```

<img src=".\img\image-20200909095639712.png" alt="image-20200909095639712" style="zoom:80%;" />