---
title: Sql Server Alter语句 运用
date: 2015-04-17 11:56:12
tags: 
  - sql
  - sql server
category: sql
---

在修改  Sql Server  表结构时，常用到  Alter  语句，把一些常用的  alter  语句列举如下。

1  ：向表中添加字段

	Alter table [  表名  ] add [  列名  ]  类型

2:  删除字段

	Alter table [  表名  ]  drop column [  列名  ]

3:  修改表中字段类型（可以修改列的类型，是否为空）

	Alter table [  表名  ] alter column [  列名  ]  类型

4  ：添加主键

	Alter table [  表名  ] add constraint [  约束名  ] primary key( [  列名  ])
<!-- more -->
5  ：添加唯一约束

	Alter table [  表名  ] add constraint [  约束名  ] unique([  列名  ])

6  ：添加表中某列的默认值

	Alter table [  表名  ] add constraint [  约束名  ] default(  默认值  ) for [  列名  ]  
7  ：添加约束

	Alter table [  表名  ] add constraint [  约束名  ] check (  内容  )

8:  添加外键约束

	Alter table [  表名  ] add constraint [  约束名  ]  foreign key(  列名  ) referencese
另一表名  (  列名  )

9:  删除约束

	Alter table [  表名  ] drop constraint [  约束名  ]

10:  重命名表

	exec sp_rename '[  原表名  ]','[  新表名  ]'

11  ：重命名列名

	exec sp_rename '[  表名  ].[  列名  ]','[  表名  ].[  新列名  ]'

创建注释（  N'user', N'dbo', N'TABLE'  为固定的写法）

12  ：为表添加描述信息  
	EXECUTE sp_addextendedpropertyN'MS_Description', '  人员信息表  ', N'user', N'dbo',N'TABLE', N'  表名  ', NULL, NULL

13  ：为字段  Username  添加描述信息  
	EXECUTE sp_addextendedpropertyN'MS_Description', '  姓名  ', N'user', N'dbo',N'TABLE', N'  表名  ', N'column', N'Username'

14  ：为字段  Sex  添加描述信息  
	EXECUTE sp_addextendedpropertyN'MS_Description', '  性别  ', N'user', N'dbo',N'TABLE', N'  表名  ', N'column', N'Sex'

15  ：更新表中列  UserName  的描述属性：  
	EXEC sp_updateextendedproperty 'MS_Description','  新的姓名','user',dbo,'TABLE','  表名  ','column','UserName'

16  ：删除表中列  UserName  的描述属性：  
	EXEC sp_dropextendedproperty 'MS_Description','user',dbo,'TABLE','  表名','column','Username'

