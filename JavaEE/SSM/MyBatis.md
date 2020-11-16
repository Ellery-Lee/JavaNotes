# MyBatis [黑马程序员](https://www.bilibili.com/video/BV1mE411X7yp?p=1)

## 一、什么是框架？

它是软件开发中的一套解决方案，不同的框架解决的是不同的问题。

- 使用框架的好处：
  - 框架封装了很多的细节，使开发者可以使用极简的方式实现功能。大大提高开发效率。

## 二、三层架构

表现层：用于展示数据的

业务层：是处理业务需求的

持久层：是和数据库交互的

![Snipaste_2020-10-12_12-22-06.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Snipaste_2020-10-12_12-22-06.png?raw=true)

## 三、持久层技术解决方案

#### 1、JDBC技术

- Connection
- PreparedStatement
- ResultSet

#### 2、Spring的JDBCTemplate

- Spring中对JDBC的简单封装

#### 3、Apache的DBUtils

- 它和Spring的JDBCTemplate很像，也是对JDBC的简单封装

**以上这些都不是框架：**

- JDBC是规范
- Spring的JDBCTemplate和Apache的DBUtils都只是工具类

## 四、MyBatis概述

MyBatis是一个持久层框架，用Java编写的。它封装了JDBC操作的很多细节，使开发者只需要关注sql语句本身，而无需关注注册驱动，创建链接等繁杂过程。它使用了ORM思想，实现了结果集的封装。

#### 1、ORM

Object Relation Mapping 对象关系映射。简单地说，就是把数据库表和实体类的属性对应起来，让我们可以操作实体类就实现操作数据库表。

## 五、MyBatis入门

### 1、MyBatis环境搭建

第一步：创建maven工程并导入坐标

第二步：创建实体类和dao接口

第三步：创建MyBatis的主配置文件  SqlMapConfig.xml

第四步：创建映射配置文件   IUserDao.xml

**环境搭建的注意事项：**

1. 创建IUserDao.xml和IUserDao.java时，名称是为了和我们之前的知识保持一致。

   在MyBatis中它把持久层的操作接口名称和映射文件也叫做：Mapper

   所以：IUserDao和IUserMapper是一样的

2. 在IDEA创建目录的时候，它和包是不一样的

   包在创建时：com.liruicong.dao它时三级结构

   目录在创建时：com.liruicong.dao它是一级目录

3. MyBatis的映射配置文件位置必须和dao接口的包结构相同

4. 映射配置文件的mapper标签namespace属性的取值必须是dao接口的全限定类名

5. 映射配置文件的操作配置（select），id属性的取值必须是dao接口的方法名

   当我们遵从了3、4、5点之后，我们在开发中就无须再写dao的实现类，dao接口的实现类转变为一个Mapper配置文件

### 2、MyBatis的入门案例

1. 读取配置文件
2. 创建SqlSessionFactory工厂
3. 创建SqlSession
4. 创建Dao接口的代理对象
5. 执行dao中的方法
6. 释放资源

![Snipaste_2020-11-01_15-53-45.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Snipaste_2020-11-01_15-53-45.png?raw=true)

**注意事项：**

- 不要忘记在映射配置中告知MyBatis要封装到哪个实体类中，配置的方式：指定实体类的全限定类名

  ```xml
  com.liruicong.domain.User
  ```

**可能会遇到的问题：**

- 配置文件没有注册
- 绑定接口错误
- 方法名不对
- 返回类型不对
- Maven导出资源问题

### 3、MyBatis基于注解的入门案例

把IUserDao.xml移除，在dao接口的方法上使用@Select注解，并且指定SQL语句，同时需要在SqlMapConfig.xml中的mapper配置时，使用class属性指定dao接口的全限定类名

**明确：**我们在实际开发中，都是越简便越好，所以都是采用不写dao实现类的方式。不管使用XML还是注解配置，但是MyBatis它是支持写dao实现类的。

## 六、自定义MyBatis的分析

MyBatis在使用代理dao的方式实现增删改查时做了什么是？

**只有两件事：**

- 创建代理对象
- 在代理对象中调用selectList

![自定义Mybatis分析.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/%E8%87%AA%E5%AE%9A%E4%B9%89Mybatis%E5%88%86%E6%9E%90.png?raw=true)

自定义MyBatis能通过入门案例看到的类

- class Resources
- class SqlSessionFactoryBuilder
- interface SqlSessionFactory
- interface SqlSession

## 七、MyBatis 参数深入

### 1、OGNL表达式

Object Graphic Navigation Language：对象图导航语言

它是通过对象的取值方法来获取数据。在写法上把get给省略了。

比如：我们获取用户的名称

类中的写法：

```java
user.getUsername();
```

OGNL表达式写法：

```java
user.username
```

