# [MySQL高级](https://www.bilibili.com/video/BV1KW411u7vy)

## 一、MySQL逻辑架构

和其他存储引擎相比，MySQL可以在多种不同场景中应用并发挥良好作用，主要体现在存储引擎的架构上，**插件式的存储引擎架构将查询处理和系统任务及数据的存储提取相分离**。可以根据实际的业务需求选择合适的存储引擎。

![MySQL逻辑架构](D:\JavaHub\学习相关\Java笔记\pictures\MySQL逻辑架构.jpg)

- 连接层
- 服务层
- 引擎层
- 存储层

**存储引擎**

- 命令查看：show engines;

- MyISAM和InnoDB

  ![MyISAM和InnoDB](D:\JavaHub\学习相关\Java笔记\pictures\MyISAM和InnoDB.jpg)

## 二、索引优化

### 1、性能下降SQL慢

- 查询语句写的烂
- 索引失效
- 关联查询太多join
- 服务器调优及各个参数设置(缓冲、线程数)

### 2、常见通用的join查询

#### ①、SQL的执行顺序

- 手写

- 机读

  ![SQL执行顺序](D:\JavaHub\学习相关\Java笔记\pictures\SQL执行顺序.jpg)

#### ②、JOIN

![join图](D:\JavaHub\学习相关\Java笔记\pictures\join图.png)

- Inner Join

  ```sql
  select
  	<select_list>
  from
  	<Table A> A
  inner join
  	<Table B> B
  on
  	A.key = B.key
  ```

- Left Join

  ```sql
  select 
  	<select_list>
  from
  	<Table A> A
  left join
  	<Table B> B
  on
  	A.key = B.key
  ```

- Right Join

  ```sql
  select
  	<select_list>
  from 
  	<Table A> A
  right Join
  	<Table B> B
  on
  	A.key = B.key
  ```

- Left Join 左独占

  ```sql
  select 
  	<select_list>
  from
  	<Table A> A
  left join
  	<Table B> B
  on 
  	A.key = B.key
  where 
  	B.key is null
  ```

- Right Join 右独占

  ```sql
  select 
  	<select_list>
  from
  	<Table A> A
  right join
  	<Table B> B
  on 
  	A.key = B.key
  where 
  	A.key is null
  ```

- Outer Join(MySQL不支持)

  ```sql
  select 
  	<select_list>
  from
  	<Table A> A
  full outer join
  	<Table B> B
  on
  	A.key = B.key
  ```

- Outer Join 双方独有(MySQL不支持)

  ```sql
  select 
  	<select_list>
  from
  	<Table A> A
  full outer join
  	<Table B> B
  on 
  	A.key = B.key
  where
  	A.key is null
  or 
  	B.key is null
  ```

### 3、索引

MySQL官方对索引的定义为：索引(Index)是帮助MySQL高效获取数据的数据结构（where和order by）。可以得到索引的本质：**索引是数据结构**。简单的理解为：**排好序的快速查找数据结构**

一般来说索引本身也很大，不可能全部存储在内存中，因此索引往往以索引文件的形式存储在磁盘上

**我们平常所说的索引，如果没有特别指明，都是指B树(多路搜索树，并不一定是二叉的)结构组织的索引。**其中聚集索引，次要索引，覆盖索引，
复合索引，前缀索引，唯一索引默认都是使用B+树索引，统称索引。当然，除了B+树这种类型的索引之外，还有哈稀索引(hash index)等。

优势：

- 类似大学图书馆建书目索引，提高数据检索的效率，降低数据库的IO成本
- 通过索引列对数据进行排序，降低数据排序的成本，降低了CPU的消耗

劣势：

- 实际上索引也是一张表，保存了主键和索引字段，并指向实体表的记录，所以索引列也是要占空间的
- 虽然索引大大提高了查询速度，但同时降低了更新表的速度，对表进行增删改时，MySQL不仅要保存数据，还要保存索引文件每次更新添加了索引列的字段
- 索引只是提高效率的一个因素，如果MySQL有大量的表，就需要花时间研究建立最优秀的索引，或优化查询

### 4、索引分类

- 单值索引：即一个索引只包含单个列，一个表可以有多个单列索引
- 唯一索引：索引列的值必须唯一，但允许有空值
- 复合索引：即一个索引包含多个列

### 5、基本语法

创建：

