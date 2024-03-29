# 本单元目标

	一、为什么要学习数据库
	二、数据库的相关概念      
		DBMS、DB、SQL
	三、数据库存储数据的特点
	四、初始MySQL
		MySQL产品的介绍        
		MySQL产品的安装          ★        
		MySQL服务的启动和停止     ★
		MySQL服务的登录和退出     ★      
		MySQL的常见命令和语法规范      
	五、DQL语言的学习   ★              
		基础查询        ★             
		条件查询  	   ★			
		排序查询  	   ★				
		常见函数        ★               
		分组函数        ★              
		分组查询		   ★			
		连接查询	 	★			
		子查询       √                  
		分页查询       ★              
		union联合查询	√			
		
	六、DML语言的学习    ★             
		插入语句						
		修改语句						
		删除语句						
	七、DDL语言的学习  
		库和表的管理	 √				
		常见数据类型介绍  √          
		常见约束  	  √			
	八、TCL语言的学习
		事务和事务处理                 
	九、视图的讲解           √
	十、变量                      
	十一、存储过程和函数   
	十二、流程控制结构       

# 一、数据库的好处

	1.持久化数据到本地
	2.可以实现结构化查询，方便管理



# 二、数据库相关概念

	1、DB：数据库，保存一组有组织的数据的容器
	2、DBMS：数据库管理系统，又称为数据库软件（产品），用于管理DB中的数据
	3、SQL:结构化查询语言，用于和DBMS通信的语言

# 三、数据库存储数据的特点

	1、将数据放到表中，表再放到库中
	2、一个数据库中可以有多个表，每个表都有一个的名字，用来标识自己。表名具有唯一性。
	3、表具有一些特性，这些特性定义了数据在表中如何存储，类似java中 “类”的设计。
	4、表由列组成，我们也称为字段。所有表都是由一个或多个列组成的，每一列类似java 中的”属性”
	5、表中的数据是按行存储的，每一行类似于java中的“对象”。



# 四、MySQL产品的介绍和安装

### 1、MySQL服务的启动和停止

​	方式一：计算机——右击管理——服务
​	方式二：通过管理员身份运行
​	net start 服务名（启动服务）
​	net stop 服务名（停止服务）

### 2、MySQL服务的登录和退出   

​	方式一：通过mysql自带的客户端
​	只限于root用户
​	

	方式二：通过windows自带的客户端
	登录：
	mysql 【-h主机名 -P端口号 】-u用户名 -p密码
	
	退出：
	exit或ctrl+C

​	
​	
​	

### 3、MySQL的常见命令 

	1.查看当前所有的数据库
	show databases;
	2.打开指定的库
	use 库名
	3.查看当前库的所有表
	show tables;
	4.查看其它库的所有表
	show tables from 库名;
	5.创建表
	create table 表名(
	
		列名 列类型,
		列名 列类型，
		。。。
	);
	6.查看表结构
	desc 表名;


	7.查看服务器的版本
	方式一：登录到mysql服务端
	select version();
	方式二：没有登录到mysql服务端
	mysql --version
	或
	mysql --V



### 4、MySQL的语法规范

​	1.不区分大小写,但建议关键字大写，表名、列名小写
​	2.每条命令最好用分号结尾
​	3.每条命令根据需要，可以进行缩进 或换行
​	4.注释
​		单行注释：#注释文字
​		单行注释：-- 注释文字
​		多行注释：/* 注释文字  */


​	
​	

### 5、SQL的语言分类

​	DQL（Data Query Language）：数据查询语言
​		select 
​	DML(Data Manipulate Language):数据操作语言
​		insert 、update、delete
​	DDL（Data Define Languge）：数据定义语言
​		create、drop、alter
​	TCL（Transaction Control Language）：事务控制语言
​		commit、rollback



### 6、SQL的常见命令

	show databases； 查看所有的数据库
	use 库名； 打开指定 的库
	show tables ; 显示库中的所有表
	show tables from 库名;显示指定库中的所有表
	create table 表名(
		字段名 字段类型,	
		字段名 字段类型
	); 创建表
	
	desc 表名; 查看指定表的结构
	select * from 表名;显示表中的所有数据



# 五、DQL语言的学习

### 进阶1：基础查询

​	语法：
​	SELECT 要查询的东西
​	【FROM 表名】;
​	

	类似于Java中 :System.out.println(要打印的东西);
	特点：
	①通过select查询完的结果 ，是一个虚拟的表格，不是真实存在
	② 要查询的东西 可以是常量值、可以是表达式、可以是字段、可以是函数