MyBatis中为什么能直接写username，而不用user.呢？

因为在parameterType中已经提供了属性所属的类，所以此时不需要写对象名。

## 八、MyBatis中的连接池以及事务控制（原理了解，应用会用）

### 1、连接池

我们在实际开发中都会使用连接池。因为它可以减少我们获取连接所消耗的时间。

**连接池就是用于存储连接的一个容器。**容器其实就是一个集合对象，该集合必须是线程安全的，不能两个线程拿到同一连接。该集合还必须实现队列的特性：先进先出。

### 2、MyBatis中的连接池

MyBatis连接池提供了3种方式的配置：

- 配置的位置：
  - 主配置文件SqlMapConfig.xml中的dataSource标签，type属性就是表示采用何种连接池方式。
- type的取值：
  - POOLED 采用传统的javax.sql.DataSource规范中的连接池，MyBatis种有针对规范的实现
  - UNPOOLED 采用传统的获取连接的方式，虽然也实现了javax.sql.DataSource接口，但是并没有使用池的思想。
  - JNDI 采用服务器提供的JNDI技术实现，来获取DataSource对象，不同的服务器所能拿到的DataSource是不一样的。**注意：**如果不是web或者Maven的war工程，是不能使用的。我们课程中使用的是tomcat服务器，采用的连接池就是dbcp连接池。

### 3、MyBatis中的事务

什么是事务？

事物的四大特性ACID

不考虑隔离性会产生的三个问题

解决办法：四种隔离级别

它是通过sqlSession对象的commit方法和rollback方法实现事务的提交和回滚。

## 九、MyBatis基于XML配置的动态SQL语句使用（会用即可）

- if
- where
- foreach

## 十、MyBatis的多表操作（掌握应用）

### 1、表之间的关系有几种：

一对多：

多对一：

一对一：

多对多：

**举例：**

用户和订单就是一对多

订单和用户就是多对一

- 一个用户可以下多个订单
- 多个订单属于同一个用户

人和身份证号就是一对一

- 一个人只能有一个身份证号

- 一个身份证号只能属于一个人

老师和学生之间就是多对多

- 一个学生可以被多个老师教过
- 一个老师可以交多个学生

**特例：**

如果拿出每一个订单，他都只能属于一个用户。所以MyBatis就把多对一看成了一对一。

### 2、MyBatis的多表查询

**示例1：用户和账户**

- 一个用户可以有多个账户
- 一个账户只能属于同一个用户（多个账户也可以属于同一个用户）

步骤：

- 建立两张表：用户表、账户表
  - 让用户表和账户表之间具备一对多的关系：需要使用外键在账户表中添加
- 建立两个实体类：用户实体类和账户实体类
  - 让用户和账户的实体类能体现出来一对多的关系
- 建立两个配置文件：用户配置文件、账户配置文件
- 实现配置：
  - 当我们查询用户时，可以同时得到用户下所包含的账户信息
  - 当我们查询账户时，可以同时得到账户的所属用户信息

**示例2：用户和角色**

- 一个用户可以有多个角色
- 一个角色可以赋予多个用户

步骤：

- 建立两张表：用户表、角色表
  - 让用户表和角色表之间具备多对多的关系：需要使用中间表，中间表中包含各自的主键，在中间表中是外键。
- 建立两个实体类：用户实体类和角色实体类
  - 让用户和角色的实体类能体现出来一对多的关系
  - 各自包含对方一个集合引用
- 建立两个配置文件：用户配置文件、角色配置文件
- 实现配置：
  - 当我们查询用户时，可以同时得到用户下所包含的角色信息
  - 当我们查询角色时，可以同时得到角色的所属用户信息

## 十一、MyBatis的延迟加载

问题：在一对多中，当我们有一个用户，它有100个账户。

- 在查询用户的时候，要不要把关联的账户查出来？

  **在查询用户时，用户下的账户信息应该是，什么时候使用，什么时候查询的**

- 在查询账户的时候，要不要把关联的用户查出来？

  **在查询账户时，账户的所属用户信息应该是随着账户查询时一起查询出来。**

### 1、什么是延迟加载？

在真正使用数据时才发起查询，不用的时候不查询。按需加载（懒加载）

### 2、什么是立即加载？

不管用不用，只要一调用方法，马上发起查询



在对应的四种表关系中：一对多，多对一，一对一，多对多

- 一对多，多对多：通常情况下都是采用延迟加载
- 多对一，一对一：通常情况下都是采用立即加载

## 十二、MyBatis的缓存

什么是缓存？

为什么使用缓存？

什么样的数据能使用缓存，什么样的数据不能使用缓存

MyBatis中的一级缓存和二级缓存

## 十三、MyBatis的注解开发

环境搭建

单表CRUD操作（代理Dao方式）

多表查询操作

缓存的配置