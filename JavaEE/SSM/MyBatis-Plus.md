# MyBatis-Plus

[MyBatis-Plus](https://github.com/baomidou/mybatis-plus)（简称 MP）是一个 [MyBatis](http://www.mybatis.org/mybatis-3/)的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

## 1、查看sql输出日志

```yaml
#mybatis日志
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

## 2、主键生成策略

### ①、自动增长 AUTO INCREMENT

![自动增长策略](D:\JavaHub\学习相关\Java笔记\pictures\自动增长策略.png)

要想主键自增需要配置如下主键策略：

- 需要在创建数据表的时候设置主键自增

- 实体字段中配置 @TableId(type = IdType.AUTO)

  ```java
  @TableId(type = IdType.AUTO)
  private Long id;
  ```

要想影响所有实体的配置，可以设置全局主键配置:

```yaml
#全局设置主键生成策略
mybatis-plus:
	global-config:
		db-config:
			id-type: auto
```



## ②、UUID

每次生成随机唯一的值

排序不方便

![UUID生成策略](D:\JavaHub\学习相关\Java笔记\pictures\UUID生成策略.png)

## ③、Redis实现

## ④、snowflake算法(mp自带策略)

snowflake是Twitter开源的分布式ID生成算法，结果是一个long型的ID。其核心思想是：使用41bit作为毫秒数，10bit作为机器的ID（5个bit是数据中心，5个bit的机器ID），12bit作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID），最后还有一个符号位，永远是0。



![主键策略](D:\JavaHub\学习相关\Java笔记\pictures\主键策略.png)

- AUTO：自动增长
- **ID_WORKKER：mp自带策略，生成19位值，数字类型使用这种策略**
- **ID_WORKKER_STR：mp自带策略，生成19位值，字符串类型使用这种策略**
- INPUT：设置id值
- NONE：输入
- UUID：随机唯一值