### 进阶2：条件查询

​	条件查询：根据条件过滤原始表的数据，查询到想要的数据
​	语法：
​	select 
​		要查询的字段|表达式|常量值|函数
​	from 
​		表
​	where 
​		条件 ;
​	

	分类：
	一、条件表达式
		示例：salary>10000
		条件运算符：
		> < >= <= = != <>
	
	二、逻辑表达式
	示例：salary>10000 && salary<20000
	
	逻辑运算符：
	
		and（&&）:两个条件如果同时成立，结果为true，否则为false
		or(||)：两个条件只要有一个成立，结果为true，否则为false
		not(!)：如果条件成立，则not后为false，否则为true
	
	三、模糊查询
	示例：last_name like 'a%'

### 进阶3：排序查询	

	语法：
	select
		要查询的东西
	from
		表
	where 
		条件
	
	order by 排序的字段|表达式|函数|别名 【asc|desc】

​	

### 进阶4：常见函数

​	一、单行函数
​	1、字符函数
​		concat拼接
​		substr截取子串
​		upper转换成大写
​		lower转换成小写
​		trim去前后指定的空格和字符
​		ltrim去左边空格
​		rtrim去右边空格
​		replace替换
​		lpad左填充
​		rpad右填充
​		instr返回子串第一次出现的索引
​		length 获取字节个数
​		
​	2、数学函数
​		round 四舍五入
​		rand 随机数
​		floor向下取整
​		ceil向上取整
​		mod取余
​		truncate截断
​	3、日期函数
​		now当前系统日期+时间
​		curdate当前系统日期
​		curtime当前系统时间
​		str_to_date 将字符转换成日期
​		date_format将日期转换成字符
​	4、流程控制函数
​		if 处理双分支
​		case语句 处理多分支
​			情况1：处理等值判断
​			情况2：处理条件判断
​		

	5、其他函数
		version版本
		database当前库
		user当前连接用户


​	


二、分组函数


		sum 求和
		max 最大值
		min 最小值
		avg 平均值
		count 计数
	
		特点：
		1、以上五个分组函数都忽略null值，除了count(*)
		2、sum和avg一般用于处理数值型
			max、min、count可以处理任何数据类型
	    3、都可以搭配distinct使用，用于统计去重后的结果
		4、count的参数可以支持：
			字段、*、常量值，一般放1
	
		   建议使用 count(*)

### 进阶5：分组查询

​	语法：
​	select 查询的字段，分组函数
​	from 表
​	group by 分组的字段


​	特点：
​	1、可以按单个字段分组
​	2、和分组函数一同查询的字段最好是分组后的字段
​	3、分组筛选
​			针对的表	位置			关键字
​	分组前筛选：	原始表		group by的前面		where
​	分组后筛选：	分组后的结果集	group by的后面		having
​	
​	4、可以按多个字段分组，字段之间用逗号隔开
​	5、可以支持排序
​	6、having后可以支持别名

### 进阶6：多表连接查询

	笛卡尔乘积：如果连接条件省略或无效则会出现
	解决办法：添加上连接条件

一、传统模式下的连接 ：等值连接——非等值连接


	1.等值连接的结果 = 多个表的交集
	2.n表连接，至少需要n-1个连接条件
	3.多个表不分主次，没有顺序要求
	4.一般为表起别名，提高阅读性和性能

二、sql99语法：通过join关键字实现连接

	含义：1999年推出的sql语法
	支持：
	等值连接、非等值连接 （内连接）
	外连接
	交叉连接（笛卡尔积）
	
	语法：
	
	select 字段，...
	from 表1
	【inner|left outer|right outer|cross】join 表2 on  连接条件
	【inner|left outer|right outer|cross】join 表3 on  连接条件
	【where 筛选条件】
	【group by 分组字段】
	【having 分组后的筛选条件】
	【order by 排序的字段或表达式】
	
	好处：语句上，连接条件和筛选条件实现了分离，简洁明了！


​	
三、自连接

案例：查询员工名和直接上级的名称

sql99

	SELECT e.last_name,m.last_name
	FROM employees e
	JOIN employees m ON e.`manager_id`=m.`employee_id`;

sql92


	SELECT e.last_name,m.last_name
	FROM employees e,employees m 
	WHERE e.`manager_id`=m.`employee_id`;

