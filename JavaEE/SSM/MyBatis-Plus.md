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

![自动增长策略.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/%E8%87%AA%E5%8A%A8%E5%A2%9E%E9%95%BF%E7%AD%96%E7%95%A5.png?raw=true)

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



### ②、UUID

每次生成随机唯一的值

排序不方便

![UUID生成策略.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/UUID%E7%94%9F%E6%88%90%E7%AD%96%E7%95%A5.png?raw=true)

### ③、Redis实现

### ④、snowflake算法(mp自带策略)

snowflake是Twitter开源的分布式ID生成算法，结果是一个long型的ID。其核心思想是：使用41bit作为毫秒数，10bit作为机器的ID（5个bit是数据中心，5个bit的机器ID），12bit作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID），最后还有一个符号位，永远是0。



![主键策略.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/%E4%B8%BB%E9%94%AE%E7%AD%96%E7%95%A5.png?raw=true)

- AUTO：自动增长
- **ID_WORKKER：mp自带策略，生成19位值，数字类型使用这种策略**
- **ID_WORKKER_STR：mp自带策略，生成19位值，字符串类型使用这种策略**
- INPUT：设置id值
- NONE：输入
- UUID：随机唯一值

## 3、增删改查

### ①、增

```java
@Test
    public void addUser(){
        User user = new User();
        user.setName("lucy");
        user.setAge(30);
        user.setEmail("lucy@qq.com");
        int insert = userMapper.insert(user);
        System.out.println("insert:" + insert);
    }
```

### ②、删

```java
//删除操作 物理删除
    @Test
    public void testDeleteById(){
        int result = userMapper.deleteById(1L);
        System.out.println(result);
    }
//批量删除
    @Test
    public void testDeleteBatchIds(){
        int result = userMapper.deleteBatchIds(Arrays.asList(1, 2));
        System.out.println(result);
    }
//逻辑删除见下
```

### ③、改

```java
@Test
    public void updateUser(){
        User user = new User();
        user.setId(2L);
        user.setAge(120);
        int row = userMapper.updateById(user);
        System.out.println(row);
    }
```

### ④、查

```java
@Test
    void findAll() {
        List<User> users = userMapper.selectList(null);
        System.out.println(users);
    }

//多个id批量查询
    @Test
    public void testSelectDemo1(){
        List<User> users = userMapper.selectBatchIds(Arrays.asList(1L, 2L, 3L));
        System.out.println(users);
    }
```

## 4、自动填充

不需要set到对象里面的值，使用mp方式实现数据添加

- 第一步：在实体类里面进行自动填充属性添加注解

  ```java
  @TableField(fill = FieldFill.INSERT)
  private Date createTime;
  @TableField(fill = FieldFill.INSERT_UPDATE)
  private Date updateTime;
  ```

- 第二步：创建类，实现接口MetaObjectHandler，实现接口里面的方法

```java
@Service
public class MyMetaObjectHandler implements MetaObjectHandler {
    //使用mp实现添加操作，这个方法执行
    @Override
    public void insertFill(MetaObject metaObject) {
        this.setFieldValByName("createTime", new Date(), metaObject);
        this.setFieldValByName("updateTime", new Date(), metaObject);
    }
    //使用mp实现修改操作，这个方法执行
    @Override
    public void updateFill(MetaObject metaObject) {
        this.setFieldValByName("updateTime", new Date(), metaObject);
    }
}
```

## 5、Mybatis实现乐观锁

数据库操作，如果不考虑事务隔离性，会产生读问题：

- 脏读

- 不可重复读

- 幻读

写问题：

- 丢失更新问题

解决方案：

- 悲观锁：串行

- 乐观锁：比较当前数据版本和数据库版本是否一样，事务提交后版本号+1

  ![乐观锁说明.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/%E4%B9%90%E8%A7%82%E9%94%81%E8%AF%B4%E6%98%8E.png?raw=true)

乐观锁的具体实现：

- 表添加字段，作为乐观锁版本号

- 对应实体类添加版本号属性

- 在实体类版本号属性添加注解

- 配置乐观锁插件

  ```java
  @Configuration
  public class MpConfig {
      // 注册乐观锁插件(3.4.0后废弃了此方法，使用OptimisticLockerInnerInterceptor)
      @Bean
      public OptimisticLockerInterceptor optimisticLockerInnerInterceptor() {
          return new OptimisticLockerInterceptor();
      }
  }
  ```

