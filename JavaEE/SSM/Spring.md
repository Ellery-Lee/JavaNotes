# Spring

## 一、Spring概述

Spring是分层的Java SE/EE应用full-stack轻量级开源框架，以IOC(Inverse Of Control：反转控制)和AOP(Aspect Oriented Programming：面向切面编程)为内核，提供了展现层Spring MVC和持久层Spring JDBC以及业务层事务管理等众多的企业级应用技术，还能整合开源世界众多著名的第三方框架和类库，逐渐成为使用最多的Java EE企业应用开源框架。

![Snipaste_2020-11-21_11-34-08.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Snipaste_2020-11-21_11-34-08.png?raw=true)

## 二、程序的耦合及解耦

**耦合：程序间的依赖关系**

包括：

- 类之间的依赖
- 方法间的依赖

**解耦：降低程序间的依赖关系**

实际开发中应该做到：编译期不依赖，运行时才依赖

解耦的思路：

第一步：使用反射来创建对象，而避免使用new关键字

第二步：通过读取配置文件来获取要创建的对象全限定类名

## 三、IOC概念和Spring中的IOC

**IOC(Inversion of Control)：**控制反转，把对象创建的权利交给框架(工厂)，是框架的重要特征，并非面向对象编程的专用术语。它包括依赖注入和依赖查找。

**作用：**削减计算机程序的耦合(不能完全消除)

```java
/**
     * 获取spring容器的IOC核心容器，并根据Id获取对象
     * ApplicationContext的三个常用实现类：
     *      ClassPathXmlApplicationContext：它可以加载类路径下的配置文件，要求配置文件必须在类路径下。不在的话，加载不了。(更常用)
     *      FileSystemXmlApplicationContext：它可以加载磁盘任意路径下的配置文件(必须有访问权限)
     *      AnnotationConfigApplicationContext：它是用于读取注解创建容器的
     *
     * 核心容器的两个接口引发出的问题：
     * ApplicationContext：单例对象适用(采用此接口)
     *      它在构建核心容器时，创建对象采取的策略是采用立即加载的方式。也就是说，只要一读取完配置文件马上就创建配置文件中配置的对象
     * BeanFactory：多例对象适用
     *      它在构建核心容器时，创建对象采取的策略是采用延迟加载的方式。也就是说，什么时候根据Id获取对象了，什么时候才真正的创建对象。
     */
```

Spring对Bean的管理细节

```xml
<!-- 把对象的创建交给spring来管理 -->
<!-- Spring对Bean的管理细节
    1、创建Bean的三种方式
    2、Bean对象的作用范围
    3、Bean对象的生命周期
-->
<!-- 创建Bean的三种方式 -->
<!-- 第一种方式：使用默认构造函数创建
    在Spring的配置文件中使用Bean标签，配以id和class属性后，且没有其他属性和标签时。
    采用的就是默认构造函数创建Bean对象，此时如果类中没有默认构造函数，则对象无法创建。

<bean id="accountService" class="com.liruicong.service.impl.AccountServiceImpl"></bean>
-->

<!-- 第二种方式；使用普通工厂中的方法创建对象（使用某个类中的方法创建对象，并存入spring容器）
<bean id="instanceFactory" class="com.liruicong.factory.InstanceFactory"></bean>
<bean id="accountService" factory-bean="instanceFactory" factory-method="getAccountService"></bean>
-->

<!-- 第三种方式：使用工厂中的静态方法创建对象（使用某个类中的静态方法创建对象，并存入spring容器） -->
<bean id="accountService" class="com.liruicong.factory.StaticFactory" factory-method="getAccountService"></bean>
```

Spring中Bean的作用范围：

![Snipaste_2020-11-22_10-56-38.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Snipaste_2020-11-22_10-56-38.png?raw=true)

Spring中Bean对象的生命周期：

```xml
<!-- Bean对象的生命周期
    单例对象
        出生：当容器创建时对象出生
        活着：只要容器还在，对象一直活着
        死亡：容器销毁，对象消亡
        总结：单例对象的生命周期和容器相同
    多例对象
        出生：当我们使用对象时spring框架为我们创建
        活着：对象只要是在使用过程中就一直活着
        死亡：当对象长时间不用且没有别的对象引用时，由Java垃圾回收器回收
-->
```

## 四、依赖注入(Dependency Injection)

