---
layout: post
title: SQL基础语句
date: 2020-09-28 14:20:23 +0900
category: sql
---

* 显示数据库
--> **show databases;**

* 判断是否存在数据库test_mysql,有的话先删除
--> **drop database if exists test_mysql;**

* 创建数据库
--> **create database test_mysql;**

* 删除数据库
--> **drop database test_mysql;**

* 使用该数据库
--> **use test_mysql;**

* 显示数据库中的表
--> **show tables;**

* 先判断表是否存在,存在先删除
--> **drop table if exists student;**

* 删除表
--> **drop table student;**

* 查看表的结构
--> **des student;**

* 创建表
```
create table student(
id int auto_increment primary key,
name varchar(50),
sex varchar(20),
date varchar(50),
)default charset=utf8;
```

* 插入数据
```
insert into student values(null,'test','2018-10-2');
```

* 查询表中的数据
```
select * from student;
select id,name from student;
```

* 修改某一条数据
```
update student set name='jack' where id=4;
```

* 删除数据
```
delete from student where id=8;
```

* and 且
```
select * from student where date>'2018-1-2' and date<'2018-12-1';
```

* or 或
```
select * from student where date<'2018-11-2' or date>'2018-12-1';
```

* between
```
select * from student where date between '2018-1-2' and '2018-12-1';
```

* in 查询制定集合内的数据
```
select * from student where id in (1,3,5);
```

* 排序-**asc升序\desc降序**
```
select * from student order by id asc;
```

* 分组查询-聚合函数
```
select max(id),name,sex from student group by sex;
select min(date) from student;
select avg(id) as 'Avg' from student;
select count(*) from student;   #统计表中总数
select count(sex) from student;   #统计表中性别总数
```

* 若有一条数据中sex为空的话,就不予以统计
```
select sum(id) from student;
```

* 查询第i条以后到第j条的数据(不包括第i条)
```
select * from student limit 2,5;  #显示3-5条数据
```

* 修改数据
```
update student set name='test' where id=2;
update student set name='花花',sex='女' where id=2
delete from student where id=2;
```

* 修改表的名字
```
alter table tbl_name rename to new_name
alter table student rename to test_1;
```

* 向表中增加一个字段(列)
```
alter table tablename add columnname type;/alter table tablename add(columnname type);
alter table student add  age varchar(20) set default '1'; #set default 设置默认值
```

* 修改表中某个字段的名字
```
alter table tablename change columnname newcolumnname type;
```
* 修改一个表的字段名
```
alter table student change name test_name varchar(50);
```

* 去掉表中字段age的默认值
```
alter table student alter age drop default;
```

* 去掉表中字段age
```
alter table student drop column age;
```

* 删除表中主键
```
alter table student drop primary key;
```

* 表中增加主键
```
alter table add primary key (column1,column2,....,column)
alter table student add primary key (student_id);
```

* 用文本方式将数据装入数据库表中（例如D:/mysql.txt）
```
load data local infile "D:/mysql.txt" into table MYTABLE;
```

* 导入.sql文件命令（例如D:/mysql.sql）
```
source d:/mysql.sql;
/. d:/mysql.sql;
```

