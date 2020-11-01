# Servlet

# 一、B/S架构角色和协议

![Snipaste_2020-09-25_10-38-21](D:\JavaHub\学习相关\Java笔记\pictures\Snipaste_2020-09-25_10-38-21.png)

一个WebApp文件夹构成：

- html、css、js文件
- **WEB-INF：**严格大小写，存放字节码文件、库文件jar包以及xml配置文件
  - classes
  - lib
  - web.xml
    - 一个webapp只有一个web.xml文件
    - web.xml文件主要配置请求路径和Servlet类名之间的绑定关系
    - web.xml文件在Tomcat服务器启动阶段被解析
    - web.xml文件解析失败，会导致webapp启动失败
    - web.xml文件中的标签不能随意编写，因为Tomcat服务器早就知道该文件中编写了哪些标签
    - web.xml文件中的标签也是SUN公司制定的Servlet规范

# 二、Servlet对象生命周期

1. 生命周期表示一个java对象从最初被创建到最终被销毁，经历的所有过程

2. Servlet生命周期由谁来管理？程序员可以干涉吗？

   Servlet对象的生命周期，javaWeb程序员是无权干涉的，包括该Servlet对象的相关方法调用，javaWeb程序员也是无权干涉的

   Servlet对象从最初的创建开始，方法的调用，以及最后Servlet对象的销毁，整个过程都是由WEB容器来管理

   **Web Container管理Servlet对象的生命周期**

3. 默认情况下，Servlet对象在Web服务器启动阶段不会被实例化。（若希望在Web服务器启动阶段实例化Servlet对象，需要进行特殊设置）

4. 描述Servlet生命周期

   1)用户在浏览器地址栏上输入URL: http://localhost:8080/webAppName/test

   2)Web容器截取请求路径: /webAppName/test

   3)Web容器在容器上下文中找请求路径 /webAppName/test 对应的Servlet对象

   4)**若没有找到对应的Servlet对象**

   ​	4.1)通过web.xml文件中相关的配置信息，得到请求路径 /webAppName/test 对应的完整类名

   ​	4.2)通过反射机制，调用Servlet类的无参构造方法完成Servlet对象的实例化

   ​	4.3)web容器调用Servlet对象的init方法完成初始化操作

   ​	4.4)web容器调用Servlet对象的service方法提供服务

   5)**若找到对应的Servlet对象**

   ​	5.1)web容器直接调用Servlet对象的service方法提供服务

   6)web容器关闭的时候/webApp重新部署的时候/该Servlet对象长时间没有用户再次访问的时候，web容器会将该Servlet对象销毁，在销毁对象之前，Web容器会调用Servlet对象的destroy方法，完成销毁之前的准备。



Servlet对象实例化之后，这个Servlet对象被存储到哪里了？

> 大多数的Web容器都是将该Servlet对象以及对应的URL-pattern存储到Map集合中：（如果找到就直接用这个集合调用Servlet对象）
>
> 在Web容器中有这样一个Map集合
>
> Map<String, Servlet> 集合

服务器在启动的时候就会解析各个WebApp的web.xml文件，做了什么？

> 将web.xml文件中的url-pattern和对应的Servlet完整类名存储到Map集合中了：（如果没找到就用这个集合调用类名构造方法创建对象）
>
> 在Web容器中有这样一个Map集合
>
> Map<String, String> 集合



**总结：**

- Servlet类的构造方法只执行一次
- Servlet对象的init方法只执行一次
- Servlet对象的service方法，只要用户请求一次，则执行一次
- Servlet对象的destroy方法只执行一次
- init方法执行时，Servlet对象已经被创建好了
- destroy方法执行的时候，Servlet对象还没有被销毁，即将被销毁

- Servlet对象是单例，但是不符合单例模式，只能成为**伪单例**。**真单例**的构造方法是私有化，Tomcat服务器是支持多线程的，所以Servlet对象在单实例多线程的环境下运行的。那么Servlet对象中若有实例变量，并且实例变量涉及到修改操作，那么这个**Servlet对象一定会存在线程安全问题**，不建议在Servlet对象中使用实例变量，尽量使用局部变量。

