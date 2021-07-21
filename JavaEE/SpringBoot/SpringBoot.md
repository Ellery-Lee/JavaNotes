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

## 四、自动装配原理

```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testCompile group: 'junit', name: 'junit', version: '4.12'
}
```

**启动器：**SpringBoot的启动场景

- 比如spring-boot-starter-web，他就会帮我们自动导入web环境所有的依赖
- SpringBoot会将所有的功能场景，都变成一个个的启动器
- 我们要使用什么功能，就只需要找到对应的启动器就可以了`starter`

## 五、主程序

```java
//主程序入口
//@SpringBootApplication:标注这个类是一个springboot的应用：启动类下的所有资源被导入
@SpringBootApplication
public class Springboot01HelloworldApplication {
    //SpringBootApplication
    public static void main(String[] args) {
        //将springboot应用启动
        SpringApplication.run(Springboot01HelloworldApplication.class, args);
    }

}
```

- 注解

  - ```java
    @SpringBootConfiguration：Springboot的配置
        @Configuration：spring配置类
        @Component：说明这也是一个spring组件
    
    @EnableAutoConfiguration：自动配置
        @AutoConfigurationPackage：自动配置包
        	@Import({Registrar.class})：导入注册器
        @Import({AutoConfigurationImportSelector.class})：自动配置导入选择器
    ```

## 六、配置文件

SpringBoot使用一个全局的配置文件 ， 配置文件名称是固定的

- application.properties

- - 语法结构 ：key=value

- application.yml

- - 语法结构 ：key：空格 value

**配置文件的作用 ：**修改SpringBoot自动配置的默认值，因为SpringBoot在底层都给我们自动配置好了；

以前的配置文件，大多数都是使用xml来配置；比如一个简单的端口配置，我们来对比下yaml和xml

传统xml配置：

```xml
<server>
    <port>8081<port>
</server>
```

yaml配置：

```yaml
server：
  prot: 8080
```



其他范例

```yaml
#对空格的要求十分高
#注入到配置类中
# 普通的key-value
name: liruicong

# 对象
student:
	name: liruicong
	age: 22
	
#行内写法
student: {name: liruicong,age: 22}

#数组
pets:
	- cat
	- dog
	
pets: [cat,dog,pig]
```

### 1、@ConfigurationProperties vs. @Value

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7KtjyIb9NEaYlz0tCWSiboOYjMibiaov73iaTsiaWEPoArDcAB1Ooibx9uR5JxtacIuicHblEtUI9SrySX2A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

1、@ConfigurationProperties只需要写一次即可 ， @Value则需要每个字段都添加

2、松散绑定：这个什么意思呢? 比如我的yml中写的last-name，这个和lastName是一样的， - 后面跟着的字母默认是大写的。这就是松散绑定。可以测试一下

3、JSR303数据校验 ， 这个就是我们可以在字段是增加一层过滤器验证 ， 可以保证数据的合法性

4、复杂类型封装，yml中可以封装对象 ， 使用value就不支持

**结论：**

配置yml和配置properties都可以获取到值 ， 强烈推荐 yml；

如果我们在某个业务中，只需要获取配置文件中的某个值，可以使用一下 @value；

如果说，我们专门编写了一个JavaBean来和配置文件进行一一映射，就直接@configurationProperties，不要犹豫！

### 2、JSR-303数据校验

Springboot中可以用`@validated`来校验数据，如果数据异常则会统一抛出异常，方便异常中心统一处理。这里来写个注解让name只能支持Email格式

```java
@Component //注册bean
@ConfigurationProperties(prefix = "person")
@Validated  //数据校验
public class Person {

    @Email(message="邮箱格式错误") //name必须是邮箱格式
    private String name;
}
```

**常见参数：**

```
@NotNull(message="名字不能为空")
private String userName;
@Max(value=120,message="年龄最大不能查过120")
private int age;
@Email(message="邮箱格式错误")
private String email;

空检查
@Null       验证对象是否为null
@NotNull    验证对象是否不为null, 无法查检长度为0的字符串
@NotBlank   检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格.
@NotEmpty   检查约束元素是否为NULL或者是EMPTY.
    
Booelan检查
@AssertTrue     验证 Boolean 对象是否为 true  
@AssertFalse    验证 Boolean 对象是否为 false  
    
长度检查
@Size(min=, max=) 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内  
@Length(min=, max=) string is between min and max included.

日期检查
@Past       验证 Date 和 Calendar 对象是否在当前时间之前  
@Future     验证 Date 和 Calendar 对象是否在当前时间之后  
@Pattern    验证 String 对象是否符合正则表达式的规则

.......等等
除此以外，我们还可以自定义一些数据校验规则
```

