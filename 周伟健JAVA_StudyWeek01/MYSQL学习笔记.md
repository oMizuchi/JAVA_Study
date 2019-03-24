 

定点数类型
=====
decimal(M,D)

M：表示数据的总长度（不包括小数点）
D：表示小数位长度

例如：decimal(5,2)     123.45
存入数据时按四舍五入计算


----------


日期与时间类型
=======
|类型      | 字节数   |  零值  |
| --------   | -----:  | :----:  |
| year    | 1|   0000    |
|date	 |4	|0000-00-00
|time|	 3|	00:00:00
|datetime	| 8|	0000-00-00 00:00:00
|timestamp |4|	00000000000000(19700101080001)


----------


字符串类型
=====
|类型|	说明      |
| --------   | -----:  | :----:  |
|char|	固定长度字符串（如char(10),不管实际需要多少，一律分配10个字节）
|varchar|	可变长度字符串（如varchar(10),按实际需要自动分配内存）
|text|	大文本
|enum|	枚举类型（只取一个元素）
|set	|集合类型（能取多个元素）


----------

二进制类型
=====
|类型	|	说明
| --------   | -----:  | :----:  |
|binary(M)	|	字节数为M，允许长度为0~M的定长二进制字符串
|varbinary(M)|	字节数为值的长度加1
|bit(M)	|	M位二进制数据，最多255各字节
|tinyblob	|	可变长二进制数，最多255个字节
|blob	|	可变长二进制数，最多2^16-1个字节
|miediumblob|	可变长二进制数，最多2^24-1个字节
|longblob	|	可变长二进制数，最多2^32-1个字节


----------


数据库
===

数据库：是按照数据结构来组织，存储和管理数据的仓库

 1. show databases：显示所有数据库
 
 2. create database+数据库名：创建数据库

 3. drop database+数据库名：删除数据库


----------


表
=
表示数据库存储数据的基本单位，一个表包含若干字段或记录

 - 语法:

create table 表名(属性名 数据类型 [完整性约束条件]，
	           属性名 数据类型 [完整性约束条件]，
		...
	           属性名 数据类型 [完整性约束条件]，
	          );


----------


	         
|约束条件		|	说明
| --------   | -----:  | :----:  |
|primary key	|标识该属性为该表的主键，可以唯一的标识对应的记录
|poreign key|	标识该属性为该表的外键，与某表的主键关联
|noy null	|	标识该属性不能为空
|unique	|	标识该属性的值是唯一的
|auto_increment	|标识该属性的值自动增加
|default	|	为该属性设置默认值


----------


 - 实例：创建一个图书类别表： bookType（父表）

 CREATE TABLE bookType(
	id int primary key auto_increment,
	bookTypeName varchar(20),
	bookTypeDasc varchar(200)
           );


----------


 - 创建图书表：book（子表）

CREATE TABLE book(
  	id int primary key auto_increment,
	bookName varchar(20),
	author varchar(10),
	price decimal(6,2),
	//把book作为外键关联bookType主键，以约束性质，确保数据一致性（类似于extends）
	constraint `fk` foreign key (`bookTypeId`)references `bookType`(`id`)
	//fk为键名		bookTypeId：当前表作为外键的列名		references：引用
		bookType：要引用表的名称	id：要引用的字段
	);


----------


查看表结构
=====

 1. 查看基本表结构： describe(或desc)+表名;
 
 2. 查看表详细结构： show create tabke+表名；
 


----------

修改表
===

 1. 修改表名		alter table 旧表名 rename 新表名
 
 2. 修改字段		alter table 表名 change 旧属性名 新属性名 新数据类型

 3. 增加字段		alter table 表名 add 属性名1 数据类型 [完整性约束条件] [front/after 属性名2] 
    //first 将字段置于首位，后不用加字段

 4. 删除字段		alter table 表名 drop 属性名


----------

删除表
===
drop table+表名 	//有子表存在的父表不能删除


----------

给属性加具体的元素值
==========
insert into `表名` (`属性1`,`属性2`,`属性3`) values (`属性1对应的元素值1`,`元素值2`,`元素值3`);


----------

一.单表查询
======

 

 1.查询所有字段
---------

 2. select 字段1,字段2,字段3 from 表名;	
//根据字段顺序查询结果顺序也不同

 3. select * from 表名;
 

----------


2.查询指定字段
--------
select 字段1,字段2,字段3 from 表名;


----------

3.Where条件查询
-----------
select 字段1,字段2,字段3 from 表名 where 条件表达式;

 - //如：select * from book where price<20;

----------

4.带IN关键字查询
----------
select 字段1,字段2,字段3 from 表名 where 字段 [not] in (元素1,元素2,元素3); 

 - //如：select * from book where author in (zwj,zjm);
//查找作者为zwj,zjm的书籍的所有属性


----------

5.带between and 的范围查询
--------------------
select 字段1,字段2,字段3 from 表名 where 字段 [not] between 取值1 and 取值2;

 - //如：select * from book where price between 20 and 50;
 //查找价格在20到50间的书籍的所有属性
 


----------

