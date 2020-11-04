#### 查询基础介绍

- 查询函数

~~~mysql
select VERSION();
~~~

- 起别名
  - 方式一：select last_name AS 姓 from student;
  - 方式二：select last_name 姓 from Student;
- 去重
  - select **DISTINCT** student_id from student;

- 加号的作用
  - mysql中加号只有一个功能：运算符

~~~mysql
select 100+90;  190
select '132'+30; 162
select 'sadasd'+30; 30    如果有字符试图转换成数值型，失败0
select null+10； 只要有null结果为null

select CONCAT(first_name,last_name) as "姓名";
select IFNULL(变量,替换值);
~~~



#### 进阶2：条件查询

- 分类

  - 一、按照条件表达式筛选

    - 条件运算符：>  <  =  !=  <>  >=  <=

  - 二、按照逻辑表达式筛选

    - 逻辑运算符： &&  ||  ！

      ​                        and  or  not

  - 三、模糊查询

    - like
    - between and
    - in
    - is null / is not null



IS NULL：仅仅可以判断 NULL 值，可读性比较高，**建议使用**

<=> （安全等于）：既可以判断 NULL 值，又可以判断普通的数值，可读性低



#### 进阶4：常见函数

- ##### 一、字符函数

  - 1. length 获取参数值的字节个数
    2.  concat 拼接字符串
    3.  upper、lower
    4.  substr（简写）、substring
    5.  instr 起始索引，找不到返回0
    6.  trim 去掉前后空格

    - TRIM( 'a' FROM 'aaaa张a三aaaa‘ ) 结果 张a三

    7.  lpad 用指定字符实现左填充
    8.  rpad 右填充
    9.  replace 替换

- ##### 二、数学函数

  - 1.  round 四舍五入
    2.  ceil 向上取整
    3.  floor 向下取整
    4.  truncate 截断
    5.  mod 取余

- ##### 三、日期函数

  - 1.  now 返回当前系统日期 + 时间
    2.  curdate 返回当前系统日期，不包含时间
    3.  curtime 返回当前时间，不包含日期
    4.  YEAR( NOW() ) 年 MONTH MONTHNAME 
    5.  str_to_date
    6. date_format 将日期格式化成字符串

- ##### 四、其他函数

  - 1.  VERSION
    2.  DATABASES
    3.  USER

- ##### 五、流程工作函数

  - 1.  if 函数 ： if else 的效果

    2.  case 函数 ： switch case 的效果

       - case 要判断的字段或表达式

         when 常量1 then 要显示的值1或语句1

         when 常量2 then 要显示的值2或语句2

         ...

         else 要显示的值 n 或语句 n；

         end

~~~mysql
SELECT salary 原始工资，department_id，
CASE department_id
WHEN 30 THEN salary*1.1
WHEN 40 THEN salary*1.2
WHEN 50 THEN salary*1.3
END AS 新工资
FROM employees;
~~~

​					3. CASE 的 if else 效果

~~~mysql
SELECT salary 原始工资，department_id，
CASE 
WHEN salary>20000 THEN 'A'
WHEN salary>12000 THEN 'B'
WHEN salary>1000 THEN 'C'
END AS 工资级别
FROM employees;
~~~





#### 进阶5：分组

#### 分组函数

- 1. 分类 sum 求和、avg 平均值、max 最大值、 min 最小值、 count 计算个数

  2. 参数支持哪些类型

     sum，avg一般用于数值型

     max，min，count 可以处理任何类

  3. 是否忽略 null 值

     以上都忽略 null 值 

  4. 和 distince 搭配

- 效率

  ​	MYISAM 引擎下，count(*) 的效率高

  ​	INNODB 引擎下，coun(*) 和 count(1) 的效率差不多，比 count(字段) 效率要高

  ​	

  ​	和分组函数用查询的字段要求是 group by  后的字段



#### 分组查询

- 语法：

  ​		select 分组函数，列（要求出现在 group by 的后面）

  ​		from 表

  ​		【 where 筛选条件】

  ​		group by 分组的列表

  ​		【order by 子句】

- 分组查询中的筛选条件分为两类

  ​	

|            | 数据源         | 位置        | 关键字 |
| ---------- | -------------- | ----------- | ------ |
| 分组前筛选 | 原始表         | group by 前 | where  |
| 分组后筛选 | 分组后的结果集 | group by 后 | having |

分组函数做条件肯定是放在having子句中



#### 进阶6：连接查询

- ##### 含义：又称多表查询，当查询的字段来自于多个表时，就会用到连接查询

​		笛卡尔乘积出现的原因：没有有效的的链接条件

- ##### 分类

  - 按功能分类

    - 内连接：

      ​				等值连接（条件用等号连接）

      ​				非等值连接

      ​				自连接

    - 外连接：

      ​				左外连接

      ​				右外连接

      ​				全外连接

    - 交叉连接



- ##### 语法：

  -  select 查询列表

    from 表1 别名 【连接类型】（内连接 inner ，左外 left 【outer】，右外 right【outer】 ，			全外 full 【outer】 ，交叉 cross（笛卡尔积））

    join   表2 别名

    on    连接条件

    【where 筛选条件】

    【group by 分组】

    【having 筛选条件】

    【order by 排序列表】





#### 进阶7：子查询

- ##### 分类：

  - 按子查询出现的位置：

    - select后面：

      ​	仅仅支持标量子查询

    - from后面：

      ​	支持表子查询

    - where或having后面

      ​	标量子查询

      ​	列子查询

      ​	行子查询

    - exists后面（相关子查询）

      ​	表子查询

      ​	exists(完整的查询语句)

  - 按结果集的行列数不同：

    - 标量子查询（结果集只有一行一列）
    - 列子查询（结果集只有一列多行）
    - 行子查询（结果集有一行多列）
    - 表子查询（结果集一般为多行多列）



列子查询，一般搭配着多行操作符使用

in、any/some、all



#### 进阶8：分页查询

语法

​		select 查询列表

​		from 表

​		【join type join 表2 

​			on 连接条件

​			where 筛选条件

​			group by 分组字段

​			having 分组后的筛选

​			order by 排序的字段】

​		limit 【offset,】size;

​		offset 起始索引 从0开始

​		size 显示条目个数



放在查询语句最后



#### 进阶9：联合查询

语法：

​		查询语句1	

​		union

​		查询语句2

​		union

​		.....



##### 应用场景：

要查询的结果来自多个表，且多个表没有连接关系，但查询的信息一致时



特点：

1. 要求多条查询的语句列数一致
2. 要求多条查询语句的每一列的类型顺序最好一致
3. union关键字默认去重，使用union all 可以包含去重