### 进阶7：子查询

含义：

	一条查询语句中又嵌套了另一条完整的select语句，其中被嵌套的select语句，称为子查询或内查询
	在外面的查询语句，称为主查询或外查询

特点：

	1、子查询都放在小括号内
	2、子查询可以放在from后面、select后面、where后面、having后面，但一般放在条件的右侧
	3、子查询优先于主查询执行，主查询使用了子查询的执行结果
	4、子查询根据查询结果的行数不同分为以下两类：
	① 单行子查询
		结果集只有一行
		一般搭配单行操作符使用：> < = <> >= <= 
		非法使用子查询的情况：
		a、子查询的结果为一组值
		b、子查询的结果为空
		
	② 多行子查询
		结果集有多行
		一般搭配多行操作符使用：any、all、in、not in
		in： 属于子查询结果中的任意一个就行
		any和all往往可以用其他查询代替

### 进阶8：分页查询

应用场景：

	实际的web项目中需要根据用户的需求提交对应的分页查询的sql语句

语法：

	select 字段|表达式,...
	from 表
	【where 条件】
	【group by 分组字段】
	【having 条件】
	【order by 排序的字段】
	limit 【起始的条目索引，】条目数;

特点：

	1.起始条目索引从0开始
	
	2.limit子句放在查询语句的最后
	
	3.公式：select * from  表 limit （page-1）*sizePerPage,sizePerPage
	假如:
	每页显示条目数sizePerPage
	要显示的页数 page

### 进阶9：联合查询

引入：
	union 联合、合并

语法：

	select 字段|常量|表达式|函数 【from 表】 【where 条件】 union 【all】
	select 字段|常量|表达式|函数 【from 表】 【where 条件】 union 【all】
	select 字段|常量|表达式|函数 【from 表】 【where 条件】 union  【all】
	.....
	select 字段|常量|表达式|函数 【from 表】 【where 条件】

特点：

	1、多条查询语句的查询的列数必须是一致的
	2、多条查询语句的查询的列的类型几乎相同
	3、union代表去重，union all代表不去重

### 进阶10：SQL语句执行顺序

- from：先确定从哪个表中取数据，所以最先执行from tab。存在多表连接，from tab1，tab2。可以对表加别名，方便后面的引用。
- where：where语句是对条件加以限定，如果没有需要限定的，那就写成where 1=1，表示总为true，无附加条件。
- group by…… having：分组语句，比如按照员工姓名分组，要就行分组的字段，必须出现在select中，否则就会报错。having是和group by配合使用的，用来作条件限定
- 聚合函数：常用的聚合函数有max，min， count，sum，聚合函数的执行在group by之后，having之前。如果在where中写聚合函数，就会出错。
- select：选出要查找的字段，如果全选可以select *。这里选出员工姓名，所有月份的总工资数。
- order by：排序语句，默认为升序排列。如果要降序排列，就写成order by [XX] desc。order by语句在最后执行，只有select选出要查找的字段，才能进行排序。

# 六、DML语言

### 插入

语法：
	insert into 表名(字段名，...)
	values(值1，...);

特点：

	1、字段类型和值类型一致或兼容，而且一一对应
	2、可以为空的字段，可以不用插入值，或用null填充
	3、不可以为空的字段，必须插入值
	4、字段个数和值的个数必须一致
	5、字段可以省略，但默认所有字段，并且顺序和表中的存储顺序一致

### 修改

修改单表语法：

	update 表名 set 字段=新值,字段=新值
	【where 条件】
修改多表语法：

	update 表1 别名1,表2 别名2
	set 字段=新值，字段=新值
	where 连接条件
	and 筛选条件

### 删除

方式1：delete语句 

单表的删除： ★
	delete from 表名 【where 筛选条件】

多表的删除：
	delete 别名1，别名2
	from 表1 别名1，表2 别名2
	where 连接条件
	and 筛选条件;


方式2：truncate语句

	truncate table 表名


两种方式的区别【面试题】
	
	#1.truncate不能加where条件，而delete可以加where条件
	
	#2.truncate的效率高一丢丢
	
	#3.truncate 删除带自增长的列的表后，如果再插入数据，数据从1开始
	#delete 删除带自增长列的表后，如果再插入数据，数据从上一次的断点处开始
	
	#4.truncate删除不能回滚，delete删除可以回滚

# 七、DDL语句

### 库和表的管理

