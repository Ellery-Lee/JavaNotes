# JSP

- JavaServer Pages，基于Java语言实现的服务器端页面

- JSP是JavaEE规范之一

- jsp文件通常放在什么位置？
  - JSP可以放在WEB-INF目录外，目前我们就是这样做的
  - 在实际开发中，有很多项目是将JSP放到WEB-INF目录中，保护JSP
  - WEB-INF目录中的数据是相对安全的
- jsp文件后缀是什么？
  - 默认是.jsp
  - 但是jsp文件的后缀也可以修改，通过修改CATALINA_HOME/conf/web.xml文件

## 一、JS和JSP区别

- JS：JavaScript，运行在浏览器中，和服务器没有关系
- JSP：JavaServer Pages，运行在服务器中。JSP底层就是Java程序，运行在JVM中

## 二、JSP的执行原理

- 浏览器上访问的路径虽然是以.jsp结尾，访问的是某个jsp文件，其实底层执行的是jsp对应的Java程序
- Tomcat服务器将.jsp文件翻译生成.java源文件，并且将java源文件编译生成.class字节码文件
- 访问jsp，其实底层还是执行了.class文件中的程序
- Tomcat服务器内置了一个JSP翻译引擎，专门负责翻译JSP文件，编译Java源文件
- index.jsp会被翻译生成index_jsp.java，编译生成index_jsp.class
- index_jsp这个类继承了HttpJspBase，而HttpJspBase继承了HttpServlet
- jsp就是Servlet，只不过职责不同，JSP的强项是做页面展示
- 在JSP文件中编写的所有的HTML、CSS、JavaScript，对于JSP来说，**只是普通的字符串，被翻译到：out.write("翻译到这里")**
- JSP文件修改之后，不需要重新部署，也不需要重新启动Tomcat服务器

## 三、JSP文件第一次访问的时候为什么非常慢

- 启动JSP翻译引擎
- 需要一个翻译的过程
- 需要一个编译的过程
- 需要Servlet对象的创建过程
- init方法调用
- service方法调用...

## 四、为什么第2+次访问快？

- 不需要重新翻译
- 不需要重新翻译
- 不需要创建Servlet对象
- 直接调用Servlet对象的service方法

jsp也是单实例多线程环境下运行的一个Servlet对象

## 五、JSP文件什么时候重新翻译？

- jsp文件被修改之后会被重新地翻译
- 怎么确定jsp文件修改了呢？
  - Tomcat服务器会记录jsp文件的最后修改时间

## 六、语法

```jsp
<%--
    这是JSP的专业注释，使用这种注释方式，不会被翻译到Java源文件中
--%>
```

在JSP文件中所编写的所有的html、css、JavaScript都会被自动翻译到Servlet的service方法中的out.write("翻译到这里");

### 1、JSP小脚本scriptlet

```jsp
<%
	java语句;
	java语句;
	java语句;
%>
```

小脚本中的java语句被翻译到Servlet的service方法中，所以小脚本中必须编写“java语句”，java语句以分号结尾

所谓JSP规范就是SUN制定好的一些翻译规则，按照翻译规则进行翻译，生成对应的java源程序，不同的web服务器翻译的结果是完全相同的，因为这些服务器在翻译的时候，都遵守了JSP翻译规范

小脚本的数量随意，可以多个

小脚本中编写java程序出现在service方法中，service方法的代码是有执行顺序的，所以小脚本中的程序也是有顺序的。

**面试题：在jsp中可以编写实例变量、方法和静态语句块吗？**

> 不能，因为jspjava语句是被翻译在service方法中，方法中不能编写上述结构

**JSP主要是完成页面的展示，最好在JSP文件中少的编写Java源代码：以后会通过EL表达式 + JSTL标签库来代替JSP中的Java源代码。当然使用某些动作也能代替Java源代码。**

### 2、声明语法

JSP声明语法格式：

```jsp
<%!
    实例变量;
	静态变量;
	方法;
	静态语句块;
	构造函数;
	......
%>
```

**注：**

- 声明块中的Java程序会被JSP翻译引擎翻译到service方法之外
- 声明块中不能直接编写Java语句，除非是变量的声明

### 3、JSP九大内置对象

#### ①、什么是内置对象？

- 可以直接在JSP文件中拿来使用的引用

#### ②、九大内置对象有哪些？

- pageContext（页面范围）               javax.servlet.jsp.pageContext                            
- request （请求范围）                       javax.servlet.http.HttpServletRequest            
- session （会话范围）                       javax.servlet.http.HttpSession
- application （应用范围）                 javax.servlet.ServletContext
- out                                                       javax.servlet.jsp.JspWriter                                    标准输出流
- response                                             javax.servlet.http.HttpServletResponse            响应对象
- config                                                   javax.servlet.ServletConfig                                  Servlet配置信息对象
- exception                                            java.lang.Throwable                                             异常引用(isErrorPage="true")
- page                                                     javax.servlet.http.HttpServlet(page = this)       很少用

**以上内置对象只能在service方法中“直接使用”，在其它方法中无法“直接”使用，可以间接使用**

#### ③、主要研究JSP中的四个作用域对象/范围对象：

pageContext < request < session < application

pageContext: 在同一个JSP页面中共享数据，不能跨JSP页面

