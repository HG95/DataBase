# ORDER BY语句

## ORDER BY：对查询结果排序

`ORDER BY` 关键字主要用来将查询结果中的数据按照一定的顺序进行排序。其语法格式如下：

```sql
ORDER BY <字段名> [ASC|DESC]
```

语法说明如下。

- 字段名：表示需要排序的字段名称，多个字段时用逗号隔开。
- ASC\|DESC：`ASC`表示字段按升序排序；`DESC`表示字段按降序排序。其中`ASC`为默认值。


使用 ORDER BY 关键字应该注意以下几个方面：

- ORDER BY 关键字后可以跟子查询（关于子查询后面教程会详细讲解，这里了解即可）。
- 当排序的字段中存在空值时，ORDER BY 会将该空值作为最小值来对待。
- ORDER BY 指定多个字段进行排序时，MySQL 会按照字段的顺序从左到右依次进行排序。

## 单字段排序

```sql
 SELECT * FROM tb_students_info ORDER BY height
```

## 多字段排序

```sql
SELECT name,height FROM tb_students_info ORDER BY height,name;
```

注意：在对多个字段进行排序时，排序的第一个字段必须有相同的值，才会对第二个字段进行排序。如果第一个字段数据中所有的值都是唯一的，MySQL 将不再对第二个字段进行排序。

默认情况下，查询数据按字母升序进行排序（A～Z），但数据的排序并不仅限于此，还可以使用 ORDER BY 中的 DESC 对查询结果进行降序排序（Z～A）。

```sql
SELECT name,height FROM tb_student_info ORDER BY height DESC,name ASC;
```

## 按列的位置排序

```sql
SELECT name,height FROM tb_student_info ORDER BY 3 DESC,name ASC;
```

数字 3 表示按表的第三列的字段排序

注意： 

- 不支持大小写的排序
- 在指定一条 `order by` 子句时，应该保证它是 `select` 语句中最后一条子句。

## SQL 时间排序

sql 中的时间排序

例如

```sql
sql = "select *  from  user  where Putout=true  order by time  desc"  //按最新时间来排序
sql = "select * from  user  where Putout=true  order by time   asc"   //按早时间来排序
```

可以把时间理解成 时间是有小到大增加的，当成普通的数字。