```xml
<!-- Spring中的依赖注入
    依赖注入：Dependency Injection
    IOC的作用：降低程序间的耦合（依赖关系）
    依赖关系的管理：以后都交给Spring来维护。在当前类需要用到其他类的对象，由Spring为我们提供，我们只需要在配置文件中说明
    依赖关系的维护：就称之为依赖注入
    依赖注入：
        能注入的数据有三类：
            基本类型和String
            其他的Bean类型（在配置文件中或者注解配置过的Bean）
            复杂类型/集合类型
        注入的方式有三种：
            第一种：使用构造函数提供
            第二种：使用set方法提供
            第三种：使用注解提供
-->

<!-- 构造函数注入
    使用的标签：constructor-arg
    标签出现的位置：bean标签的内部
    标签中的属性
        type：用于指定要注入的数据的数据类型，该数据类型也是构造函数中某个或某些参数的类型
        index：用于指定要注入的数据给构造函数中指定索引位置的参数赋值。参数索引的位置是从0开始
        name：用于指定给构造函数中指定名称的参数赋值
        ====================以上三个用于指定给构造函数中哪个参数赋值=======================
        value：用于提供基本类型和String类型的数据
        ref：用于指定其他的Bean类型的数据。它指的就是在Spring的IOC核心容器中出现过的bean对象

    优势：在获取Bean对象时，注入数据是必须的操作，否则对象无法创建成功
    弊端：改变了Bean对象的实例化方式，使我们在创建对象时，如果用不到这些数据，也必须提供
-->
<!-- set方法注入  更常用的方式
        涉及的标签：property
        出现的位置：bean标签的内部
        标签的属性
            name：用于指定注入时所调用的set方法名称
            value：用于提供基本类型和String类型的数据
            ref：用于指定其他的Bean类型的数据。它指的就是在Spring的IOC核心容器中出现过的bean对象

        优势：创建对象时没有明确的限制，可以直接使用默认构造函数
        弊端：如果有某个成员必须有值，则获取对象时有可能set方法没有执行。
-->
<!-- 复杂类型的注入/集合类型的注入
        用于给List结构集合注入的标签：
        list array set
        用于给Map结构集合注入的标签：
        map props
        结构相同，标签可以互换
-->
```

## 五、Spring中IOC的常用注解

```
/**
 * 账户的业务层实现类
 * 曾经XML的配置：
 * <bean id="accountService" class="com.liruicong.service.impl.AccountServiceImpl"
 *      scope"" init-method="" destroy-method="">
 *          <property name="" value="" / ref=""></property>
 * </bean>
 * 用于创建对象的：它们的作用就和在xml配置文件中编写一个<bean></bean>标签实现的功能是一样的
 * @Component:
 *      作用：用于把当前类对象存入Spring容器中
 *      属性：
 *          value：用于指定bean的id。当我们不写时，它的默认值是当前类名，且首字母改小写。
 * @Controller
 *      一般用在表现层
 * @Service
 *      一般用在业务层
 * @Repository
 *      一般用于持久层
 *      以上三个注解它们的作用和属性与Component是一模一样的。
 *      他们三个是Spring框架为我们提供明确三层使用的注解，使我们的三层对象更加清晰
 * 用于注入数据的：它们的作用就和在xml配置文件中的<bean></bean>标签中写一个<property></property>标签的作用是一样的
 * @Autowired
 *      作用：自动按照类型注入，只要容器中有唯一的一个bean对象类型和要注入的变量类型匹配，就可以注入成功
 *          如果IOC容器中没有任何bean类型和要注入的变量类型匹配，则报错。
 *          如果IOC容器中有多个类型匹配时：
 *      出现位置：可以是变量上，也可以是方法上
 *      细节：在使用注解注入时，set方法就不是必须的了。
 * @Qualifier
 *      作用：在按照类中注入的基础之上再按照名称注入。它在给类成员注入时不能单独使用。但是在给方法参数注入时可以
 *      属性：
 *          value：用于指定注入bean的id
 * @Resource
 *      作用：直接按照bean的id注入。它可以独立使用
 *      属性：
 *          value：用于指定bean的id
 *      以上三个注解都只能注入其他bean类型的数据，而基本类型和String类型无法使用上述注解实现
 *      另外，集合类型的注入只能通过xml来实现s
 * @Value
 *      作用：用于注入基本类型和String类型的数据
 *      属性：
 *          value：用于指定数据的值。它可以使用spring中的Spel(也就是spring的el表达式)
 *              Spel的写法：${表达式}
 * 用于改变作用范围的：它们的作用就和在<bean></bean>标签中使用scope属性实现的功能是一样的
 * @Scope
 *      作用：用于指定bean的作用范围
 *      属性：
 *          value：指定范围的取值。常用取值：singleton prototype
 * 和生命周期相关：它们的作用就和在<bean></bean>标签中使用init-method和destroy-method的作用是一样的(了解)
 * @PreDestroy
 *      作用：用于指定销毁方法
 * @PostConstruct
 *      作用：用于指定初始化方法
 */
```

Autowired执行原理：先按照类型自动注入，如果有唯一bean对象就注入成功，如果没有任何bean类型相匹配就报错。如果有多个匹配，再按照变量名称匹配。

