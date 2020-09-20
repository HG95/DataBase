# GROUP_WS函数

## `CONCAT_WS() `函数

使用函数`CONCAT_WS()`。使用语法为：

```sql
CONCAT_WS(separator,str1,str2,…)
```



`CONCAT_WS() `代表 CONCAT With Separator ，是CONCAT()的特殊形式。

第一个参数是其它参数的分隔符。分隔符的位置放在要连接的两个字符串之间。分隔符可以是一个字符串，也可以是其它参数。**如果分隔符为 NULL，则结果为 NULL**。函数会忽略任何分隔符参数后的 NULL 值。**但是`CONCAT_WS()`不会忽略任何空字符串。** (然而会忽略所有的 NULL）。

```sql
select concat_ws('-',c_id,s_id)
from score
where s_id='01';
```

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200603222312.png"/></center>


## 参考

- <a href="https://segmentfault.com/a/1190000004844113" target="_blank">mysql中函数CONCAT及GROUP_CONCAT的使用</a>
- <a href="https://www.jianshu.com/p/447eb01eebb2" target="_blank">MySQL 的 GROUP_CONCAT 函数详解</a>