### 3、多环境切换:

**多配置文件：**

profile是Spring对不同环境提供不同配置功能的支持，可以通过激活不同的环境版本，实现快速切换环境。

我们在主配置文件编写的时候，文件名可以是 application-{profile}.properties/yml , 用来指定多个环境版本：

**例如：**

application-test.properties 代表测试环境配置

application-dev.properties 代表开发环境配置

但是Springboot并不会直接启动这些配置文件，它**默认使用application.properties主配置文件**

我们需要通过一个配置来选择需要激活的环境：

 ```properties
#比如在配置文件中指定使用dev环境，我们可以通过设置不同的端口号进行测试；
#我们启动SpringBoot，就可以看到已经切换到dev下的配置了；
spring.profiles.active=dev
 ```

**yaml多环境文档模块：**

和properties配置文件中一样，但是使用yml去实现不需要创建多个配置文件，更加方便了 !

```yaml

server:
  port: 8081
#选择要激活那个环境块
spring:
  profiles:
    active: prod

---
server:
  port: 8083
spring:
  profiles: dev #配置环境的名称


---

server:
  port: 8084
spring:
  profiles: prod  #配置环境的名称
```

**注意：如果yml和properties同时都配置了端口，并且没有激活其他环境 ， 默认会使用properties配置文件的！**



**配置文件加载位置及优先级：**

外部加载配置文件的方式十分多，我们选择最常用的即可，在开发的资源文件中进行配置！

官方外部配置文件说明参考文档

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7IPEXZtUAUBhnSZvUmrPzbDUoiazZ6ehegLG4doZK0uSJHribIqwVKiaNibSaYZSgjZf4kGzhLdGrkzzw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

springboot 启动会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件：

```
优先级1：项目路径下的config文件夹配置文件
优先级2：项目路径下配置文件
优先级3：资源路径下的config文件夹配置文件
优先级4：资源路径下配置文件
```

**优先级由高到底，高优先级的配置会覆盖低优先级的配置**

我们还可以通过spring.config.location来改变默认的配置文件位置，这种情况，一般是**后期运维**做的多，相同配置，**外部指定的配置文件优先级最高**。

**配置文件加载顺序：**

1. bootstrap.properties/yml
2. application.properties/yml
3. application-dev.properties/yml

### 4、自动配置原理

以**HttpEncodingAutoConfiguration（Http编码自动配置）**为例分析自动配置原理

```java
//表示这是一个配置类，和以前编写的配置文件一样，也可以给容器中添加组件；
@Configuration 

//启动指定类的ConfigurationProperties功能；
  //进入这个HttpProperties查看，将配置文件中对应的值和HttpProperties绑定起来；
  //并把HttpProperties加入到ioc容器中
@EnableConfigurationProperties({HttpProperties.class}) 

//Spring底层@Conditional注解
  //根据不同的条件判断，如果满足指定的条件，整个配置类里面的配置就会生效；
  //这里的意思就是判断当前应用是否是web应用，如果是，当前配置类生效
@ConditionalOnWebApplication(
    type = Type.SERVLET
)

//判断当前项目有没有这个类CharacterEncodingFilter；SpringMVC中进行乱码解决的过滤器；
@ConditionalOnClass({CharacterEncodingFilter.class})

//判断配置文件中是否存在某个配置：spring.http.encoding.enabled；
  //如果不存在，判断也是成立的
  //即使我们配置文件中不配置pring.http.encoding.enabled=true，也是默认生效的；
@ConditionalOnProperty(
    prefix = "spring.http.encoding",
    value = {"enabled"},
    matchIfMissing = true
)

public class HttpEncodingAutoConfiguration {
    //他已经和SpringBoot的配置文件映射了
    private final Encoding properties;
    //只有一个有参构造器的情况下，参数的值就会从容器中拿
    public HttpEncodingAutoConfiguration(HttpProperties properties) {
        this.properties = properties.getEncoding();
    }
    
    //给容器中添加一个组件，这个组件的某些值需要从properties中获取
    @Bean
    @ConditionalOnMissingBean //判断容器没有这个组件？
    public CharacterEncodingFilter characterEncodingFilter() {
        CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
        filter.setEncoding(this.properties.getCharset().name());
        filter.setForceRequestEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.REQUEST));
        filter.setForceResponseEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.RESPONSE));
        return filter;
    }
    //。。。。。。。
}
```