- 若希望在Web服务器启动阶段实例化Servlet对象，需要在web.xml文件中进行相关的配置，例如：`<load-on-startup>1</load-on-startup>`自然数越小优先级越高



## Servlet接口中方法编写什么代码，什么时候使用这些代码

### 1、无参构造方法

以后就不需要再考虑构造函数了，尽量别动构造函数。

### 2、init()

以上两个方法执行时间几乎相同，执行次数都是一次，构造方法执行的时候对象正在创建，init()方法执行的时候对象已经创建：

- 若系统要求在对象创建时刻执行一段特殊的程序，这段程序尽量写到init方法中。

- 为什么不建议将代码写到构造函数中？

  > 存在风险，当程序员编写构造方法时，可能会导致无参数构造方法不存在。
  >
  > 一个类不编写任何构造函数，默认有一个无参数构造方法，但是一旦编写一个有参数的构造方法之后，系统则不再提供无参数构造函数。

**Servlet中的init方法是SUN公司为javaweb程序员专门提供的一个初始化时刻，若希望再初始化时刻执行一段特殊的程序，这个程序可以编写到init方法，将来会被自动调用。**

### 3、service()

这个方法是必然要重写的，因为在这个方法需要完成业务逻辑的处理，请求的处理，以及完成响应。而且这个方法中的代码是最有价值的。也是最难写的，因为最难编写的就是业务代码。

### 4、destroy()

这个方法也是SUN公司为javaweb程序员提供的一个特殊的时刻，这个特殊的时刻被称为对象的销毁时刻，若希望再销毁时刻执行一段特殊的代码，需要将这段代码编写到destroy方法，自动被容器调用。

**回顾：**类加载时刻执行程序，代码写到哪里？

> 编写到静态代码块中

**结论：**SUN公司为我们程序员提供了很多个不同的时刻。若在这个特殊时刻执行特殊程序，这些程序是有位置编写的。

# 三、UML

## 1、什么是UML

- 统一建模语言，1997年OMG组织指定的图标式语言，在系统设计阶段由系统分析师使用的语言，使用这些图标语言体现设计思想。
- 后期程序员开发的依据就是这些设计图
- UML和任何一个编程语言无关，是一个独立的学科
- UML属于软件工程学

## 2、UML建模工具

- StarUML
- Rational Rose

## 3、UML图有哪些

- 类图：描述类的信息以及类和类之间的关系

  ![Snipaste_2020-11-01_18-44-43](D:\JavaHub\学习相关\Java笔记\pictures\Snipaste_2020-11-01_18-44-43.png)

- 时序图：描述一个程序的执行过程

  ![Snipaste_2020-11-01_18-46-43](D:\JavaHub\学习相关\Java笔记\pictures\Snipaste_2020-11-01_18-46-43.png)

- 用例图：站在系统用户的角度分析系统中存在哪些功能

  ![Snipaste_2020-11-01_18-46-17](D:\JavaHub\学习相关\Java笔记\pictures\Snipaste_2020-11-01_18-46-17.png)

- 状态图：描述一个对象的生命周期

# 四、ServletConfig接口

1. javax.servlet.ServletConfig是接口

2. Apache Tomcat服务器实现了Servlet规范，Tomcat服务器专门写了一个ServletConfig接口的实现类，实现类的完整类名：org.apache.catalina.core.StandardWrapperFacade（作为了解，主要是思想）

3. javaWeb程序员在编程的时候，一直是面向ServletConfig接口去完成调用，不需要关系具体的实现类。

   webapp放在Tomcat服务器中，ServletConfig的实现类是：org.apache.catalina.core.StandardWrapperFacade

   webapp放到JBOSS服务器中，ServletConfig的实现类可能是另外一个类名了。

   这些所有的实现类，我们都不需要关心，只需要学习ServletConfig接口中有哪些可以使用的方法。