![Snipaste_2020-11-22_15-33-55.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Snipaste_2020-11-22_15-33-55.png?raw=true)

## 六、案例使用xml方式和注解方式实现单表的CRUD操作

### 持久层技术选择：dbutils

## 七、改造基于注解的IOC案例，使用纯注解的方式实现

### Spring的一些新注解使用

```java
/**
 * 该类是一个配置类，它的作用和bean.xml是一样的
 * Spring中的新注解
 * @Configuration
 *      作用：指定当前类是一个配置类
 *      细节：当配置类作为AnnotationConfigApplicationContext对象创建的参数时，该注解可以不写。
 * @ComponentScan
 *      作用：用于通过注解指定Spring在创建容器时要扫描的包
 *      属性：
 *          value：它和basePackages的作用是一样的，都是用于指定创建容器时要扫描的包
 *          我们使用此注解就等同于在xml中配置了：
 *          <context:component-scan base-package="com.liruicong"></context:component-scan>
 * @Bean
 *      作用：用于把当前方法的返回值作为bean对象存入Spring的ioc容器中
 *      属性：
 *          name：用于指定bean的id，当不写时，默认值是当前方法的名称
 *      细节：
 *          当我们使用注解配置方法时，如果方法有参数，spring框架会去容器中查找有没有可用的bean对象。
 *          查找的方式和Autowired注解的作用一样的
 * @Import
 *      作用：用于导入其他的配置类
 *      属性：
 *          value：用于指定其他配置类的字节码。
 *              当我们使用Import的注解之后，有Import注解的类就是父配置类，而导入的都是子配置类
 * @PropertySource
 *      作用：用于指定properties文件的位置
 *      属性：
 *          value：指定文件的名称和路径
 *              关键字：classpath，表示类路径下
 *
 */
```

## 八、Spring和Junit整合

1. 应用程序入口：main方法

2. junit单元测试中，没有main方法也能执行

   Junit集成了一个main方法，该方法就会判断当前测试类中哪些方法有@Test注解，Junit就让有Test注解的方法执行

3. Junit不会管我们是否采用Spring框架，在执行测试方法时，Junit根本不知道我们是不是使用了Spring框架，所以也就不会为我们读取配置文件/配置类创建Spring核心容器

4. 由以上三点可知，当测试方法执行时，没有IOC容器，就算写了Autowired注解，也无法实现注入。

```java
/**
 * 使用Junit单元测试：测试配置
 * Spring整合Junit的配置
 *      1、导入Spring整合Junit的jar(坐标)
 *      2、使用Junit提供的一个注解把原有的main方法替换了，替换成Spring提供的
 *          @Runwith
 *      3、告知Spring的运行器，Spring和IOC创建是基于XML还是注解的，并且说明位置
 *          @ContextConfiguration
 *              locations:指定xml文件位置，加上classpath关键字，表示在类路径下
 *              classes:指定注解类所在的位置
 *      4、当我们使用Spring 5.x版本的时候，要求Junit的jar包必须是4.12及以上
 */
```

## 九、AOP的概念

**AOP：全称是Aspect Oriented Programming， 即：面向切面编程。**

简单的说它就是把我们程序重复的代码抽取出来，在需要执行的时候，使用动态代理的技术，在不修改源码的基础上，对我们的已有方法进行增强。

**优势：**

- 减少重复代码
- 提高开发效率
- 维护方便

**实现方式：**使用动态代理技术

```java
/**
 * 动态代理：
 * 特点：字节码随用随创建，随用随加载
 * 作用：在不修改源码的基础上对方法增强
 * 分类：
 *      基于接口的动态代理
 *      基于子类的动态代理
 * 基于子类的动态代理：
 *      涉及的类：Enhancer
 *      提供者：第三方cglib库
 * 如何创建代理对象：
 *      使用Enhancer类中的create方法
 * 创建代理类的要求：
 *      被代理类不能是最终类
 * create方法的参数：
 *      Class:字节码
 *          用于指定被代理对象的字节码
 *      Callback：用于提供增强代码
 *          让我们学如何代理，我们一般都是写一些该接口的实现类，通常情况下都是匿名内部类，但不是必须的。
 *          此接口的实现类都是谁用谁写
 *          我们一般写的都是该接口的子接口实现类：MethodInterceptor
 */
```

## 十、Spring中的AOP相关术语

**Jointpoint(连接点)：**所谓连接点是指那些被拦截到的点。在Spring中，这些点指的是方法，因为Spring只支持方法类型的连接点。

**Pointcut(切入点)：**所谓切入点是指我们要对哪些Jointpoint进行拦截的定义。

**Advice(通知/增强)：**所谓通知是指拦截到Jointpoint之后要做的事情

