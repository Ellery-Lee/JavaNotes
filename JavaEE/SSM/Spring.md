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

## 四、依赖注入(Dependency Injection)