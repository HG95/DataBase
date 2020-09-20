# substring_index函数

截取字符串的函数：`substring_index`。

用法规则：

```sql
substring_index（“待截取有用部分的字符串”，“截取数据依据的字符”，截取字符的位置N）
```

详细说明：

1. 首先，设待处理对象字符串为“15,151,152,16”（虽然这里指的不是iP，可以看作是IP来处理吧）
2.   这里截取的依据是逗号：“**，**”
3.   具体要截取第**N**个逗号前部分的字符;

 意思是：在字符串中以逗号为索引，获取不同索引位的字符。

-  取第一个逗号前的字符串 ：

  ```sql
  root@localhost|iris>SELECT SUBSTRING_INDEX('15,151,152,16',',',1);
  +----------------------------------------+
  | SUBSTRING_INDEX('15,151,152,16',',',1) |
  +----------------------------------------+
  | 15                                     |
  +----------------------------------------+
  1 row in set (0.00 sec)
  ```

- 截取第二个逗号前面部分

  ```sql
  root@localhost|iris>SELECT SUBSTRING_INDEX('15,151,152,16',',',2);
  +----------------------------------------+
  | SUBSTRING_INDEX('15,151,152,16',',',2) |
  +----------------------------------------+
  | 15,151                                 |
  +----------------------------------------+
  1 row in set (0.00 sec)
  ```

**N可以为负数**，表示倒数第N个索引字符后面的字符串。*有负号的时候，可以将整个字符倒过来看，依旧是第N个字符前面的部分。*

- 截取目标字符串中最后一个含 “**，**” 位子的后的部分：

  ```sql
  root@localhost|iris>SELECT SUBSTRING_INDEX('15,151,152,16',',',-1) as 'subrting';
  +----------+
  | subrting |
  +----------+
  | 16       |
  +----------+
  1 row in set (0.00 sec)
  ```

- 取倒数第2个逗号前那部分字符串里，最后逗号后面的部分

  ```sql
  root@localhost|iris>SELECT SUBSTRING_INDEX(SUBSTRING_INDEX('15,151,152,16',',',2),',',-1) as 'subrting';
  +----------+
  | subrting |
  +----------+
  | 151      |
  +----------+
  1 row in set (0.00 sec)
  ```





参考：

-  <a href="https://blog.csdn.net/iris_xuting/article/details/38415181" target="_blank">mysql函数substring_index的用法</a>