- 测试乐观锁

  ```java
  //测试乐观锁
      @Test
      public void testOptimisticLocker(){
          //根据id查询数据
          User user = userMapper.selectById(1395648418038161411L);
          //进行修改
          user.setAge(200);
          userMapper.updateById(user);
      }
  ```

## 6、分页查询

PageHelper类似

- 第一步：配置分页插件

  ```java
  //分页插件
      @Bean
      public PaginationInterceptor paginationInterceptor(){
          return new PaginationInterceptor();
      }
  ```

- 第二步：编写分页代码

  直接new page对象，传入两个参数(当前页和每页显示记录数)，调用mp方法实现分页查询

  ```java
  //分页查询
      @Test
      public void testPage(){
          //1、创建page对象
          //传入两个参数：当前页和每页显示记录数
          Page<User> page = new Page<>(1,3);
          //调用mp分页查询的方法
          //调用mp分页查询过程中，底层封装
          //把分页所有数据封装到page对象里面
          userMapper.selectPage(page,null);
          //通过page对象获取分页数据
          System.out.println(page.getCurrent());//当前页
          System.out.println(page.getRecords());//每页数据list集合
          System.out.println(page.getSize());//每页显示记录数
          System.out.println(page.getTotal());//总记录数
          System.out.println(page.getPages());//总页数
          System.out.println(page.hasNext());//下一页
          System.out.println(page.hasPrevious());//上一页
      }s
  ```

## 7、逻辑删除

物理删除：真实删除，将对应数据从数据库中删除，之后查询不到此条被删除数据

逻辑删除：假删除，将对应数据中代表是否被删除字段状态修改为“被删除状态”，之后在数据库中仍旧能看到此条数据记录。

- 第一步：表添加逻辑删除字段，对应实体类添加属性

  ```java
  @TableLogic
  private Integer deleted;
  ```

- 第二步：配置逻辑删除插件

  ```java
  @Bean
  public ISqlInjector sqlInjector(){
      return new LogicSqlInjector();
  }
  ```

- 第三步：测试执行删除代码，最终效果deleted值变成了1

## 8、性能分析

性能分析拦截器，用于输出每条SQL语句及其执行时间。

SQL性能执行分析，开发环境使用，超过指定时间，停止运行。有助于发现问题

- 第一步：配置 插件

  ```java
  //SQL执行性能分析插件(高版本已移除)
      //开发环境使用，线上不推荐。maxTime指的是sql最大执行时长
      //三种环境：
      //dev：开发环境
      //test：测试环境
      //prod：生产环境
      @Bean
      @Profile({"dev", "test"})//设置dev test 环境开启
      public PerformanceInterceptor performanceInterceptor(){
          PerformanceInterceptor performanceInterceptor = new PerformanceInterceptor();
          performanceInterceptor.setMaxTime(100);//ms，超过此处设置的ms则sql不执行
          performanceInterceptor.setFormat(true);
          return performanceInterceptor;
      }
  ```

- 第二步：测试

  ```
  Time：69 ms - ID：com.liruicong.mpdemo_01.mapper.UserMapper.insert
  Execute SQL：
  Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@9fc9f91]
      INSERT 
      INTO
          user
          ( name, age, email, create_time, update_time, version ) 
      VALUES
          ( '岳不群1', 70, 'lilei@qq.com', '2021-06-06 10:05:45.422', '2021-06-06 10:05:45.422', 1 )
  
  insert:1
  ```

  如果执行时间超过maxTime会报错

## 9、mp实现复杂条件查询

![Wrapper继承结构.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Wrapper%E7%BB%A7%E6%89%BF%E7%BB%93%E6%9E%84.png?raw=true)

一般使用**QueryWrapper**构造条件

Wrapper：条件构造抽象类，最顶端父类

AbstractWrapper：用于查询条件封装，生成sql的where条件

QueryWrapper：Entity对象封装操作类，不是用lambda语法

UpdateWrapper：Update条件封装，用于Entity对象更新操作

AbstractLambdaWrapper：lambda语法使用Wrapper统一处理解析lambda获取column

LambdaQueryWrapper：看名称也能明白就是用于Lambda语法使用的查询Wrapper

LambdaUpdateWrapper：Lambda更新封装Wrapper

- 第一步：创建QueryWrapper对象，调用方法实现各种条件查询