**一句话总结 ：根据当前不同的条件判断，决定这个配置类是否生效！**

- 一但这个配置类生效；这个配置类就会给容器中添加各种组件；
- 这些组件的属性是从对应的properties类中获取的，这些类里面的每一个属性又是和配置文件绑定的；
- 所有在配置文件中能配置的属性都是在xxxxProperties类中封装着；
- 配置文件能配置什么就可以参照某个功能对应的这个属性类

```java
//从配置文件中获取指定的值和bean的属性进行绑定
@ConfigurationProperties(prefix = "spring.http") 
public class HttpProperties {
    // .....
}
```

**精髓：**

1、SpringBoot启动会加载大量的自动配置类

2、我们看我们需要的功能有没有在SpringBoot默认写好的自动配置类当中；

3、我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件存在在其中，我们就不需要再手动配置了）

4、给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们只需要在配置文件中指定这些属性的值即可；

**xxxxAutoConfigurartion：自动配置类；**给容器中添加组件

**xxxxProperties:封装配置文件中相关属性；**

**了解：**

关注一个细节问题，**自动配置类必须在一定的条件下才能生效；**

**@Conditional派生注解（Spring注解版原生的@Conditional作用）**

作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置配里面的所有内容才生效

## 七、静态资源处理

### 方法1、webjars

SpringBoot中，SpringMVC的web配置都在 WebMvcAutoConfiguration 这个配置类里面，我们可以去看看 WebMvcAutoConfigurationAdapter 中有很多配置方法。有一个方法：addResourceHandlers 添加资源处理，所有的 /webjars/** ， 都需要去 classpath:/META-INF/resources/webjars/ 找对应的资源。

**webjars：**Webjars本质就是以jar包的方式引入我们的静态资源 ， 我们以前要导入一个静态资源文件，直接导入即可。

网站：https://www.webjars.org 

访问：只要是静态资源，SpringBoot就会去对应的路径寻找资源，我们这里访问：http://localhost:8080/webjars/jquery/3.4.1/jquery.js

### 方法2、自己的静态资源

我们去找staticPathPattern发现第二种映射规则 ：/** , 访问当前的项目任意资源，它会去找 resourceProperties 这个类，我们可以点进去看一下分析：

```java

// 进入方法
public String[] getStaticLocations() {
    return this.staticLocations;
}
// 找到对应的值
private String[] staticLocations = CLASSPATH_RESOURCE_LOCATIONS;
// 找到路径
private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { 
    "classpath:/META-INF/resources/",
  "classpath:/resources/", 
    "classpath:/static/", 
    "classpath:/public/" 
};
```

ResourceProperties 可以设置和我们静态资源有关的参数；这里面指向了它会去寻找资源的文件夹，即上面数组的内容。

所以得出结论，以下四个目录存放的静态资源可以被我们识别：

 ```
"classpath:/META-INF/resources/"
"classpath:/resources/"
"classpath:/static/"
"classpath:/public/"
 ```

我们可以在resources根目录下新建对应的文件夹，都可以存放我们的静态文件。比如我们访问 http://localhost:8080/1.js , 他就会去这些文件夹中寻找对应的静态资源文件

## 八、模板引擎Thymeleaf

模板引擎的作用就是我们来写一个页面模板，比如有些值是动态的，我们写一些表达式。而这些值就是我们在后台封装一些数据。然后把这个模板和这个数据交给我们模板引擎，模板引擎按照我们这个数据帮你把这表达式解析、填充到我们指定的位置，然后把这个数据最终生成一个我们想要的内容给我们写出去，这就是我们这个模板引擎，不管是jsp还是其他模板引擎，都是这个思想。