库的管理：

	一、创建库
	create database 库名
	二、删除库
	drop database 库名
表的管理：

#### 1.创建表

```sql
CREATE TABLE IF NOT EXISTS stuinfo(
	stuId INT,
	stuName VARCHAR(20),
	gender CHAR,
	bornDate DATETIME
	);
	DESC stuinfo;
```

#### 2.修改表 alter

​	语法：ALTER TABLE 表名 ADD|MODIFY|DROP|CHANGE COLUMN 字段名 【字段类型】;
​	

##### ①修改字段名

​	ALTER TABLE studentinfo CHANGE  COLUMN sex gender CHAR;
​	

##### ②修改表名

​	ALTER TABLE stuinfo RENAME [TO]  studentinfo;

##### ③修改字段类型和列级约束

​	ALTER TABLE studentinfo MODIFY COLUMN borndate DATE ;
​	

##### ④添加字段

​	
​	ALTER TABLE studentinfo ADD COLUMN email VARCHAR(20) first;

##### ⑤删除字段

​	ALTER TABLE studentinfo DROP COLUMN email;

​	

#### 3.删除表

​	
​	DROP TABLE [IF EXISTS] studentinfo;


​	

### 常见类型

	整型：
		
	小数：
		浮点型
		定点型
	字符型：
	日期型：
	Blob类型：



### 常见约束

	NOT NULL
	DEFAULT
	UNIQUE
	CHECK
	PRIMARY KEY
	FOREIGN KEY

# 八、数据库事务

### 含义

​	通过一组逻辑操作单元（一组DML——sql语句），将数据从一种状态切换到另外一种状态

### 特点

​	（ACID）
​	原子性(Atomicity)：要么都执行，要么都回滚
​	一致性(Consistent)：保证数据的状态操作前和操作后保持一致
​	隔离性(Isolation)：多个事务同时操作相同数据库的同一个数据时，一个事务的执行不受另外一个事务的干扰
​	持久性(Durable)：一个事务一旦提交，则数据将持久化到本地，除非其他事务对其进行修改

相关步骤：

	1、开启事务
	2、编写事务的一组逻辑操作单元（多条sql语句）
	3、提交事务或回滚事务

### 事务的分类：

隐式事务，没有明显的开启和结束事务的标志

	比如
	insert、update、delete语句本身就是一个事务


显式事务，具有明显的开启和结束事务的标志

		1、开启事务
		取消自动提交事务的功能
		
		2、编写事务的一组逻辑操作单元（多条sql语句）
		insert
		update
		delete
		
		3、提交事务或回滚事务
### 使用到的关键字

	set autocommit=0;
	start transaction;//自动提交关闭，开启事务
	commit;
	rollback;
	
	savepoint  断点
	commit to 断点
	rollback to 断点

### 事务的隔离级别:

事务并发问题如何发生？ 

	当多个事务同时操作同一个数据库的相同数据时
事务的并发问题有哪些？

	脏读：一个事务读取到了另外一个事务未提交的数据
	不可重复读：同一个事务中，多次读取到的数据不一致
	幻读：一个事务读取数据时，另外一个事务进行更新，导致第一个事务读取到了没有更新的数据

如何避免事务的并发问题？

	通过设置事务的隔离级别
	1、READ UNCOMMITTED 
	2、READ COMMITTED 可以避免脏读
	3、REPEATABLE READ 可以避免脏读、不可重复读和一部分幻读
	4、SERIALIZABLE可以避免脏读、不可重复读和幻读

设置隔离级别：

	set session|global  transaction isolation level 隔离级别名;
查看隔离级别：

	select @@tx_isolation;

### MySQL如何解决脏读、不可重复读

- 多版本并发控制（MVCC）**快照读**

  MVCC即多个版本的数据实现并发控制的技术，基本思想是为每次事务生成一个新版本的数据，在读数据时选择不同版本的数据就可以实现对事务结果的完整性读取。

  update、insert、delete会生成事务id

  - 每条记录都会保存两个隐藏列：trx_id(事务id)和roll_pointer(回滚指针)两个字段
  - 每次操作都会生成一条undo log日志，回滚指针指向前一条记录
  - 查询的时候会读取ReadView：未提交的事务id数组+**已提交的最大事务id**，根据readview从undo log日志中最新的记录依次往下找

  **可重复读：只在第一次select获取readview，之后复用上一次select的readview**

  如果当前事务id<未提交的最小id，则可以读

  如果 未提交的最小id < 当前事务id < 未提交的最大id，则不可读(只有自己可读)， 当前事务id <= 已提交事物的最大id，则可读

  如果当前事务id > 已提交事务的最大id，则不可读

  **读已提交：每次select都会创建使用最新的readview**

  从最新事物记录开始找到事务id为已提交事务的最大id为止

  

  ![MVCC原理](/Users/ruicong/Documents/JavaNotes/pictures/MVCC原理.png)

