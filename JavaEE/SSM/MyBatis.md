# MyBatis [黑马程序员](https://www.bilibili.com/video/BV1mE411X7yp?p=1)

## 一、什么是框架？

它是软件开发中的一套解决方案，不同的框架解决的是不同的问题。

- 使用框架的好处：
  - 框架封装了很多的细节，使开发者可以使用极简的方式实现功能。大大提高开发效率。

## 二、三层架构

表现层：用于展示数据的

业务层：是处理业务需求的

持久层：是和数据库交互的

![Snipaste_2020-10-12_12-22-06](D:\JavaHub\学习相关\课堂截图\Snipaste_2020-10-12_12-22-06.png)

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

   当我们遵从了3、4、5点之后，我们在开发中就无须再写dao的实现类。