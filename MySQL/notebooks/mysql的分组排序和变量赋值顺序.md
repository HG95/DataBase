# mysql的分组排序和变量赋值顺序

有个这样的需求是，根据不同性别进行分组排序他们的年龄，并得到序列号

```sql
CREATE TABLE person (id int, first_name varchar(20), age int, gender char(1));

INSERT INTO person VALUES (1, 'Bob', 25, 'M');
INSERT INTO person VALUES (2, 'Jane', 20, 'F');
INSERT INTO person VALUES (3, 'Jack', 30, 'M');
INSERT INTO person VALUES (4, 'Bill', 32, 'M');
INSERT INTO person VALUES (5, 'Nick', 22, 'M');
INSERT INTO person VALUES (6, 'Kathy', 18, 'F');
INSERT INTO person VALUES (7, 'Steve', 36, 'M');
INSERT INTO person VALUES (8, 'Anne', 25, 'F');
```

<img src=".\img\image-20200825205931750.png" alt="image-20200825205931750" style="zoom:80%;" />

先来得到想要的结果

```sql
select 
    first_name,
    gender,
    age ,
    rank
from
    (select  
        first_name,
        gender,
        age,
        @rank:=if(@gen=gender,@rank+1,1) rank,
        @gen:=gender
    from person,(select @rank:=0,@gen:=null) temp
    order by gender, age asc) b
```

简便方法

```sql
select first_name,
       age,
       gender,
       dense_rank() over ( partition by gender order by age) rk
from person;

```

<img src=".\img\image-20200825211832050.png" alt="image-20200825211832050" style="zoom:80%;" />

## mysql 变量解释

通过 `set `赋值变量

<img src=".\img\image-20200825211906168.png" alt="image-20200825211906168" style="zoom:80%;" />

通过 `select `赋值变量

<img src=".\img\image-20200825211943348.png" alt="image-20200825211943348" style="zoom:80%;" />

## 解释上面的分组排序代码

第一步先赋值变量

<img src=".\img\image-20200825212019336.png" alt="image-20200825212019336" style="zoom:80%;" />

- 第二步使用**`IF`**条件进行分组

下面，我们不要太关注**`from person,(select @rank:=0,@gen:=null) temp`**，就是当作进行变量的初始化就好

```sql
   select  
        first_name,
        gender,
        age,
        @rank:=if(@gen=gender,@rank+1,1) rank,
        @gen:=gender
    from person,(select @rank:=0,@gen:=null) temp
    order by gender, age asc
```

**a.第一步：**变量赋值，是先运行from 后面的内容，以及排序，排序的目的是把 **F**、**M**放到各自的组中（这一点和我们原来的先select 后order 是不一样的，等下会有说明）此时@rank等于0,@gen等于null
 **b.第二步：** 开始进行select中的内容，会先进行
 **第一行**，运行
 **@rank:=if(@gen=gender,@rank+1,1) rank**，此时@gen是等于null的，而gender 是第一行的值，所以IF函数将会返回1，第一行的rank就会返回1，接着运行**@gen:=gender** ，此时的@gen会被赋值第一行的值
 **第二行**,
 还是先运行**@rank:=if(@gen=gender,@rank+1,1) rank**，此时的@gen是等于gender，根据IF会返回@rank+1 然后赋值到@rank，直到遇到下一个不一样的gender，@rank 才会重新变成1

## 变量赋值顺序

```sql
set @rownum:=0;
select 
        first_name,
        gender,
        age, @rownum as rownum
from person
where @rownum<1
order by first_name,least(0,@rownum:=@rownum+1);
```

在sql 语句中的执行顺序是 from 、where 、select 、order by
 在这我们的疑问是先进行的order by 后进行的 select

如果是先进行的select 的话，rownum会输出0、1，而真实的结果是
 rownnum是输出的1、2

<img src=".\img\image-20200825212304393.png" alt="image-20200825212304393" style="zoom:80%;" />

所以我们可以暂认为是先进行的order by 后进行的 select，因为没有找到官方的说明。

## 注意事项

mysql 的变量赋值有**`=`** 和 **`:=`**，这两种形式，但是在**select** 后面的赋值，要用**:=**这种形式，如果不用就会出现这样的问题。

<img src=".\img\image-20200825212423166.png" alt="image-20200825212423166" style="zoom:80%;" />







作者：数据蛙datafrog
链接：https://www.jianshu.com/p/d732d1cb1a89
来源：简书
