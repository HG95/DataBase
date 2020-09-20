# 分区函数Partition By

`partition by`关键字是分析性函数的一部分，它和聚合函数不同的地方在于它能返回一个分组中的多条记录，而聚合函数一般只有一条反映统计值的记录，`partition by`用于给结果集分组，如果没有指定那么它把整个结果集作为一个分组，分区函数一般与排名函数一起使用。



准备测试数据：

```sql
create table Student
(
 id int,
 Grade int,
 Score int
);


insert into Student values  (1,1,88),
                            (2,1,66),
                            (3,1,75),
                            (4,2,30),
                            (5,2,70),
                            (6,2,80),
                            (7,2,60),
                            (8,3,90),
                            (9,3,70),
                            (10,3,80),
                            (11,3,80);
```

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200602221117.png"/></center>

## 一、分区函数`Partition By `的与`row_number()` 的用法

1、不分班按学生成绩排名

```sql
select *,row_number() over(order by Score desc) as Sequence 
from Student;
-- 让数据先按班级进行分组，在每个组内进行排序
```

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200602221501.png"/></center>

2、分班后按学生成绩排名

```sql
select *, 
       row_number() over (partition by Grade order by Score desc ) Sequence
from student;
```

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200602222342.png"/></center>

3、获取每个班的前 2 (几)名

```sql
select * from (
              select *, row_number() over (
                  partition by Grade order by Score desc
                  ) Sequence
    from student
                  ) T
where T.Sequence<=2;
```

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200602222829.png"/></center>

`T.Sequence<=2` 是排名的序号 <=2



## 二、分区函数`Partition By` 与排序 `rank()`的用法

1、分班后按学生成绩排名 该语句是对分数相同的记录进行了同一排名，例如：两个80分的并列第2名，第3名就没有了

```sql
select *,
       rank() over(partition by Grade order by Score desc) as Sequence
from Student;
```

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200602223309.png"/></center>

2、获取每个班的前2(几)名 该语句是对分数相同的记录进行了同一排名，例如：两个80分的并列第2名，第4名就没有了

```sql
select *
from (
     select *,
            rank() over (partition by Grade order by Score desc ) Sequence
    from student
         ) T
where T.Sequence<=2;
```

<center><img src="https://raw.githubusercontent.com/HG1227/image/master/img_tuchuang/20200602223737.png"/></center>

## 参考

- <a href="https://www.cnblogs.com/linJie1930906722/p/6036053.html" target="_blank">分区函数Partition By的与row_number()的用法以及与排序rank()的用法详解(获取分组(分区)中前几条记录</a>
- <a href="https://blog.csdn.net/weixin_41770169/article/details/82691501" target="_blank">SQL中Partition关键字的使用</a>