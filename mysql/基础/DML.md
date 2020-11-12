### 插入语句

#### 方式一

语法：

​		insert into 表名（列明，......）values (值1，.....)

- 插入的值要与列的类型一致或兼容

- 不可以为null的列必须插入值，可以为null的列如何插入值？

  ​	方式一：insert into student(id,name) values(1,null);

  ​	方式二：insert into student(id) values(1);

- 列的顺序可以调换

- 列数和值数必须一致

- 可以省略列名，默认所有列，而且列的顺序和表中列的顺序一致

  ​	insert into student values(1,'张三');



#### 方式二：

语法：

​		insert into 表名

​		set 列名 = 值，列名 = 值，....





### 修改语句

1.修改单表的记录

语法：

​		update 表名

​		set 列 = 新值，列 = 新值，.....

​		where 筛选条件



2.修改多表的记录【补充】

语法：

​		sql92语法：

​			update 表1 别名，表2 别名

​			set  列 = 值，...

​			where 连接条件

​			and 筛选条件；

​		

​		sql99 语法

​			update 表1 别名

​			inner|left|right	join 表2 别名

​			on	连接条件

​			set	列 = 值 ....

​			where 筛选条件





### 删除语句

方式一：delete

​	语法：

​		1.单表删除

​			delete from 表名 where 筛选条件

​		2.多表删除

​			sql92语法

​			delete 表1的别名，表2的别名

​			from 表1 别名，表2 别名

​			where 连接条件

​			and 筛选条件；



​		sql99语法：

​			delete 表1的别名，表2的别名

​			from 表1 别名

​			inner|left|right join 表2 别名 on 连接条件

​			where 筛选条件；



方式二：truncate

​	语法：

​		truncate table 表名；



​		truncate删除不能回滚，delete删除可以回滚



























