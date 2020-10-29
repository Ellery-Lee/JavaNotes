# Maven

# 一、Maven简介

### ①、完成一个Java项目，需要知道做哪些工作

1. 分析项目要做什么，知道项目有哪些组成部分

2. 设计项目，通过哪些步骤，使用哪些技术。需要多少人，多长时间。

3. 组建团队，招人，购置设备，服务器，软件，笔记本。 

4. 开发人员写代码。开发人员需要测试自己写代码。重复多次的工作。

5. 测试人员，测试项目功能是否符合要求。

   测试开发人员提交代码-如果测试有问题--需要开发人员修改--再提交代码给测试--测试人员再测试代码--如果还有问题-再交给开发-开发人员再提交再测试--直到测试代码通过

### ②、传统开发项目的问题，没有使用Maven管理项目

1. 很多模块，模块之间有关系，手动管理关系，比较繁琐。

2. 需要很多第三方功能，需要很多jar文件，需要手工从网络中获取各个jar包

3. 需要管理jar的版本，你需要的是mysql5.1.5.jar，那你不能给一个mysql1.4.0.jar

4. 管理jar文件之间的依赖，你的项目要使用a.jar需要使用b.jar里面的类。必须首先获取到b.jar才可以，然后才能使用a.jar

   a.jar需要b.jar这个关系叫做依赖，或者你的项目中要使用mysql的驱动，也可以叫做项目依赖mysql驱动。

   a.class使用b.class，a依赖b类

### ③、需要改进项目的开发和管理，需要Maven

1. Maven可以管理jar文件
2. Maven可以管理jar文件，自动下载jar和它的文档源代码
3. 管理jar之间的依赖，a.jar需要b.jar，Maven会自动下载b.jar
4. 管理你需要的jar版本
5. 帮你编译程序，把Java编译为class
6. 帮你测试你的代码是否正确
7. 帮你打包文件，形成jar文件，或者war文件
8. 帮你部署项目

### ④、构建：项目的构建

构建是面向过程的，就是一些步骤，完成项目代码的编译，测试，运行，打包，部署等等。

Maven支持的构建包括有：

1. 清理：把之前项目编译的东西删除掉，为新的代码做准备。
2. 编译：把程序源代码编译为执行代码，Java--class文件。这个过程是批量的，Maven可以同时把成千上百的文件编译为class。**跟javac不一样，javac一次编译一个文件**
3. 测试：Maven可以执行测试程序代码，验证你的功能是否正确。这个过程也是批量的，Maven同时执行多个测试代码，同时测试很多功能。
4. 报告：生成测试结果的文件，测试通过没有。
5. 打包：把项目中所有的class文件，配置文件等所有资源放到一个压缩文件中。这个压缩文件就是项目的结果文件，**通常Java程序，压缩文件是jar扩展名的。对于web应用，压缩文件扩展名是.war**
6. 安装：把5中生成的文件jar，war安装到本机仓库
7. 部署：把程序安装好可以执行。

### ⑤、Maven的核心概念

#### 1、POM

一个文件，名称是pom.xml，pom翻译过来叫做项目对象模型。Maven把一个项目当做一个模型使用。可以控制Maven构建项目的过程，管理jar依赖。

#### 2、约定的目录结构

Maven项目的目录和文件的位置都是规定的。

#### 3、坐标

是一个唯一的字符串，用来表示资源的。

#### 4、依赖管理

管理项目中可以使用的jar文件

#### 5、仓库管理（了解）

资源存放的位置

#### 6、生命周期（了解）

Maven工具构建项目的过程，就是生命周期。

#### 7、插件和目标

执行Maven构建的时候用的工具是插件

#### 8、继承

#### 9、聚合

讲Maven的使用，先难后易。难是说使用Maven的命令，完成Maven使用，在idea中直接使用Maven，代替命令。

### ⑥、Maven工具的安装和配置

1. 需要从Maven的官网下载Maven的安装包 apache-maven-3.3.9-bin.zip

2. 解压安装包，解压到一个目录，非中文目录

   子目录bin：执行程序，主要是mvn.cmd

   conf：maven工具本身的配置文件 settings.xml

   

3. 配置环境变量

   在系统的环境变量中，指定一个M2_HOME的名称，指定它的值是Maven工具安装目录，bin之前的目录

   ```
   M2_HOME=D:\....
   ```

   再把M2_HOME加入到path之中，在所有的路径之前加入

   ```
   %M2_HOME%\bin;
   ```

4. 验证，新的命令行中，执行mvn -v

   注意：需要配置JAVA_HOME，指定jdk路径

# 二、Maven的核心概念

## ①、约定的目录结构

约定是大家都遵守的一个规则。

每一个Maven项目在磁盘中都是一个文件夹。（项目-Hello）

Hello/

​	---/src

​	------/main	#放置主程序的Java代码和配置文件

​	---------/Java	#程序包和包中的Java文件

​	---------/resources	#Java程序中要使用的配置文件



​	------/test	#放置测试程序代码和文件的（可以没有）

​	---------/Java	#测试程序包和包中的Java文件

​	---------/resources	#测试Java程序中要使用的配置文件



​	---/pom/xml	Maven的核心文件（Maven项目必须有）

**疑问**  mvn compile 编译src/main目录下的所有Java文件

1. 为什么要下载？

   Maven工具执行的操作需要很多插件（Java类--jar文件）完成

2. 下载什么东西了

   jar文件--叫做插件--插件是完成某些功能的

3. 下载的东西存放到哪里

   默认仓库（本机仓库）：

   ```
   C:\Users\HP（登录操作系统用户名）\.m2\repository
   ```