4. Tomcat服务器是一个实现了Servlet规范和JSP规范的容器。

5. ServletConfig接口中有哪些常用的方法？

   | `String`              | `getInitParameter(String name)`Gets the value of the initialization parameter with the given name. |
   | --------------------- | ------------------------------------------------------------ |
   | `Enumeration<String>` | `getInitParameterNames()`Returns the names of the servlet's initialization parameters as an `Enumeration` of `String` objects, or an empty `Enumeration` if the servlet has no initialization parameters. |
   | `ServletContext`      | `getServletContext()`Returns a reference to the [`ServletContext`](https://docs.oracle.com/javaee/7/api/javax/servlet/ServletContext.html) in which the caller is executing. |
   | `String`              | `getServletName()`Returns the name of this servlet instance. |

6. ServletConfig到底是什么？

   - **ServletConfig是一个Servlet对象的配置信息对象**，ServletConfig对象中封装了一个Servlet对象的配置信息。Servlet对象的配置信息到web.xml文件中
   - 一个Servlet对象对应一个ServletConfig对象，100个Servlet对象对应100个ServletConfig对象。

7. 将init方法上的ServletConfig参数移动到service方法中，因为我们程序员主要编写的方法是service方法，在service方法中我们可能需要使用ServletConfig！！！！

   **在init方法中完成，局部变量Config赋值给实例变量Config**

   **实现getServletConfig方法，提供公开的get方法，目的是供子类使用**

# 五、ServletContext接口

1. javax.servlet.ServletContext接口，Servlet规范

2. Tomcat服务器对ServletContext接口的实现类的完整类名：org.apache.catalina.core.ApplicationContextFacade

   javawen程序员还是只需要面向ServletContext接口调用方法即可，不需要关心Tomcat具体的实现。

3. ServletContext到底是什么？什么时候创建？什么时候被销毁？创建几个？

   - ServletContext被翻译为：Servlet上下文【Context一般都翻译为上下文】
   - 一个webapp只有一个web.xml文件，web.xml文件在服务器启动阶段被解析
   - 一个webapp只有一个ServletContext对象，ServletContext在服务器启动阶段被实例化
   - ServletContext在服务器关闭的时候会被销毁
   - ServletContext对应的是web.xml文件，是web.xml文件的代表
   - ServletContext是所有Servlet对象四周环境的代表（在同一个webapp中，所有的Servlet对象共享同一个四周环境对象，该对象就是ServletContext），一般把ServletContext变量命名为：application，这个对象代表一个应用，范围极大。
   - 所有的用户若想共享同一个数据，可以将这个数据放到ServletContext对象中。
   - 一般放到ServletContext对象中的数据是不建议涉及到修改操作的，因为ServletContext是多线程共享的一个对象。修改的时候会存在线程安全问题。

4. ServletContext接口中有哪些常用的方法？

   | `Object`              | `getAttribute(String name)`Returns the servlet container attribute with the given name, or `null` if there is no attribute by that name. | 向ServletContext范围中添加数据  |
   | --------------------- | ------------------------------------------------------------ | ------------------------------- |
   | `void`                | `setAttribute(String name, Object object)`Binds an object to a given attribute name in this ServletContext. | 从ServletContext范围中获取数据  |
   | `void`                | `removeAttribute(String name)`Removes the attribute with the given name from this ServletContext. | 移除ServletContext范围中的数据  |
   | `String`              | `getInitParameter(String name)`Returns a `String` containing the value of the named context-wide initialization parameter, or `null` if the parameter does not exist. | 通过上下文初始化的name获取value |
   | `Enumeration<String>` | `getInitParameterNames()`Returns the names of the context's initialization parameters as an `Enumeration` of `String` objects, or an empty `Enumeration` if the context has no initialization parameters. | 获取所有上下文初始化参数的name  |
   | `String`              | `getRealPath(String path)`Gets the *real* path corresponding to the given *virtual* path. | 获取文件绝对路径                |

   

5. Servlet、ServletConfig、ServletContext之间的关系？

- 一个Servlet对应一个ServletConfig     100个Servlet对应100个ServletConfig
- 所有的Servlet共享一个ServletContext对象

6. ServletContext范围可以完成跨用户传递数据

**总结：**

所有编写过的路径：

- 超链接  <a href="/webappname/dosome"></a>

- web.xml中的url-pattern  <url-pattern>/dosome</url-pattern>

- form表单的action属性    <form action="/webappname/dosome"></form>

- application.getRealPath("/WEB-INF/resources/db.properties");

- 欢迎页面路径：<welcome-file>html/welcome.html</welcome-file>

  

# 六、WebApp欢迎界面的设计

## 1、欢迎界面怎么设计？

假设在Webroot目录下创建login.html，想让login.html作为整个webapp的欢迎页面，应该做这样的设置，编写web.xml文件

```xml
<welcome-file-list>
        <welcome-file>login.html</welcome-file>
</welcome-file-list>
```

假设在Web目录下创建html目录，html目录中创建welcome.html，想让welcome.html作为整个webapp的欢迎页面，应该这样设置，编写web.xml文件

```xml
<welcome-file-list>
        <welcome-file>html/welcome.html</welcome-file>
</welcome-file-list>
```



## 2、为什么设计欢迎界面？

为了提高用户体验，访问方便

设置欢迎页面后，直接在浏览器地址栏上访问该webapp即可，自动定位到欢迎界面，例如http://localhost:8080/ServletApp/ 直接访问login.html

- 欢迎页面可以设置多个，越考上，优先级越高。

```xml
<welcome-file-list>
		<welcome-file>login.html</welcome-file>
        <welcome-file>html/welcome.html</welcome-file>
</welcome-file-list>
```

- 以上的配置，优先选择login.html，若这个资源不存在，才会选择html/welcome.html作为欢迎页面

- **注意：**欢迎页面设计的时候，路径不需要以"/"开始

  ```xml
  <welcome-file>login.html</welcome-file>
  ```

  要求在webapp的根目录下，必须有一个文件叫login.html

  ```xml
  <welcome-file>html/welcome.html</welcome-file>
  ```

  要求在webapp的根目录下，必须有一个文件夹，叫做html，该文件夹中必须有一个文件叫做：welcome.html

- 一个webapp的欢迎页面不一定是一个html资源，可以是任何一种类型的web资源，欢迎页面可以是Servlet

- 欢迎页面包括全局配置和局部配置

  - 全局配置：CATALINA_HOME/conf/web.xml
  - 局部配置：CATALINA_HOME/webapp/web.xml
  - 遵守**就近原则**，若一个页面的名称是index.html、index.htm、index.jsp，这些都是默认的欢迎页面，在全局配置中已经配置过了。

# 七、HTTP状态码

## 1、webapp中常见的错误代码

- 404-Not Found（资源未找到：请求的资源路径写错）

- 500-Server Inner Error（服务器内部错误：一般是java程序出现异常）

  404和500是HTTP协议状态码

  以上的这些状态号是W3C指定的，所有的浏览器和服务器都必须遵守

  正常相应的HTTP协议状态码：200

- 在一些错误发生之后统一进行错误的处理，可以在web.xml文件中做一下配置

  ```xml
  <error-page>
      <error-code>404</error-code>
      <location>/error/error.html</location>
  </error-page>
  ```

## 2、路径总结

- 以“/”开始，加Webapp名称
- 以“/”开始，不加Webapp名称
- 不以“/”开始，也不加Webapp名称（只有欢迎页面会有这样的路径）

# 八、适配器

- 当项目中的程序不使用适配器模式，分析程序存在的缺点：

  当前程序中A、B、C类只需要使用CommonIn接口中的m1，m2，m3方法，让这些类直接实现这个接口，需要实现更多的方法，代码丑陋。

- 使用适配器模式优点：

  代码优雅

- 设计模式的分类

  - 创建型：解决对象的创建问题
  - 行为型：该模式与方法、行为、算法有关的设计模式
  - 结构型：更多类，更多的对象组合成更大的结构解决某个特定问题

- 有哪些设计模式？

  - Gof95(1995年，四人组提出的23种设计模式)
    - 单例模式
    - 工厂模式
    - 适配器模式
    - 迭代模式【集合】
    - 策略模式【集合】
    - 装饰器模式【IO流】
  - JavaEE设计模式

我们目前所有的Servlet类直接实现了javax.servlet.Servlet接口，但是这个接口中有很多方法是目前不需要的，我们可能只需要填写service。直接实现Servlet接口代码丑陋，有必要在中间添加一个适配器，以后所有的Servlet类不再直接实现Servlet接口，应该去继承适配器类。

适配器除了可以让代码优雅之外，我们还可以在适配器中提供大量方法，子类继承之后，可以在子类中直接使用，方便编程。

> GenericServlet是一个适配器，这个适配器是一个Servlet，以后javaWeb程序员无需直接实现Servlet接口，去继承这个适配器即可，重写service方法。

# 九、HttpServletRequest接口

1. HttpServletRequest是一个接口，Servlet规范中重要接口之一

2. 继承关系：**public interface** HTTPServletRequest **extends** ServletRequest{}

3. HttpServletRequest接口的实现类是Web容器负责实现的，Tomcat服务器有自己的实现。但是程序员还是只需要面向HttpServletRequest接口调用方法即可，不需要关心具体的实现类。

4. HttpServletRequest这个对象中封装了哪些信息？

   封装了HTTP请求协议的全部内容

   - 请求方式
   - URL
   - 协议版本号
   - 表单提交的数据
   - ....

5. HttpServletRequest一般变量的名字叫做：request，表示请求，HttpServletRequest对象代表一次请求，一次请求对应一个request对象，100个请求对应100个request对象，所以request对象的生命周期是短暂的。

   什么是一次请求？

   - 到目前为止，我们可以这样理解一次请求：在网页上点击超链接，到最终网页停下来，这就是一次完整的请求。

6. 表单提交的数据，会被自动封装在request对象中，request对象中又Map集合存储这些数据：Map集合的key是name，value是一个字符串类型的一维数组。

   | `String`               | `getParameter(String name)`Returns the value of a request parameter as a `String`, or `null` if the parameter does not exist. | 通过指定key获取value这个一维数组的**首元**素（通常情况下一维数组就只有一个元素） |
   | ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
   | `String[]`             | `getParameterValues(String name)`Returns an array of `String` objects containing all of the values the given request parameter has, or `null` if the parameter does not exist. | 通过Map集合key获取value                                      |
   | `Enumeration<String>`  | `getParameterNames()`Returns an `Enumeration` of `String` objects containing the names of the parameters contained in this request. | 获取这个集合所有key；                                        |
   | `Map<String,String[]>` | `getParameterMap()`Returns a java.util.Map of the parameters of this request. | 获取所有参数对应的Map，其中key为参数名，value为参数值。      |
   | `Object`               | `getAttribute(String name)`Returns the value of the named attribute as an `Object`, or `null` if no attribute of the given name exists. | 从request范围中读取数据                                      |
   | `void`                 | `setAttribute(String name, Object o)`Stores an attribute in this request. | 向request范围存储数据                                        |
   | `void`                 | `removeAttribute(String name)`Removes an attribute from this request. | 移除request范围中的数据                                      |
   | `String`               | `getRemoteAddr()`Returns the Internet Protocol (IP) address of the client or last proxy that sent the request. | 获取客户端IP地址                                             |
   | `RequestDispatcher`    | `getRequestDispatcher(String path)`Returns a [`RequestDispatcher`](https://docs.oracle.com/javaee/7/api/javax/servlet/RequestDispatcher.html) object that acts as a wrapper for the resource located at the given path. | 获取请求转发器，让转发器对象指向某个资源                     |
   | `void`                 | `setCharacterEncoding(String env)`Overrides the name of the character encoding used in the body of this request. |                                                              |

   | `String`       | `getContextPath()`Returns the portion of the request URI that indicates the context of the request. | 获取上下文路径【webapp根路径】              |
   | -------------- | ------------------------------------------------------------ | ------------------------------------------- |
   | `String`       | `getMethod()`Returns the name of the HTTP method with which this request was made, for example, GET, POST, or PUT. | 获取浏览器请求方式                          |
   | `String`       | `getRequestURI()`Returns the part of this request's URL from the protocol name up to the query string in the first line of the HTTP request. | 获取请求的URI                               |
   | `StringBuffer` | `getRequestURL()`Reconstructs the URL the client used to make the request. | 获取请求的URL                               |
   | `String`       | `getServletPath()`Returns the part of this request's URL that calls the servlet. | 获取请求的Servlet Path（url-pattern）       |
   | `Cookie[]`     | `getCookies()`Returns an array containing all of the `Cookie` objects the client sent with this request. |                                             |
   | `HttpSession`  | `getSession()`Returns the current session associated with this request, or if the request does not have a session, creates one. | 获取当前session，获取不到，则新建session    |
   | `HttpSession`  | `getSession(boolean create)`                                 | 若为true则跟上面一样，若为false，则返回null |

7. HttpServletRequest是一个怎样的范围呢？

   HttpServletRequest类型的变量通常命名为：request，代表当前的本次请求，一次请求对应一个request对象，100个请求对应100个请求对象。请求范围是极小的。请求范围是极小的，request只能完成在同一请求中传递数据。

   - **跳转：**执行完AServlet之后，跳转到BServlet执行，将AServlet执行和BServlet执行放在同一个请求当中，必须使用转发技术

   - **forward【转发】**

     1. 获取请求转发器对象（以下转发器指向了BServlet）

        ```java
        RequestDispatcher dispatcher = request.getRequestDispatcher("/b");
        ```

     2. 调用请求转发器的forward方法即可完成转发

        ```
        dispatcher.forward(request, response);
        ```

        转发

        ```java
        request.getRequestDispatcher("/b").forward(request, response);
        ```

8. 关于范围对象的选择：

   - ServletContext应用范围，可以跨用户传递数据
   - ServletRequest请求范围，只能在同一个请求中传递数据【可以跨Servlet传递数据，但是这多个Servlet必须在同一个请求当中】
   - **优先选择Request范围**
   
   

# 十、关于项目中的乱码

**乱码经常出现在什么位置上？**

- 数据传递过程中的乱码
- 数据展示过程中的乱码
- 数据保存过程中的乱码

**数据保存过程中的乱码**

- 最终保存到数据库表中的时候，数据出现乱码
- 导致数据保存过程中的乱码包括以下两种情况：
  - 第一种：在保存之前，数据本身就是乱码，保存到数据库中的时候一定时乱码
  - 第二种：保存之前，数据不是乱码，但是由于本身数据库不支持简体中文，保存之后出现乱码

**数据展示过程中的乱码**

- 最终显示在网页上的数据出现中文乱码

- 经过执行java程序之后，java程序负责向浏览器响应的时候，中文出现乱码，怎么解决：

  - 设置响应的内容类型，以及对应的字符编码方式：response.setContentType("text/html;charset=UTF-8");

- 没有经过java执行程序，直接访问html页面，出现中文乱码，怎么解决：

  - <meta content="text/html";charset=UTF-8>

**数据传递过程中的乱码？**

- 将数据从浏览器发送给服务器的时候，服务器接收到的数据是乱码。

- 浏览器是这样发送数据给服务器的：dname=%E5%H6...

- 上面的编码是ISO-8859-1的编码

- ISO-8859-1是国际标准码，不支持中文编码，兼容ASCII码，又被称为latin1编码

- 不管是哪个国家的文字，在浏览器发送给服务器的时候，都会采用ISO-8859-1的编码方式发送。

  - 发送给web服务器之后，web服务器不知道这些数据之前是什么类型的文字
  - 所以web服务器接收到的数据出现乱码

- **解决方式：**

  - 第一种：万能方式，既能够解决POST请求乱码，又能解决GET请求乱码

    - 先将服务器中接收到的数据采用ISO-8859-1的方式解码，回归原始状态，再给定一种支持简体中文的编码方式重新编码组装。

      ```java
      byte[] bytes = dname.getBytes("ISO-8859-1");//解码
      dname = new String(bytes, "UTF-8");//编码【这里的编码方式，需要和浏览器的编码方式一致】
      System.out.println(dname);
      ```

  - 第二种：只支持POST请求，只对请求体编码

    - 调用request的setCharacterEncoding方法，告诉Tomcat服务器，请求体中的数据采用UTF-8的方式进行编码

      ```java
      request.setCharacterEncoding("UTF-8");//必须在获取数据之前设置有效果
      String dname = request.getParameter("dname");
      System.out.println(dname);
      ```

  - 第三种：专门解决get请求，只对请求行编码

    - 修改CATALINA_HOME/conf/server.xml文件

# 十、Servlet线程安全问题

1. Servlet是单实例多线程环境下运行的
2. 什么时候程序存在线程安全问题？
   - 多线程并发
   - 有共享的数据
   - 共享数据有修改操作
3. 在JVM中，哪些数据可能会存在线程安全问题？
   - 局部变量内存空间不共享，一个线程一个栈，局部变量在栈中存储，局部变量不会存在线程安全问题
   - 常量不会被修改，所以常量不会存在线程安全问题
   - 所有线程共享一个堆
     - 堆内存new出来的对象，在其中存储，对象内部有“实例变量”，所以“实例变量”的内存多线程是共享的
     - 实例变量多线程共同访问，并且涉及到修改操作的时候就会存在线程安全问题
   - 所有线程共享一个方法区
     - 方法区中有静态变量（存疑，JVM课程中说静态变量在堆中class之后存储），静态变量的内存也是共享的，若涉及到修改操作，静态变量也存在线程安全问题
4. 线程安全问题不止是体现在JVM中，还有可能发生在数据库中，例如：多线程共享同一张表，并且同时去修改表中一些记录，那么这些记录就存在线程安全问题，怎么解决数据库表中数据的线程安全问题呢？有两种方案：
   - 第一种：在java程序中使用synchronized关键字，线程排队执行，自然不会在数据库中并发，解决线程安全问题。
   - 第二种：行级锁（悲观锁）
   - 第三种：事务隔离级别，例如：串行化
   - 第四种：乐观锁......
5. 怎么解决线程安全问题？
   - 不使用实例变量，尽量使用局部变量
   - 若必须使用实例变量，那么我们可以考虑将该对象变成多例对象，一个线程一个java对象，实例变量的内存也不会共享
   - 若必须使用单例，那就只能使用synchronized线程同步机制，线程一旦排队执行，则吞吐量降低，降低用户体验
6. Servlet中怎么解决线程安全问题？
   - 不使用实例变量，尽量使用局部变量
   - Servlet必须是单例的，所以剩下的方式只能考虑使用synchronized，线程同步机制

# 十一、转发和重定向

关于web系统中资源跳转

1. 跳转包括两种方式：
   - 转发- forward
   - 重定向- redirect
2. 转发和重定向代码怎么完成
   - 转发：request.getRequestDispatcher("/b").forward(request, response);
   - 重定向：response.sendRedirect(request.getContextPath() + "/b");
3. 转发和重定向的相同点和不同点：
   - 相同点：都可以完成资源跳转
   - 不同点：
     - 转发是request对象触发
     - 重定向是response对象触发的
     - 转发是一次请求，浏览器地址栏上地址不会变化【/a】
     - 重定向是两次请求，浏览器地址栏上的地址发生变化【/a，/b】
     - 重定向的路径需要加webapp根路径
     - 转发是在本项目内部完成资源跳转
     - 重定向可以完成跨app跳转资源
4. 跳转的下一个资源可以是什么？
   
- 跳转的下一个资源可以是web服务器中任何一种资源，可以是Servlet，也可以是HTML,也可以使用JSP......
  
5. 什么时候采用转发，什么时候采用重定向？【大部分情况下都使用重定向】

   - 若想完成跨app跳转，必须使用重定向

   - 若在上一个资源中向request范围中存储了数据，希望在下一个资源中从request范围中将数据取出，必须使用转发
   - 重定向可以解决浏览器的刷新问题
     - 一个网页有提交数据功能，如果用转发的话，刷新页面会重复提交，但如果用重定向，最后网页的页面是最后一次请求的页面，刷新后不会重复提交
     - 刷新的是http://localhost:8080/webapp/success.html

6. 重定向原理是什么？

   response.sendRedirect("/jd/login");

   程序执行到以上代码，将请求路径/jd/login/反馈给浏览器

   浏览器自动又向web服务器发送了一次全新的请求：/jd/b

   浏览器地址栏上最终显示的地址是：/jd/login

7. 这句话还对吗？：在浏览器上点击一个超链接，到最终网页停下来是一次请求。

   这句话已经不正确了，因为有重定向机制存在，点击一个超链接，到网页最终停下来，这个过程，可能是多次请求。

# 十二、关于url-pattern的编写方式和路径的总结

## 1、路径的编写形式

- ```html
  <a href="/项目名/资源路径"></a>
  ```

- ```html
  <form action="/项目名/资源路径"></form>
  ```

- ```java
  转发: request.getRequestDispatcher("/资源路径").forward(request, response);
  ```

- ```java
  重定向: response.sendRedirect("/项目名/资源路径");
  ```

- ```xml
  欢迎页面
  <welcome-file-list>
  		<welcome-file>资源路径</welcome-file>
  </welcome-file-list>
  ```

- ```xml
  Servlet编写
  <url-pattern>/资源路径</url-pattern>
  ```

- ```java
  Cookie设置path
  cookie.setPath("/项目名/资源路径");
  ```

- ```java
  ServletContext
  ServletContext application = config.getServletContext();
  application.getRealPath("/WEB-INF/classes/db.properties");
  application.getRealPath("/资源路径");
  ```

## 2、url-pattern编写方式

1. url-pattern可以编写多个

2. **精确匹配**

   ```xml
   <url-pattern>/hello</url-pattern>
   <url-pattern>/system/hello</url-pattern>
   ```

3. 扩展匹配

   ```xml
   <url-pattern>/abc/*</url-pattern>
   ```

4. **后缀匹配**

   ```xml
   <url-pattern>*.action</url-pattern>
   <url-pattern>*.do</url-pattern>
   ```

5. 全部匹配

   ```xml
   <url-pattern>/*</url-pattern>
   ```


# 十三、HttpSession常用方法

| Modifier and Type | Method and Description                    |             |
| :---------------- | :---------------------------------------- | ----------- |
| `Object`          | `getAttribute(String name)`               |             |
| `void`            | `setAttribute(String name, Object value)` |             |
| `void`            | `removeAttribute(String name)`            |             |
| `void`            | `invalidate()`                            | 销毁session |

# 十四、ServletContext、HttpSession、HttpServletRequest接口的对比

- 以上三个接口都是范围对象
  - ServletContext application：是应用范围
  - HttpSession session：是会话范围
  - HttpServletRequest request：是请求范围

- 三个范围的排序：application>session>request
- application完成跨用户共享数据、
- session完成跨请求共享数据，但是这些请求必须在同一个会话当中
- request完成跨Servlet共享数据，但是这些Servlet必须在同一个请求当中【转发】
- 使用原则：
  - 由小到大尝试，优先使用小范围
  - 例如：登录成功之后，已经登陆的状态需要保存起来，可以将登陆成功的这个状态保存到session对象中。登录成功的状态不能保存到request范围中，因为一次请求对应一个新的request对象。登录成功状态也不能保存到application范围中，因为登录成功状态是属于会话级别的，不能所有用户共享。