6.带like的模糊查询
------------

 - select 字段1,字段2,字段3 from 表名 where 字段 [not] like `字符串`, 
 “%”代表任意字符
   “_”代表单个字符 
   //如：select * from book where bookName like `你好%`;	
   //查找结果可为书名为 你好鸭，你好帅鸭，你好吗 等
   
  
 - //如：select * from book where bookName like `你好_`;	 //查找结果可为书名为
   你好鸭，你好吗 等（为三个字符）
   
   
  
 - //如：select * from book where bookName like `%你好%`;	 //查询结果的书名中只要含有 你好 即可


----------


7.空值查询
------
select 字段1,字段2,字段3 from 表名 where 字段 is [not] null;

 - //如：select * from book where author is null;
//查询作者名未知的书籍信息


----------

8.带AND的多条件查询
------------
select 字段1,字段2 from 表名 where 条件表达式1 and 条件表达式2 [and 条件表达式n];

 - //如：select *from book where author=`zwj` and price < 20;
//查询作者为zwj且价格低于20的书籍信息


----------

9.带OR的多条件查询
-----------
select 字段1,字段2 from 表名 where 条件表达式2 or 条件表达式2 [or 条件表达是n];

 - //如：select *from book where author=`zwj` or price < 20;
//查询作者为zwj或价格低于20的书籍信息


----------

10.查询所有不重复字段
------------
select distinct 字段名 from 表名;

 - //如：select distinct author from book;
//查询所有作者名，不重复


----------

11.ORDER BY对查询结果排序
------------------

 - select 字段1,字段2 from 表名 order by 属性名 [asc/desc];	
 //默认为asc，升序；desc,降序
 
 
 - //如：select * from book order by id asc;	           
 //按书籍id升序的顺序排序

----------

12.GROUP BY分组查询
---------------
select 字段1,字段2 from 表名 group by 属性名 [having 条件表达式] [with rollup];

 - 与group_concat()函数一起使用 
 //如：select author,group_concat(bookName) from book group by author; 
   //排列出每一位作者的所有书籍名
   
 - 与聚合函数一起使用 
 //如：select author,count(bookName) from book group by  author; 
   //列出每一位作者的所有书籍数
   
 - 与having一起使用（限制输出的结果）
 //如：select author,count(bookName) from book   group by author having count(bookName)>3; 
   //列出每一位作者的书籍名大于3个字的书籍数
   
 - 与with rollup一起使用（最后加入一个总和行） 
//如：select author,count(bookName) from  book group by author with rollup; 
//在最后另加一行列出所有书籍名，若为数字则统计相加

----------

 13.limit分页查询
-------------
select 字段1,字段2 from 表名 limit 初始位置,记录数;

 - //如 ：select * from book limit 0,5;
//查询id从1到5的书籍所有属性

 - //如 ：select * from book limit 5,5;
//查询id从6到10的书籍所有属性


----------

第二节：使用聚合函数查询
============

1.count()函数
-----------
1.count()函数用来统计记录的条数
 

 - //如：select count(*) from book;
//统计出表中所有元素总个数，默认名为count(*)
 
 - //如：select count(*) as total from book;
//统计出表中所有元素总个数，并命名为total
	 


----------
2.与group by 关键字一起使用

 - //如：select author,count(*) from book group by bookName;
//统计出每个作者对应的书名数，即出版的书籍数


----------

2.sum()函数
---------
1)sum()函数是求和函数

 - //如：select author,sum(price) from book while author="zwj";
 //计算作者为zwj的所有书籍的价格总和


----------


2)与group by关键字一起使用

 - //如：select author,sum(price) from book group by author;
 //分别计算不同作者的所有书籍的价格总和


----------

3.avg()函数
---------
1)avg()函数是求平均值的函数

 - //如：select author,avg(price) from book while author="zwj";
 //计算作者为zwj的所有书籍的平均价格


----------
2)与group by关键字一起使用

 - //如：select author,sum(price) from book group by author;
 //分别计算不同作者的所有书籍的平均价格


----------

4.max()函数
---------
1)max()函数是求最大值的函数

 - //如：select author,max(price) from book while author="zwj";
 //计算作者为zwj的所有书籍中的最高价格


----------
2)与group by关键字一起使用

 - //如：select author,max(price) from book group by author;
//分别计算不同作者的所有书籍中的最高价格	


----------

5.min函数
-------
1)min()函数是求最小值的函数

 - //如：select author,min(price) from book while author="zwj";
 //计算作者为zwj的所有书籍中的最低价格


----------
2)与group by关键字一起使用

 - //如：select author,min(price) from book group by author;
 //分别计算不同作者的所有书籍中的最低价格


----------

第三节:连接查询
========
连接查询是将两个或两个以上的表按照某个条件连接起来，从中选取需要的元素

1.内连接查询
-------
内连接查询是一种最常用的连接查询，内连接查询可以查询两个或两个以上的表

 - //如：select * from book1,book2 where book1.id1 = book2.id2;
 //查询表一book1中的属性id1与表二book2中的属性id2相同的元素并列出

 - //如：select b1.bookName,b1.author,b2.price from book1 b1,book2 b2  where b1.id1 = b2.id2;
 //用别名的方式便于查看