- CREATE [UNIQUE ]  INDEX [indexName] ON mytable  (columnname(length)) 

- ALTER mytable ADD  [UNIQUE ]  INDEX [indexName] ON (columnname(length)) 

删除：DROP INDEX [indexName] ON mytable; 

查看：SHOW INDEX FROM table_name

### 6、BTree索引（MyISAM普通索引）

![image-20210721161429911](C:\Users\86177\AppData\Roaming\Typora\typora-user-images\image-20210721161429911.png)

**初始化介绍**
一颗b树，浅蓝色的块我们称之为一个磁盘块，可以看到每个磁盘块包含几个数据项（深蓝色所示）和指针（黄色所示)，如磁盘块1包含数据项17和35，包含指针P1、P2、P3，P1表示小于17的磁盘块，P2表示在17和35之间的磁盘块，P3表示大于35的磁盘块。真实的数据存在于叶子节点即3、5、9、10、13、15、28、29、36、60、75、79、90、99。非叶子节点不存储真实的数据，只存储指引搜索方向的数据项，如17、35并不真实存在于数据表中。

**查找过程**
如果要查找数据项29，那么首先会把磁盘块1由磁盘加载到内存，此时发生一次IO，在内存中用二分查找确定29在17和35之间，锁定磁盘块1的P2指针，内存时间因为非常短（相比磁盘的IO）可以忽略不计，通过磁盘块1的P2指针的磁盘地址把磁盘块3由磁盘加载到内存，发生第二次IO，29在26和30之间，锁定磁盘块3的P2指针，通过指针加载磁盘块8到内存，发生第三次IO，同时内存中做二分查找找到29，结束查询，总计三次IO。

真实的情况是，3层的b+树可以表示上百万的数据，如果上百万的数据查找只需要三次IO，性能提高将是巨大的，如果没有索引，每个数据项都要发生一次IO，那么总共需要百万次的IO，显然成本非常非常高。

### 7、哪些情况适合建索引

- 主键自动建立唯一索引
- 频繁作为参训条件的字段应该创建索引
- 查询中与其他表关联的字段，外键关系建立索引
- 单键/组合索引的选择问题，who？(在高并发下倾向创建组合索引)
- 查询中排序的字段，排序字段若通过索引去访问将大大提高排序速度
- 查询中统计或者分组字段

### 8、哪些情况不创建索引

- 表记录太少

- 经常增删改的表

  原因：提高了查询速度，同时却会降低更新表的速度，如对表进行INSERT、UPDATE和DELETE。因为更新表时，MySQL不仅要保存数据，还要保存一下索引文件

- Where条件里用不到的字段不创建索引

- 数据重复且分布平均的表字段，因此应该只为最经常查询和最经常排序的数据列建立索引。注意，如果某个数据列包含许多重复的内容，为它建立索引就没有太大的实际效果。

## 三、性能分析

### 1、MySQL Query  Optimizer

MySQL中有专门负责优化select语句的优化器模块，主要功能：计算分析系统中收集到的统计信息，为客户端请求的Query提供他认为最优的执行计划(他认为最优的数据检索方式，不一定是DBA认为最优的，这部分最耗时间)

当客户端向MySQL请求一条Query，命令解析器模块完成请求分类，区别出是select并转发给MySQL Query Optimizer时，MySQL Query Optimizer首先会对整条Query进行优化，处理掉一些常量表达式的预算，直接转换成常量值。并对Query中的查询条件进行简化和转换，如去掉一些无用或显而易见的条件、结构调整等。然后分析Query中的Hint信息，看显示Hint信息是否可以完全确定该Query的执行计划。如果没有Hint或Hint信息还不足以完全确定执行计划，则会读取所涉及对象的统计信息，根据Query进行写相应的计算分析，然后再得出最后的执行计划。

### 2、MySQL常见瓶颈

- CPU：SQL中对大量数据进行比较、关联、排序、分组
- IO：
  - 实例内存满足不了缓存数据或排序等需要，导致产生大量 物理 IO。
  - 查询执行效率低，扫描过多数据行。
- 锁：
  - 不适宜的锁的设置，导致线程阻塞，性能下降。
  - 死锁，线程之间交叉调用资源，导致死锁，程序卡住。
- 服务器硬件的性能瓶颈：top,free, iostat和vmstat来查看系统的性能状态
