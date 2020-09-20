# 修改数据库：ALTER DATABASE

在 [MySQL](http://c.biancheng.net/mysql/) 数据库中只能对数据库使用的字符集和校对规则进行修改，数据库的这些特性都储存在 db.opt 文件中。下面我们来介绍一下修改数据库的基本操作。

在 MySQL 中，可以使用 **`ALTER DATABASE`** 来修改已经被创建或者存在的数据库的相关参数。修改数据库的语法格式为：

```sql
ALTER DATABASE [数据库名] { 
[ DEFAULT ] CHARACTER SET <字符集名> |
[ DEFAULT ] COLLATE <校对规则名>}
```

语法说明如下：

- ALTER DATABASE 用于更改数据库的全局特性。
- 使用 ALTER DATABASE 需要获得数据库 ALTER 权限。
- 数据库名称可以忽略，此时语句对应于默认数据库。
- CHARACTER SET 子句用于更改默认的数据库字符集。

例 1

查看 test_db 数据库的定义声明的执行结果如下所示：

```sql
mysql> SHOW CREATE DATABASE test_db;
+----------+--------------------------------------------------------+
| Database | Create Database                                        |
+----------+--------------------------------------------------------+
| test_db  | CREATE DATABASE `test_db` /*!40100 DEFAULT CHARACTER SET utf8 */|
+----------+--------------------------------------------------------+
1 row in set (0.05 sec)
```

使用命令行工具将数据库 test_db 的指定字符集修改为 gb2312，默认校对规则修改为 utf8_unicode_ci，输入 SQL 语句与执行结果如下所示：

```sql
mysql> CREATE DATABASE test_db
    -> DEFAULT CHARACTER SET gb2312
    -> DEFAULT COLLATE gb2312_chinese_ci;
mysql> SHOW CREATE DATABASE test_db;
+----------+--------------------------------------------------------+
| Database | Create Database                                        |
+----------+--------------------------------------------------------+
| test_db  | CREATE DATABASE `test_db` /*!40100 DEFAULT CHARACTER SET gb2312 */|
+----------+--------------------------------------------------------+
1 row in set (0.00 sec)
```



## alter 修改表

添加索引