request：在同一个请求中共享数据 【使用较多】

session：在同一个会话中共享数据 【使用较多】

application：所有用户共享的数据可以放到应用范围中



### 4、表达式

```jsp
<%= %> 等同于 out.print();
<%="Hello World!"%> 等同于 out.print("Hello World!");
```

### 5、JSP指令

指令的作用：指导JSP的翻译引擎如何翻译JSP代码

JSP中共三个指令：

- page：页面指令
- include：包含指令
- taglib 标签库指令【以后讲】

指令的使用语法格式：

```jsp
<%@指令名 属性名=属性值 属性名=属性值......%>
```

#### ①、JSP page指令

page指令中常用的属性

- contentType：设置JSP响应内容类型
- pageEncoding：设置JSP响应时的字符编码方式
- import：组织导入
- session：设置当前JSP页面中是否可以直接使用session内置对象
- errorPage：错误页面
- isErrorPage：是否是错误页面
- isELIgnored：是否忽略EL表达式【后期讲】



##### 关于page指令中的session属性

- session=“true”

  表示在当前JSP中可以直接使用内置对象session

  程序执行的时候获取当前的session会话对象，若获取不到则新建session对象

- session=“false”

  表示在当前JSP中不能直接使用内置对象session

  但是有一些业务可能要求在当前JSP页面中获取当前的session对象，没有获取到则不新建session对象，此时需要编写以下程序

  ```jsp
  <%@page contentType="text/html; charset=UTF-8" session="false"%>
  <%
      HttpSession session = request.getSession(false);
  %>
  ```

- 若session这个属性没有指定，默认值就是session=“true”

##### 关于page指令中的errorPage属性:

- 当前JSP页面出错之后，要跳转的页面路径，需要使用该属性指定

  ```jsp
  <%@page contentType="text/html; charset=UTF-8" errorPage="路径"%>
  <%
      exception.printStackTrace();
  %>
  ```

- errorPage页面使用内置对象exception打印异常堆栈追踪信息，exception引用指向了抛出的异常

  ```jsp
  <%@page contentType="text/html; charset=UTF-8" isErrorPage="true"%>
  <%
      exception.printStackTrace();
  %>
  ```

  

##### 关于page指令中的isErrorPage属性:

- isErrorPage="false"

  表示内置对象exception无法使用

- isErrorPage=“true”

  表示内置对象exception可以使用

- 默认值：“false”

#### ②、JSP include指令

```jsp
<%@page contentType="text/html; charset=UTF-8"%>
<html>
    <head>
        <title>include指令</title>
    </head>
    <body>
        <%@include file="/index2.jsp"%>
    </body>
</html>
```

##### 关于include指令：

- a.jsp可以将b.jsp包含进来，当然被包含的资源不一定是jsp，也可能是其他的网络资源
- include作用：
  - 在网页中有一些主体框架，例如：网页头、网页脚、这些都是固定不变的，我们可以将网页头、网页脚等固定不变的单独编写到某个JSP文件中，在需要页面使用include指令包含进来
- 优点：
  - 代码量少，便于维护（修改一个文件就可以作用于所有的页面）
- 在一个jsp中可以使用多个include指令
- include实现原理：
  - 编译期包含
  - a.jsp包含b.jsp，底层共生成一个java源文件，一个class字节码文件，**翻译期包含/编译期包含/静态联编**
- 静态联编的时候，多个jsp中可以共享同一个局部变量，因为最终翻译之后service方法只有一个

### 6、JSP动作

##### 关于JSP中的动作：

- 语法格式：

```jsp
<jsp:动作名 属性名=属性值......></jsp:动作名>
```

#### ①、forward动作

```jsp
<jsp:forward page=""></jsp:forward>
```

**转发是一次请求**

- 以上JSP的动作可以编写为对应的Java程序进行实现**转发**

  ```jsp
  <% request.getRequestDisPatcher("/index2.jsp").forward(request,response); %>
  ```

- 编写Java程序完成**重定向**

  ```jsp
  <%
  	response.sendRedirect(request.getContextPath() + "/index2.jsp");
  %>
  ```

#### ②、include动作

```jsp
<%@page contentType="text/html; charset=UTF-8"%>
<html>
    <head>
        <title>include指令</title>
    </head>
    <body>
        <jsp:include page="/b.jsp"></jsp:include>
    </body>
</html>
```

##### 关于JSP中的include动作

- a.jsp包含b.jsp，底层会分别生成两个Java源文件，两个class字节码文件
- 编译阶段并没有包含，编译阶段是两个独立的class字节码文件，生成两个Servlet，两个独立的service方法
- 使用include动作属于运行阶段包含，实际上是在**运行阶段a中的service方法调用了b中的service方法，达到了包含效果**
- a.jsp包含b.jsp，若两个jsp文件中有重名的变量，只能使用动态包含，其余都可以使用静态包含。
- include动作完成的动态包含，被称为**动态联编**。

#### ③、bean动作

- useBean
- setProperty
- getProperty

```jsp
<jsp:useBean id=""></jsp:useBean>
<jsp:setProperty property="" name=""/>
<jsp:getProperty property="" name=""/>
```

以上的bean动作使用较少，作为了解。