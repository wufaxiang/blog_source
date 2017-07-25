---
title: MySQL Alter语句 运用
date: 2015-04-17 11:57:12
tags: 
  - sql
  - mysql
category: sql
---

修改表名

	mysql> alter table student rename person;

这里的student是原名，person是修改过后的名字

用rename来重命名，也可以使用rename to

修改字段的数据类型

	mysql> alter table person modify name varchar(20);

此处modify后面的name为字段名，我们将原来的varchar(25)改为varchar(20)

修改字段名

	mysql> alter table person change stu_name name varchar(25);

这里stu_name是原名，name是新名
<!-- more -->
需要注意的是不管改不改数据类型，后面的数据类型都要写

如果不修改数据类型只需写成原来的数据类型即可

tips：我们同样可以使用change来达到modify的效果，只需在其后写一样的字段名

增加无完整性约束条件的字段

	mysql> alter table person add sex boolean;

此处的sex后面只跟了数据类型，而没有完整性约束条件

增加有完整性约束条件的字段

	mysql> alter table person add age int not null;

地处增加了一条age字段，接着在后面加上了not null完整性约束条件

增加多个字段

	mysql> alter table person add num int not null,username varchar(10),scode int not null

在表头添加字段

	mysql> alter table person add num int primary key first;

默认情况下添加字段都是添加到表尾，在添加语句后面加上first就能添加到表头

在指定位置添加字段

	mysql> alter table person add birth date after name;

这里添加一条新字段放在name字段后面

tps：表中字段的排序对表不会有什么影响，不过更合理的排序能便于理解表

删除字段

	mysql> alter table person drop sex;

和前面删除表或数据库一样，这里也需要用drop

不同的是，删除字段还要用alter table跟着表名

修改字段到第一个位置

	mysql> alter table person modify id int first;

first在前面已经讲过，此处要注意的是字段后面要写数据类型

修改字段到指定位置

	mysql> alter table person modify name varchar(25) after id;

我们把name字段放到了id后面，此处的varchar(25)要写全，varchar不行，建议操作以上步骤之前都先desc table

修改表的存储引擎

	mysql> alter table user rename person;

这里先不具体讲各个存储引擎的特点，内容比较多

修改完之后别忘了使用show create table语句查看，第三节有写用法

	tips：如果表中已存在很多数据，不要轻易修改存储引擎

增加表的外键

	mysql> alter table score add constraint fk foreign key(stu_id) references student(id);

这里只需使用add增加即可

删除表的外键约束

	mysql> alter table student3 drop foreign key fk;

由于基本的表结构描述无法显示外键，所以在进行此操作前最好使用show create table查看表

这里的fk就是刚刚设置的外键

需要注意的是：如果想要删除有关联的表，那么必先删除外键

删除外键后，原先的key变成普通键

如果创建表的时候没有设置外键，可使用上面的方法