通知的类型：

- 前置通知，后置通知，异常通知，最终通知，环绕通知

![Snipaste_2020-12-03_08-56-42.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Snipaste_2020-12-03_08-56-42.png?raw=true)

```java
/**
 * 环绕通知
 * 问题：
 *      当我们配置了环绕通知之后，切入点方法没有执行，而通知方法执行了。
 * 分析：
 *      通过对比动态代理中的环绕通知代码，发现动态代理的环绕通知有明确的切入点方法调用，而我们的代码中没有。
 * 解决：
 *      Spring框架为我们提供了一个接口：ProceedingJoinPoint。该接口有一个方法proceed()，此方法就相当于明确调用切入点方法。
 *      该接口可以作为环绕通知的方法参数，在程序执行时，Spring框架会为我们提供该接口的实现类供我们使用。
 * Spring中的环绕通知：
 *      它是Spring框架为我们提供的一种可以在代码中手动控制增强方法何时执行的方式。
 */
```

**Introduction(引介)：**引介是一种特殊的通知，在不修改类代码的前提下，Introduction可以在运行期为类动态地添加一些方法或Field。

**Target(目标对象)：**代理的目标对象。

**Weaving(织入)：**是指把增强应用到目标对象来创建新的代理对象的过程。Spring采用动态代理织入，而AspectJ采用编译期织入和类装载期织入。

**Proxy(代理)：**一个类被AOP织入增强后，就产生一个结果代理类。

**Aspect(切面)：**是切入点和通知(引介)的结合



学习Spring中的AOP要明确的事

- 开发阶段(我们做的)

  编写核心业务代码(开发主线)：大部分程序员来做，要求熟悉业务需求

  把公用代码抽取出来，制作成通知。(开发阶段最后再做)：AOP编程人员来做

  在配置文件中，声明切入点和通知的关系，即切面：AOP编程人员来做

- 运行阶段(Spring框架完成的)

  Spring框架监控切入点方法的执行。一旦监控到切入点方法被运行，使用代理机制，动态创建目标对象的代理对象，根据通知类别，在代理对象的对应位置，将通知对应的功能织入，完成代码的逻辑运行。



## 十一、Spring中基于XML和注解的AOP配置

```xml
<!-- Spring中基于XML的AOP配置步骤
    1、把通知Bean也交给Spring来管理
    2、使用aop:config标签表明开始AOP的配置
    3、使用aop:aspect标签表明配置切面
            id属性：是给切面提供一个唯一标识
            ref属性：是指定通知类bean的Id。
    4、在aop:aspect标签的内部使用对应标签来配置通知的类型
            我们现在的示例是让printLog方法在切入点方法执行之前执行：所以是前置通知
            aop:before:表示配置前置通知
                method属性：用于指定Logger类中哪个方法是前置通知
                pointcut属性：用于指定切入点表达式，该表达式的含义指的是对业务层中哪些方法增强
        切入点表达式的写法：
            关键字：execution(表达式)
            表达式：
                访问修饰符 返回值 包名.包名.包名...类名.方法名(参数列表)
            标准的表达式写法：
                public void com.liruicong.service.impl.AccountServiceImpl.saveAccount()
            访问修饰符可以省略
                void com.liruicong.service.impl.AccountServiceImpl.saveAccount()
            返回值可以使用通配符，表示任意返回值
                * com.liruicong.service.impl.AccountServiceImpl.saveAccount()
            包名可以使用通配符，表示任意包。但是有几级包，就需要写几个*.
                * *.*.*.*.AccountServiceImpl.saveAccount()
            包名可以使用..表示当前包及其子包
                * *..AccountServiceImpl.saveAccount()
            类名和方法名都可以使用*来实现通配
                * *..*.*()
            参数列表：
                可以直接写数据类型：
                    基本类型直接写名称   int
                    引用类型写包名.类名的方法   java.lang.String
                可以使用通配符表示任意类型，但是必须有参数
                可以使用..表示有无参数均可，有参数可以是任意类型
            全通配写法：
                * *..*.*(..)
            实际开发中切入点表达式的通常写法：
                切到业务层实现类下的所有方法
                    * com.liruicong.service.impl.*.*(..)
-->
```

## 十二、Spring中的JdbcTemplate

它是Spring框架中提供的一个对象，是对原始Jdbc API对象的简单封装。Spring框架为我们提供了很多的操作模板类。

**JdbcTemplate的作用：**用于和数据库交互的，实现对表的CRUD操作

**如何创建该对象：**

**对象中的常用方法：**

![Snipaste_2020-12-04_08-21-18.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Snipaste_2020-12-04_08-21-18.png?raw=true)





## 十三、Spring中的事务控制

### 1、基于XML的

### 2、基于注解的