----------

2.外连接查询
-------
外连接可以查看某一张表的所有信息

select 属性名列表 from 表名1 left/right join 表名2 on 表名1.属性1 = 表名2.属性2


----------


1)左连接查询

可以查询出“表名一”的所有记录，而“表名二”中，只能查询出匹配的记录

 - //如：select * from book1 left join book2 on book1.id1 = book2.id2;
 //列出book1的所有元素并在其后追加book2中id2与id1相同的元素，没有则显示空


----------
2)右连接查询

可以查询出“表名二”的所有记录，而“表名二”中，只能查询出匹配的记录

 - //如：select b1.bookName,b1.author,b2.price from book1 b1 left join book2 b2 on b1.id1 =b2.id2;
 //用别名的方式便于查看


----------

3.多条件连接查询
---------

 - //如：select b1.bookName,b1.author,b2.price from book1 b1,book2 b2 where b1.id1 = b2.id2 and b1.price > 50;
 //查询表一book1中的属性id1与表二book2中的属性id2相同且book1中价格大于70的元素并列出


----------

第四节：子查询
=======

1.带in关键字的子查询
------------

一个查询语句的条件可能落在另一个select语句的查询结果中

 - //如：select * from book1 where id1 in (select id2 from book2);
 //查询出book1中id1范围在book2中id2的所有元素


----------

2.带比较运算符的子查询
------------
子查询可以使用比较运算符

 - //如：select * from book1 where price1 >=(select price3 from
   priceleve(表名) where price3 = 80);
//查询book1中价格大于或等于priceleve中price2等于80的元素，即book1中价格大于80的元素，括号内必须为一个值而不是一个集合


----------

3.带exists关键字的子查询
----------------
假如子查询查询到记录，则进行外层查询，否则，不执行外层查询

 - //如：select * from book1 where exists(select * from book2);
 //若book2中有元素存在则列出book1中的所有元素，book2中没有元素则返回空
 


----------

4.带any关键字的子查询
-------------
any关键字表示满足其中任一条件

 - //如：select * from book1 where price1 >= any(select price3 from priceleve);
//查询book1中price1大于或等于priceleve中任意price3值的元素


----------

5.带all关键字的子查询
-------------
all关键字表示满足所有条件

 - //如：select * from book1 where price1 >= all(select price3 from  priceleve);
//查询book1中price1大于或等于priceleve所有price3值的元素


----------

第五节：合并查询结果
==========

1.union
-------
带union关键是数据库系统会将所有的查询结果合并在一起，然后去除相同的记录

 - //如：select id1 from book1 union select id2 from book2;
 //查询合并book1与book2中的id属性，并去除book1中id1与book2中id2 相同的属性


----------

2.union all
-----------
使用union all，不会去除系统的记录

 - //如：select id1 from book1 union all select id2 from book2;
 //查询合并book1与book2中的id属性


----------

第六节：为表的字段取别名
============

1.为表取别名
-------
格式：表名 表的别名

 - //如：select * from book1 b1 where b1.id1=1001;
 


----------

 

2.为字段取别名
----------
格式：属性名 [as] 别名

 - //如：select author1 a1 from book1 b1 where b1.id1=1001;
 
 
 - //如：select author1 as a1 from book1 b1 where b1.id1=1001;

----------

第六章
===

第一节：插入数据
========

1.给表的所有字段插入数据
-------------
格式：insert into 表名 values (值1，值2...);

 - //如：insert into book values(1001,`bookName`,`zwj`,80);
 //依次按创建表的属性顺序给表插入数据，若没有则写null


----------

2.给表的指定字段插入数据
-------------
格式：insert into 表名（属性1,属性2...) values (值1，值2...);

 - //如：inserrt into book(bookName,author) values(`bookName`,`zwj`);
 //给特定字段插入数据，没有的则默认为空


----------

3.同时插入多条记录
----------
insert into 表名[(属性列表)] values(取值列表1),(取值列表2)...

 - //如：inserrt into book(bookName,author) values(`bookName`,`zwj`),(`bookName2`,`zjm`);


----------

第二节：更新数据
========
update 表名 set 属性名1=取值1,属性名2=取值2...where 条件表达式

 - //如：update book set author=zjm,price=100 where id=1001;
//修改id为1中的作者，价格属性值

 - //如：update book set bookName=`你好` where bookName like `%你好%`;
 //修改书名中含有“你好”的属性为“你好”


----------

第三节：删除数据
========
delete from 表名 [where 条件表达式]

 - //delete from book where id=1002;
 //删除book中id为1002的那一列元素


----------

第七章
===

第一节：索引引入
========
索引定义：索引是由数据库表中一列或者多列组合而成，其作用是提高对表中数据的查询速	类似于图书的目录，方便快速定位，寻找指定的内容


----------

第二节：索引的优缺点
==========

 - 优点：提高查询数据的速度
 
 - 缺点：创建和维护索引的时间增加了，即改变书籍内容，目录也要跟着改
 
 - 右键表点击管理索引新建即可



