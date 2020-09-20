# UPDATE语句

可以使用 UPDATE 语句来修改、更新一个或多个表的数据。

## UPDATE 语句的基本语法

使用 UPDATE 语句修改单个表，语法格式为：

```sql
UPDATE <表名> SET 字段1=值1 [,字段2=值2… ] [WHERE 子句 ]
[ORDER BY 子句] [LIMIT 子句]
```

语法说明如下：

- `<表名>`：用于指定要更新的表名称。
- `SET` 子句：用于指定表中要修改的列名及其列值。其中，每个指定的列值可以是表达式，也可以是该列对应的默认值。如果指定的是默认值，可用关键字 DEFAULT 表示列值。
- `WHERE` 子句：可选项。用于限定表中要修改的行。若不指定，则修改表中所有的行。
- `ORDER BY` 子句：可选项。用于限定表中的行被修改的次序。
- `LIMIT` 子句：可选项。用于限定被修改的行数。

> 注意：修改一行数据的多个列值时，SET 子句的每个值用逗号分开即可。

## 修改表中的数据

【实例 1】在 tb_courses_new 表中，更新所有行的 course_grade 字段值为 4，输入的 SQL 语句和执行结果如下所示。

```sql
mysql> UPDATE tb_courses_new
    -> SET course_grade=4;
Query OK, 3 rows affected (0.11 sec)
Rows matched: 4  Changed: 3  Warnings: 0
mysql> SELECT * FROM tb_courses_new;
+-----------+-------------+--------------+------------------+
| course_id | course_name | course_grade | course_info      |
+-----------+-------------+--------------+------------------+
|         1 | Network     |            4 | Computer Network |
|         2 | Database    |            4 | MySQL            |
|         3 | Java        |            4 | Java EE          |
|         4 | System      |            4 | Operating System |
+-----------+-------------+--------------+------------------+
4 rows in set (0.00 sec)
```

## 根据条件修改表中的数据

【实例 2】在 tb_courses 表中，更新 course_id 值为 2 的记录，将 course_grade 字段值改为 3.5，将 course_name 字段值改为“DB”，输入的 SQL 语句和执行结果如下所示。

```sql
mysql> UPDATE tb_courses_new
    -> SET course_name='DB',course_grade=3.5
    -> WHERE course_id=2;
Query OK, 1 row affected (0.13 sec)
Rows matched: 1  Changed: 1  Warnings: 0
mysql> SELECT * FROM tb_courses_new;
+-----------+-------------+--------------+------------------+
| course_id | course_name | course_grade | course_info      |
+-----------+-------------+--------------+------------------+
|         1 | Network     |            4 | Computer Network |
|         2 | DB          |          3.5 | MySQL            |
|         3 | Java        |            4 | Java EE          |
|         4 | System      |            4 | Operating System |
+-----------+-------------+--------------+------------------+
4 rows in set (0.00 sec)
```

注意：**保证 UPDATE 以 WHERE 子句结束，通过 WHERE 子句指定被更新的记录所需要满足的条件，如果忽略 WHERE 子句，MySQL 将更新表中所有的行**。





## 案例

## 627. 交换工资 [简单]

给定一个 salary 表，如下所示，有 m = 男性 和 f = 女性 的值。交换所有的 f 和 m 值（例如，将所有 f 值更改为 m，反之亦然）。要求只使用一个更新（Update）语句，并且没有中间的临时表。

注意，您必只能写一个 Update 语句，请不要编写任何 Select 语句。

例如：

| id   | name | sex  | salary |
| ---- | ---- | ---- | ------ |
| 1    | A    | m    | 2500   |
| 2    | B    | f    | 1500   |
| 3    | C    | m    | 5500   |
| 4    | D    | f    | 500    |

运行你所编写的更新语句之后，将会得到以下表:

| id   | name | sex  | salary |
| ---- | ---- | ---- | ------ |
| 1    | A    | f    | 2500   |
| 2    | B    | m    | 1500   |
| 3    | C    | f    | 5500   |
| 4    | D    | m    | 500    |

### 解题

方法一：

```sql
update salary
set sex=if(sex='f','m','f');
```

方法二 ：使用 `case when...end`

```sql
UPDATE salary
SET
    sex = CASE sex
        WHEN 'm' THEN 'f'
        ELSE 'm'
    END;
```

`