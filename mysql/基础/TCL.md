ACID

1. 原子性（Atomicity）

   原子性是指事物不可分割的工作单位，事物中的操作要么都发生，要么都不发生。

2. 一致性（Consistency）

   事物必须使数据库从一个一致性状态变换到另外一个一致性状态

3. 隔离性（Isolation）

   事物的隔离性是指一个事务的执行不能被其他事务干扰，即一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的个个事务之间不能相互干扰

4. 持久性（Durability）

   持久性是指一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来的其他操作和数据库故障不应该对其有任何影响



事务的创建

隐式事务：事务没有明显的开启和结束语句

比如insert、update、delete语句



步骤1：开启事务

set autocommit=0；

start transaction；可选的

步骤2：编写事务中的sql语句（select insert update delete）

语句1；

语句2；

......

步骤3：结束事务

commit；提交事务

rollback；回滚事务



脏读：读取了没有被提交的字段，后又回滚

不可重复读：读取后，字段更新了，被提交，又读，两次读取的值不一样

幻读：插入了新的行后第一次和第二次行数不一样 



read uncommitted 读未提交

~~~mysql
1.事务A 开启事务，对数据进行修改成111，事务B开启事务查询数据得到111
2.事务A 回滚，事务B再读读到999
~~~

read committed 不可重复读

~~~mysql
1.事务 A 开启事务，对数据进行修改原来999改成111，事务B开启事务，读取得到999
2.事务 A 提交，事务B又读得到111
~~~

repeatable read 可重复读

~~~mysql
1.事务A开启事务，事务B开启事务，事务B查询，事务A增加或删除数据
2.事务B查询到多或少的数据
~~~

serializable 串行化 全部解决



savepoint

~~~mysql
set autocommit = 0;
start transaction;
delete from teacher where id = 100;
savepoint a;#设置保存点
delete from teacher where id = 99;
rollback to a;#回滚到保存点，只有id=100被删除了

~~~