- **当前读**

  MVCC 其它会对数据库进行修改的操作（INSERT、UPDATE、DELETE）需要进行加锁操作，从而读取最新的数据。可以看到 MVCC 并不是完全不用加锁，而只是避免了 SELECT 的加锁操作。

  在进行 SELECT 操作时，可以强制指定进行加锁操作。以下第一个语句需要加 S 锁，第二个需要加 X 锁。

  ```sql
  SELECT * FROM table WHERE ? lock in share mode;
  SELECT * FROM table WHERE ? for update;
  ```

  **在RR级别下，快照读是通过MVVC(多版本控制)和undo log来实现的，当前读是通过加record lock(记录锁)和gap lock(间隙锁)来实现的。**

### MySQL如何解决幻读

MVCC + next-key锁

**快照读的幻读是用MVCC解决的，当前的读的幻读是用间隙锁解决的。**

- next-key锁

  next-key 锁包含两部分：

  - 记录锁（行锁）
  - 间隙锁

  原理：将当前数据行与上一条数据和下一条数据之间的间隙锁定，保证此范围内读取的数据是一致的。

[参考](https://www.cnblogs.com/xuwc/p/13873293.html)

### 如何实现事物？

隔离性：MVCC+undo log

持久性：redo log

原子性：undo log

一致性：由其他三个特性保证



### SQL语句执行过程

1. Innodb在收到一个update语句后，会先根据条件找到数据所在的页，并将该页缓存在buffer pool中
2. 修改前的记录写入undo log
3. 执行update语句，修改buffer pool中的数据，也就是内存中的数据
4. 针对update语句生成一个redolog对象，存入logbuffer中，log(prepare状态)
5. 针对update语句生成binlog日志，用于数据恢复
6. 如果事务提交，则redolog设为commit状态，把redolog对象持久化，后续还有其他机制将buffer pool中所修改的数据页持久化到磁盘中
7. 如果事务回滚，则利用undolog日志进行回滚

![img](https://img2020.cnblogs.com/blog/2012006/202012/2012006-20201203220840727-213780904.png)

# 九、视图

含义：理解成一张虚拟的表

视图和表的区别：
	
		使用方式	占用物理空间
	
	视图	完全相同	不占用，仅仅保存的是sql逻辑
	
	表	完全相同	占用

视图的好处：


	1、sql语句提高重用性，效率高
	2、和表实现了分离，提高了安全性

### 视图的创建

	语法：
	CREATE VIEW  视图名
	AS
	查询语句;
### 视图的增删改查

​	1、查看视图的数据 ★
​	

	SELECT * FROM my_v4;
	SELECT * FROM my_v1 WHERE last_name='Partners';
	
	2、插入视图的数据
	INSERT INTO my_v4(last_name,department_id) VALUES('虚竹',90);
	
	3、修改视图的数据
	
	UPDATE my_v4 SET last_name ='梦姑' WHERE last_name='虚竹';

​	
​	4、删除视图的数据
​	DELETE FROM my_v4;

### 某些视图不能更新

​	包含以下关键字的sql语句：分组函数、distinct、group  by、having、union或者union all
​	常量视图
​	Select中包含子查询
​	join
​	from一个不能更新的视图
​	where子句的子查询引用了from子句中的表

### 视图逻辑的更新

#### 方式一：

​	CREATE OR REPLACE VIEW test_v7
​	AS
​	SELECT last_name FROM employees
​	WHERE employee_id>100;
​	

#### 方式二:

​	ALTER VIEW test_v7
​	AS
​	SELECT employee_id FROM employees;
​	

	SELECT * FROM test_v7;
### 视图的删除

​	DROP VIEW test_v1,test_v2,test_v3;

### 视图结构的查看	

​	DESC test_v7;
​	SHOW CREATE VIEW test_v7;



# 十、变量

- 系统变量
  - 全局变量
  - 会话变量
- 自定义变量
  - 用户变量
  - 局部变量

## 1、系统变量

变量由系统提供，不是用户定义，属于服务器层面

**全局变量：**

- 作用域：服务器每次启动将为所有的全局变量赋初始值，针对于所有的会话（连接）有效，但不能跨重启

**会话变量：**

- 作用域：仅仅对当前会话(连接)有效

**注意：**如果是全局级别，需要加global，如果是会话级别，则需要加SESSION，如果不写，则默认SESSION

使用的语法：

1、查看所有的系统变量

```mysql
SHOW GLOBAL variables;//查询全局变量
SHOW [SESSION] variables;//查询会话变量，session可省略
```

2、查看满足条件的部分系统变量

```mysql
SHOW GLOBAL|[SESSION] variables like '';
```

3、查看指定的某个系统变量值

```mysql
SELECT @@[SESSION.]系统变量名;//会话变量名,session可省略
SELECT @@GLOBAL.系统变量名;//全局变量名
```

4、为某个系统变量赋值

方式一：

```mysql
SET GLOABAL|[SESSION] 系统变量名 = 值;
```

方式二：

```mysql
SET @@GLOBAL|[SESSION].系统变量名 = 值;
```

## 2、自定义变量

**用户变量**

变量是用户自定义的，不是由系统的

- 作用域：针对于当前会话（连接）有效，同于会话变量的作用域
- 应用在任何地方，bigin end里面外面都行

1、声明并初始化

```mysql
SET @用户变量名 = 值;
SET @用户变量名 := 值;
SELECT @用户变量名 := 值;
```

2、赋值

方式一：

```mysql
SET @用户变量名 = 值;
SET @用户变量名 := 值;
SELECT @用户变量名 := 值;
```

方式二：

```mysql
SELECT 字段 INTO @变量名;
FROM 表
```

3、查看用户变量值

```mysql
SELECT @用户变量名;
```

**局部变量：**

- 作用域：仅仅在定义它的bigin end中有效
- 应用在bigin end中，必须在bigin end第一句话

1、声明

```mysql
DECLARE 变量名 类型;
DECLARE 变量名 类型 DEFAULT 值;
```

2、赋值

方式一：

```
SET 局部变量名 = 值;
SET 局部变量名 := 值;
SELECT @局部变量名 := 值;
```

方式二：

```mysql
SELECT 字段 INTO 局部变量名;
FROM 表
```

3、使用

```
SELECT 局部变量名;
```

**对比用户变量和局部变量：**

- 作用域
- 定义和使用位置
- 语法



# 十一、存储过程

含义：一组经过预先编译的sql语句的集合
好处：

	1、提高了sql语句的重用性，减少了开发程序员的压力
	2、提高了效率
	3、减少了传输次数

分类：

	1、无返回无参
	2、仅仅带in类型，无返回有参
	3、仅仅带out类型，有返回无参
	4、既带in又带out，有返回有参
	5、带inout，有返回有参
	注意：in、out、inout都可以在一个存储过程中带多个
### 创建存储过程

语法：

	create procedure 存储过程名(in|out|inout 参数名  参数类型,...)
	begin
		存储过程体
	
	end

类似于方法：

	修饰符 返回类型 方法名(参数类型 参数名,...){
	
		方法体;
	}

注意

	1、需要设置新的结束标记
	delimiter 新的结束标记
	示例：
	delimiter $
	
	CREATE PROCEDURE 存储过程名(IN|OUT|INOUT 参数名  参数类型,...)
	BEGIN
		sql语句1;
		sql语句2;
	
	END $
	
	2、存储过程体中可以有多条sql语句，如果仅仅一条sql语句，则可以省略begin end
	
	3、参数前面的符号的意思
	in:该参数只能作为输入 （该参数不能做返回值）
	out：该参数只能作为输出（该参数只能做返回值）
	inout：既能做输入又能做输出

## 调用存储过程

	call 存储过程名(实参列表)$
## 函数

### 创建函数

学过的函数：LENGTH、SUBSTR、CONCAT等
语法：

	CREATE FUNCTION 函数名(参数名 参数类型,...) RETURNS 返回类型
	BEGIN
		函数体
	
	END

### 调用函数

​	SELECT 函数名（实参列表）





### 函数和存储过程的区别

			关键字		调用语法	返回值			应用场景
	函数		FUNCTION	SELECT 函数()	只能是一个		一般用于查询结果为一个值并返回时，当有返回值而且仅仅一个
	存储过程	PROCEDURE	CALL 存储过程()	可以有0个或多个		一般用于更新

# 十二、流程控制结构

### 系统变量

一、全局变量

作用域：针对于所有会话（连接）有效，但不能跨重启

	查看所有全局变量
	SHOW GLOBAL VARIABLES;
	查看满足条件的部分系统变量
	SHOW GLOBAL VARIABLES LIKE '%char%';
	查看指定的系统变量的值
	SELECT @@global.autocommit;
	为某个系统变量赋值
	SET @@global.autocommit=0;
	SET GLOBAL autocommit=0;

二、会话变量

作用域：针对于当前会话（连接）有效

	查看所有会话变量
	SHOW SESSION VARIABLES;
	查看满足条件的部分会话变量
	SHOW SESSION VARIABLES LIKE '%char%';
	查看指定的会话变量的值
	SELECT @@autocommit;
	SELECT @@session.tx_isolation;
	为某个会话变量赋值
	SET @@session.tx_isolation='read-uncommitted';
	SET SESSION tx_isolation='read-committed';

### 自定义变量

一、用户变量

声明并初始化：

	SET @变量名=值;
	SET @变量名:=值;
	SELECT @变量名:=值;
赋值：

	方式一：一般用于赋简单的值
	SET 变量名=值;
	SET 变量名:=值;
	SELECT 变量名:=值;


	方式二：一般用于赋表 中的字段值
	SELECT 字段名或表达式 INTO 变量
	FROM 表;

使用：

	select @变量名;

二、局部变量

声明：

	declare 变量名 类型 【default 值】;
赋值：

	方式一：一般用于赋简单的值
	SET 变量名=值;
	SET 变量名:=值;
	SELECT 变量名:=值;


	方式二：一般用于赋表 中的字段值
	SELECT 字段名或表达式 INTO 变量
	FROM 表;

使用：

	select 变量名



二者的区别：

			作用域			定义位置		语法
用户变量	当前会话		会话的任何地方		加@符号，不用指定类型
局部变量	定义它的BEGIN END中 	BEGIN END的第一句话	一般不用加@,需要指定类型

### 分支

一、if函数
	语法：if(条件，值1，值2)
	特点：可以用在任何位置

二、case语句

语法：

	情况一：类似于switch
	case 表达式
	when 值1 then 结果1或语句1(如果是语句，需要加分号) 
	when 值2 then 结果2或语句2(如果是语句，需要加分号)
	...
	else 结果n或语句n(如果是语句，需要加分号)
	end 【case】（如果是放在begin end中需要加上case，如果放在select后面不需要）
	
	情况二：类似于多重if
	case 
	when 条件1 then 结果1或语句1(如果是语句，需要加分号) 
	when 条件2 then 结果2或语句2(如果是语句，需要加分号)
	...
	else 结果n或语句n(如果是语句，需要加分号)
	end 【case】（如果是放在begin end中需要加上case，如果放在select后面不需要）


特点：
	可以用在任何位置

三、if elseif语句

语法：

	if 情况1 then 语句1;
	elseif 情况2 then 语句2;
	...
	else 语句n;
	end if;

特点：
	只能用在begin end中！！！！！！！！！！！！！！！


三者比较：
			应用场合
	if函数		简单双分支
	case结构	等值判断 的多分支
	if结构		区间判断 的多分支

### 循环

语法：


	【标签：】WHILE 循环条件  DO
		循环体
	END WHILE 【标签】;

特点：

	只能放在BEGIN END里面
	
	如果要搭配leave跳转语句，需要使用标签，否则可以不用标签
	
	leave类似于java中的break语句，跳出所在循环！！！



# 十三、数据库设计三范式

**范式：**设计表的依据，按照这个三范式设计的表不会出现数据冗余。

## 第一范式

**任何一张表都应该有主键，并且每一个字段原子性不可再分。**

## 第二范式

**建立在第一范式的基础之上，所有非主键字段完全依赖主键，不能产生部分依赖**

![Snipaste_2020-09-23_15-27-59](D:\JavaHub\学习相关\课堂截图\Snipaste_2020-09-23_15-27-59.png)

**解决方法：**多对多 **三张表，关系表两个外键**

## 第三范式

建立在第二范式的基础之上，所有非主键字段直接依赖主键字段，不能产生传递依赖。

班级t_class

学生t_student

![Snipaste_2020-09-23_15-29-18](D:\JavaHub\学习相关\课堂截图\Snipaste_2020-09-23_15-29-18.png)

**解决方法：**一对多 **两张表，多的表加外键**



面试：**在实际开发中，以满足客户的需求为主，有的时候会拿冗余换执行速度**



# 十四、数据库存储引擎

存储引擎这个名字只有在MySQL中存在。（Oracle中有对应机制，但是不叫做存储引擎）

MySQL支持很多存储引擎，每一个存储引擎都对应了一种不同的存储方式。每一个存储引擎都有自己的优缺点，需要在合适的时机选择合适的引擎。

查看当前MySQL支持的存储引擎

```mysql
show engines \G
```

**常见的存储引擎：**

## 1、MyISAM

- 这种存储引擎不支持事务
- 这种存储引擎是MySQL最常用的存储引擎，但是这种引擎不是默认的。

**特征：**

- 使用三个文件表示每个表：

  > 格式文件 - 存储表结构的定义（mytable.frm）
  >
  > 数据文件 - 存储表行的内容、数据（mytable.MYD）
  >
  > 索引文件 - 存储表上索引（mytable.MYI）

- 优点：可被压缩，节省存储空间。并且可以转换为只读表，提高检索效率。

- 缺点：不支持事务。

## 2、InnoDB

**优点**：支持事务、行级锁、外键，这种存储引擎数据的安全得到保障

表的结构存储在xxx.frm文件中

数据存储在tablespace这样的表空间中（逻辑概念），无法被压缩，无法转换成只读。

这种引擎在MySQL数据库崩溃之后提供自动恢复机制。

## 3、MEMORY

**缺点：**不支持事务。数据容易丢失，所有数据和索引都是存储在内存当中的。

**优点：**查询速度最快。以前叫做**HEAP引擎**



# 十五、索引

## 1、为什么使用索引

索引相当于一本书的目录，通过目录可以快速找到对应资源

在数据库方面，查询一张表的时候有两种检索方式：

- 全表扫描
- 根据索引检索（效率很高）

索引最根本是缩小了扫描范围。

索引虽然可以提高检索效率，但不能随意添加索引。因为索引也是数据库中的对象，也需要数据库不断地维护，是有维护成本的。比如，表中的数据经常被修改，这样就不适合添加索引，因为数据一旦被修改，索引需要重新排序，进行维护。

添加索引是给某一个字段，或者说某些字段添加索引。

```mysql
SELECT book FROM books WHERE book = 'haryy potter'
```

**当book字段没有添加索引的时候，以上sql语句会进行全表扫描，扫描ename字段中所有的值。**

**当book字段添加索引的时候，以上sql语句会进行索引扫描，快速定位。**

## 2、什么时候添加索引

- 数据量庞大（根据客户的需求）
- 该字段很少DML操作（因为字段进行修改操作，索引也需要维护）
- 该字段经常出现在WHERE子句中（经常根据哪个字段查询）

**主键和具有UNIQUE约束的字段会自动添加索引**，根据主键查询效率较高，尽量根据主键检索。



## 3、怎么添加和删除索引

**查看sql语句的执行计划：EXPLAIN**

```mysql
EXPLAIN SELECT book FROM books WHERE book = 'haryy potter'
```

**创建索引**

```mysql
CREATE INDEX 索引名称 on 表名(字段名)
```

**删除索引**

```mysql
DROP INDEX 索引名称 ON 表名
```

## 4、索引实现原理

索引底层采用的数据结构是：B+ Tree

通过B+ Tree缩小了扫描范围，底层索引进行了排序，分区，索引会携带数据在表中的“物理地址”，最终通过索引检索到数据之后，获取到关联的物理地址，通过物理地址定位表中的数据，效率是最高的。

![Snipaste_2020-08-30_22-32-45](D:\JavaHub\学习相关\课堂截图\Snipaste_2020-08-30_22-32-45.png)

## 5、索引的分类

**单一索引：**给单个字段添加索引

**复合索引：**给多个字段联合起来添加1个索引

**主键索引：**主键上会自动添加索引

**唯一索引：**有UNIQUE约束的字段上会自动添加索引



## 6、索引失效

1. **模糊查询的时候，第一个通配符使用的是%，这个时候索引是失效的。**

   ```sql
   SELECT book FROM books WHERE book = '%A%'
   ```

2. 一些关键字会导致索引失效，

   ```sql
   or, != , not in, is null, is not unll
   ```

