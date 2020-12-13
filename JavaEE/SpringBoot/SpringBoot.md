# SpringBoot

## 微服务阶段

JaveSE：OOP

MySql：持久化

Html+CSS+JS+Jquery+框架：视图，框架不熟练，CSS不好

JavaWeb：独立开发MVC三层架构的网站：原始

SSM：框架：简化了我们的开发流程，配置也开始较为复杂

**war：Tomcat运行**

Spring再简化：SpringBoot-jar：内嵌Tomcat；微服务架构!

服务越来越多：SpringCloud；

![Snipaste_2020-12-13_09-59-51.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Snipaste_2020-12-13_09-59-51.png?raw=true)



## 一、什么是SpringBoot？

SpringBoot就是一个JavaWeb开发框架，和SpringMVC类似，对比其他JavaWeb框架的好处，官方说是简化开发，约定大于配置。

Spring Boot 基于 Spring 开发，Spirng Boot 本身并不提供 Spring 框架的核心特性以及扩展功能，只是用于快速、敏捷地开发新一代基于 Spring 框架的应用程序。也就是说，它并不是用来替代 Spring 的解决方案，而是和 Spring 框架紧密结合用于提升 Spring 开发者体验的工具。Spring Boot 以**约定大于配置的核心思想**，默认帮我们进行了很多设置，多数 Spring Boot 应用只需要很少的 Spring 配置。同时它集成了大量常用的第三方库配置（例如 Redis、MongoDB、Jpa、RabbitMQ、Quartz 等等），Spring Boot 应用中这些第三方库几乎可以零配置的开箱即用。

**SpringBoot的优点：**

- 为所有Spring开发者更快的入门
- **开箱即用**，提供各种默认配置来简化项目配置
- 内嵌式容器简化Web项目
- 没有冗余代码生成和XML配置的要求

## 二、什么是微服务？

微服务是一种架构风格，它要求我们在开发一个应用的时候，这个应用必须构建成一系列小服务的组合；可以通过http的方式进行互通。

all in one的架构方式，我们把所有的功能单元放在一个应用里面。然后我们把整个应用部署到服务器上。

所谓微服务架构，就是打破之前all in one的架构方式，把每个功能元素独立出来。把独立出来的功能元素动态组合，需要的功能元素才去拿来组合。需要多一些时可以整合多个功能元素。所以微服务架构是对功能元素进行复制，而没有对整个应用进行复制。

**这样做的好处：**

- 节省了调度资源
- 每个功能元素的服务都是一个可替换的、可独立升级的软件代码。

## 三、第一个SpringBoot程序

**准备工作：**

- JDK11
- Maven3.6.3
- SpringBoot
- IDEA

官方：提供了一个快速生成的网站！IDEA集成了这个网站

- 可以在官网直接下载后，导入IDEA开发()
- 直接使用IDEA创建一个SpringBoot项目(一般开发直接在IDEA中创建)