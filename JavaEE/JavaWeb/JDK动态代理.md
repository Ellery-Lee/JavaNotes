# JDK动态代理（理解）

动态代理：基于反射机制

掌握的程度：

1. 什么是动态代理？

2. 知道动态代理能做什么？

   可以在不改变原来目标方法功能的前提下，可以在代理中增强自己的功能代码。

   程序开发中的意义：比如你所在的项目中有一个功能是其他人（公司的其他部门，其他小组的人）写好的，你可以使用。但你发现这个功能还缺点，不能完全满足我项目的需要，我需要在方法执行后增加自己的代码。用代理实现方法调用时，增加自己的代码，而不用去修改原来的文件。

后面会讲Mybatis、Spring

## 一、什么是动态代理？

### 1、代理：代购、中介、换IP、商家等等

留学中介（代理）：帮助这家美国学校招生，中介是学校的代理，中介是代替学校完成招生功能。

代理特点：

1. 中介和代理要做的事情是一致的，招生。
2. 中介是学校的代理，学校是目标。
3. 家长---中介（学校介绍，办入学手续）---美国学校。
4. 中介是代理，不能白干活，需要收取费用。
5. 代理不让你访问到目标

为什么要中介？

1. 中介是专业的，方便
2. 家长现在不能自己去找学校，家长没有能力访问学校，或者美国学校不接受个人来访

买东西都是商家卖，商家是某个商品的代理，你个人买东西，肯定不会让你接触到厂家的。



### 2、使用场景

在开发中也会有这样的情况，你有a类，本来是调用c类的方法，完成某个功能。但是c不让a调用。

a---不能调用c的方法

在a和c之间创建一个b代理，c让b访问。

a---访问b---访问c

实际的例子：登录，注册有验证码，验证码是手机短信，

中国移动，联通能发短信，他们有子公司或者关联公司，他们面向社会提供短信的发送功能。

张三项目发送短信---子公司，或者关联公司**（代理）**---中国移动，联通

![Snipaste_2020-10-17_12-29-48.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Snipaste_2020-10-17_12-29-48.png?raw=true)

### 3、代理模式的作用

1. 功能增强：在原有的功能上，增加了额外的功能。新增加的功能，叫做功能增强。
2. 控制访问：代理类不让你访问目标，例如商家不让用户访问厂家，

### 4、实现代理的方式

#### ①静态代理：

- 代理类是自己手工实现的，自己创建一个Java类，表示代理类。

- 同时你所要代理的目标类是确定的。

**优点：**

- 实现简单
- 容易理解

**缺点：**

- 当目标类增加了，代理类可能也需要成倍增加。导致代理类数量过多
- 当你的接口中功能增加了，或者修改了，会影响众多的实现类，厂家类，代理都需要修改，影响比较多。



模拟一个用户购买u盘的行为。用户是客户端类，商家代理某个品牌的u盘。厂家是目标类。

三者关系：用户（客户端）---商家（代理）---厂家（目标）

商家和厂家都是卖u盘的，他们完成的功能是一致的，都是卖u盘。

**实现步骤：**

- 创建一个接口，定义卖u盘的方法，表示你的厂家和商家要做的事情。
- 创建厂家类，实现1步骤的接口。
- 创建商家，就是代理，也需要实现1步骤中的接口。
- 创建客户端类，调用商家的方法买一个u盘。

代理类完成的功能：

1. 目标类中方法的调用
2. 功能增强

#### ②动态代理：

在静态代理中目标很多的时候，可以使用动态代理，避免静态代理的缺点。

动态代理中目标类即使很多，代理类数量可以很少，当你修改了接口中的方法时，不会影响代理类。

**动态代理：**在程序的执行过程中，使用JDK的反射机制，创建代理类对象，并动态地指定要代理的目标类。换句话说，动态代理是一种创建Java对象的能力，让你不用创建TaoBao类，就能创建代理类对象。

动态：在程序执行时，调用JDK提供的方法才能创建代理类的对象。

动态代理的实现有两种：

1. JDK动态代理（理解）：使用**反射机制**，Java反射包中的类和接口实现动态代理的功能。反射包Java.lang.reflect，里面有三个类：InvocationHandler, Method, Proxy

2. CGLIB动态代理（了解）：cglib是第三方的工具库，创建代理对象。cglib的原理是继承，cglib通过继承目标类，创建它的子类，在子类中重写父类中同名的方法，实现功能的修改。

   因为cglib是继承，重写方法，所以要求目标类不能是final的，方法也不能是final的。cglib的要求目标类比较宽松，只要能继承就可以了。cglib在很多的框架中使用，比如Mybatis，Spring框架中都有使用。

**JDK动态代理：**

1. 反射：Method类，表示方法。类中的方法，通过Method可以执行某个方法

2. JDK动态代理的实现

   反射包java.lang.reflect，里面有三个类：InvocationHandler, Method, Proxy

   - InvocationHandler接口：就一个方法invoke()

     invoke()：表示代理对象要执行的功能代码。你的代理类要完成的功能就写在invoke()方法中。

     方法原型：

     >```java
     >public Object invoke(Object proxy, Method method, Object[] args)
     >```

     参数：Object proxy：JDK创建的代理对象，无需赋值

     ​			Method method：目标类中的方法，JDK提供method对象

     ​			Object[] args：目标类中方法的参数，JDK提供的

     怎么用：

     1. 创建类实现接口InvocationHandler
     2. 重写invoke()方法，把原来静态代理中代理类要完成的功能，写在这里

   - Method类：表示方法的，确切的说就是目标类中的方法。

     作用：通过Method可以执行某个目标类的方法，Method.invoke();

     ```java
     method.invoke(目标对象，方法参数)
     Object ret = method.invoke(target, "李四");
     ```

   - Proxy类：核心的对象，创建代理对象。之前创建对象都是new类的构造方法，现在我们使用Proxy类的方法，代替new的使用。

     方法：静态方法 newProxyInstance()

     作用是：创建代理对象，等同于静态代理中的TaoBao taobao = new TaoBao();

     方法原型：

     >```java
     >public static Object newProxyInstance(ClassLoader loader,
     >                                      Class<?>[] interfaces,
     >                                      InvocationHandler h)
     >```

     参数：ClassLoader loader类加载器，负责像内存中加载对象的。使用反射获取对象的ClassLoader

     ```java
     a.getClass().getClassLoader()
     ```

     ​			Class<?>[] interfaces：接口，目标对象实现的接口，也是反射获取的。

     ​			InvocationHandler h：我们自己写的，代理类要完成的功能

     返回值：就是代理对象

**实现动态代理的步骤：**

1. 创建接口，定义目标类要完成的功能
2. 创建目标类实现接口
3. 创建InvocationHandler接口的实现类，在invoke方法中完成代理类的功能
   - 调用目标方法
   - 增强功能
4. 使用Proxy类的静态方法，创建代理对象。并把返回值转为接口类型

![Snipaste_2020-10-17_17-08-49.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/Snipaste_2020-10-17_17-08-49.png?raw=true)