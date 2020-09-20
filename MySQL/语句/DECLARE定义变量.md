# DECLARE定义变量

变量分文局部变量和全局变量

局部变量是`@`开头，全局变量是`@@`开头，

在 sql 中，定义一个变量需要关键字 `DECLARE`, 还需要个特殊符号标记（`@`）表示是变量。

看看简单的声明语法：

```sql
Declare @Local_Var data_type;
```

@Local_Var是一个整体，表示一个变量。

data_type就是数据类型了，这个大家都很熟悉的，例如int,decimal ,float,text等。

变量声明了，怎么赋值呢，能在声明的时候赋值么？像这样

```sql
declare @ID=2 varchar(20);
```

这样是不行的

但是这样呢

```sql
declare @ID varchar(20)=2
print @ID  --这句话的意思是在sql server窗口中打印出变量的值
这样是正确的，结果是
---------
2
```

声明可以赋值，再声明后是可以再赋值的，
这里有两种方式赋值
`set`, `select` ,先看基本用法，再说区别

## 基本用法

```sql
declare @ID varchar(20)      --定义一个变量叫@ID
set @ID=3                    --变量赋值为3
print @ID                    --打印  
select @ID=1                 --变量赋值为1
print @ID                    --打印

查看结果
-------------

3
1
```

从上面看出来了，Set,与select都可以给变量赋值。

然后我们看看变量的运算，运算其实很简单，下面看看加减法

```sql
declare @ID varchar(20)
set @ID=3
print @ID
select @ID=1+@ID       --将变量@id加1
print @ID
select @ID=(select 1+5)  --类似于@ID=1+5
print @ID
select @ID=(select 1-@ID)  --类似于@ID=1-@ID
print @ID

结果
-----------
　　3
　　4
　　6
　 -5
```

## 区别

1，表达式返回多个值时

```sql
表达式返回多个值时，使用 SET 赋值

declare @name varchar(128)
set @name=(select username from userinfo)
print @name
/*
--出错信息为
服务器: 消息 512，级别 16，状态 1，行 2
子查询返回的值多于一个。当子查询跟随在 =、!=、<、<=、>、>= 之后，或子查询用作表达式时，这种情况是不允许的。
*/


表达式返回多个值时，使用 SELECT 赋值
declare @name varchar(20)
select @name= username from userinfo
print @name --结果集中最后一个 username 列的值
结果: 
---------
wangwu
```

2,表达式未返回值时

```sql
--表达式未返回值时，使用 SET 赋值
declare @name varchar(20)
set @name='jack'
set @name= (select username from userinfo where username='not')
print @name  --Null值

结果
--------

--表达式未返回值时，使用 SELECT 赋值
declare @name varchar(20)
set @name='jack'
select @name=username from userinfo where username='not'
print @name  --jack,保存原来的值

结果
-------
jack
```


下表列出 SET 与 SELECT 的区别。

<img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200819105811.png" alt="image-20200819105806593" style="zoom:80%;" />



参考

- <a href="https://www.cnblogs.com/lideng/archive/2013/04/11/3014407.html" target="">SQL 存储过程入门（变量）</a>