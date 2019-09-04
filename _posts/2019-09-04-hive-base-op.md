### hive
#### 简介
>Apache Hive是一个建立在Hadoop架构之上的数据仓库。它能够提供数据的精炼，查询和分析。
>Hive的出现可以让非java编程者对hdfs的数据做mapreduce操作。
>
#### 命令
> Hive基本操作
>
>（1）启动hive
>
> `[atguigu@hadoop102 hive]$ bin/hive`
>
>（2）查看数据库
>
> `hive> show databases;`
>
>（3）打开默认数据库
>
> `hive> use default;`
>
>（4）显示default数据库中的表
>
> `hive> show tables;`
>
>（5）创建一张表
>
>`hive> create table student(id int, name string);`
>
>（6）显示数据库中有几张表
>
>`hive> show tables;`
>
>（7）查看表的结构
>
>`hive> desc student;`
>
>（8）向表中插入数据
>
>`hive> insert into student values(1000,"ss");`
>
>（9）查询表中数据
>
>`hive> select * from student;`
>
>（10）退出hive
>
>`hive> quit;`


####