执行mvn compile，结果是在项目的根目录下生成target目录（结果目录），Maven编译的Java程序，最后的class文件都放在target目录中

**设置本机存放资源的目录位置：**

1. 修改Maven的配置文件，Maven安装目录/conf/settings.xml

   先备份 settings.xml

2. 修改local_repository指定你的目录（不要使用中文目录）

## ②、仓库

1. 仓库是什么？

   仓库是存放Maven使用的jar和我们项目使用的jar包

   - Maven使用的插件（各种jar）
   - 项目使用的jar（第三方工具）

2. 仓库的分类？

   - 本地仓库，就是你的个人计算机上的文件夹，存放各种jar
   - 远程仓库，在互联网上的，使用网络才能使用的仓库
     - 中央仓库，最权威的，所有的开发人员都共享使用的一个集中的仓库，https://repo.maven.apache.org/：中央仓库的地址。
     - 中央仓库的镜像：就是中央仓库的备份，在各大洲，重要的城市都是镜像。
     - 私服，在公司内部，在局域网中使用，不是对外使用的。
   
3. 仓库的使用，Maven仓库的使用不需要人为参与。

   开发人员需要使用mysql的驱动--->Maven首先查本地仓库--->私服--->镜像--->中央仓库
   
4. pom：项目对象模型，是一个pom.xml文件

   ```
   modelVersion：Maven的模型版本，对于Maven2和Maven3来说，只能是4.0.0
   groupId：组织id，一般是公司域名的倒写，比如com.baidu
   artifactId：项目名称，也是模块名称，对应groupId中项目中的子项目
   version：项目版本号。不稳定版本一般在版本后带-SNAPSHOT
   //groupId、artifactId、version三个构成坐标，唯一标识一个Maven项目
   packaging：项目打包类型
   ```

   **①、坐标：**唯一值，在互联网中唯一标识一个项目的

   ```java
   <groupId>com.Mybatis</groupId>
   <artifactId>ex01_Mybatis</artifactId>
   <version>1.0-SNAPSHOT</version>
   ```

   https://mvnrepository.com/ 搜索使用中央仓库，使用groupId或者artifactId作为搜索条件

   **②、packaging：**打包后压缩文件的扩展名，默认是jar，web应用是war。packaging可以不写，默认是jar

   **③、依赖：**dependency和dependencies项目中要使用的各种资源说明，相当于Java中的import。比如要使用mysql驱动

   ```java
   <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
   <dependencies>
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>5.1.9</version>
       </dependency>
   </dependencies>
   ```

   **④、properties：**设置属性

   **⑤、build：**Maven在进行项目的构建时，配置信息，例如指定编译Java代码使用的JDK版本等。

## ③、Maven的生命周期、命令、插件

Maven的生命周期：就是Maven构建项目的过程，清理，编译，测试，报告，打包，安装，部署。

Maven的命令：Maven独立使用，通过命令，完成Maven的生命周期的执行。Maven可以使用命令，完成项目的清理，测试等等。

Maven的插件：Maven命令执行时，真正完成功能的是插件，插件就是一些jar文件，一些类。

### 1、单元测试（测试方法）

用的是junit，junit是一个专门测试的框架（工具）。junit测试的内容：测试的是类中的方法，每一个方法都是独立测试的。方法是测试的基本单位（单元）。

Maven借助单元测试，批量测试类中的大量方法是否符合预期。

### 2、使用步骤

1. 加入依赖，在pom.xml

2. 在Maven项目中的src/test/java目录下，创建测试程序。

   推荐的创建类和方法提示：

   - 测试类的名称是 Test+要测试的类名
   - 测试方法的名称是 test+方法名称

   例如要测试HelloMaven

   - 创建测试类TestHelloMaven

     ```java
     @Test
     public void testAdd(){
         测试HelloMaven的add方法是否正确
     }
     ```

     其中testAdd叫做测试方法，它的定义规则

     1. 方法是public的，必须的
     2. 方法没有返回值，必须的
     3. 方法名称是自定义的，推荐是 test+方法名称
     4. 在方法的上面加 @Test

3. mvn compile

   编译main/java/目录下的java为class文件，同时把class拷贝到target/classes目录下面，同时把main/resources目录下的所有文件都拷贝到target/classes目录下

4. mvn test-compile

   编译测试程序到target/test-classes目录下面

5. mvn test

   一起执行上面三个命令，输出测试结果。

6. mvn package

   把项目压缩成jar包，只包含src/main/目录下内容

7. mvn install

   安装主程序，把本工程打包，并且按照本工程的坐标保存到本地仓库中。

# 三、在IDEA中使用Maven

## 1、配置Maven

idea中内置了Maven，一般不使用内置Maven，因为内置修改Maven设置不方便。

使用自己安装的Maven，需要覆盖idea中的默认设置。让idea知道Maven安装位置等信息。

**配置的入口①：**file---settings---build,execution，deployment---build tools---maven（配置当前工程的设置）

Maven Home directory：Maven的安装目录

User Settings File：就是Maven安装目录conf/settings.xml配置文件

Local Repository：本机仓库的目录位置

---Build Tools---Maven---Runner

VM Options：archetypeCatalog=internal（Maven项目创建时，会联网下载模板文件，比较大，使用此参数配置不用下载，创建Maven项目速度快）

JRE：项目的JDK

**配置的入口②：**file---new project settings---settings for new project（配置以后新建工程的设置）

操作同①一样

## 2、使用模板创建项目

1. maven-archetype-quickstart：普通的Java项目
2. maven-archetype-webapp：web工程