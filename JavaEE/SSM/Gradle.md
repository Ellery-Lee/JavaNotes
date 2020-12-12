# Gradle

# 一、Gradle简介

### 1、什么是Gradle

Gradle是一个开源的项目自动化构建工具，建立在Apache Ant和Apache Maven概念的基础上，并引入了基于Groovy的特定领域语言(DSL)，而不再使用XML形式管理构建脚本。

### 2、什么是Groovy

Groovy是用于Java虚拟机的一种敏捷的动态语言，它是一种成熟的面向对象编程语言，既可以用于面向对象编程，又可以用作纯粹的脚本语言。使用该种语言不必编写过多的代码，同时又具有闭包和动态语言中的其他特性。

**Groovy和Java比较：**

- Groovy完全兼容Java的语法
- 分号是可选的
- 类、方法默认是public的
- 编译器给属性自动添加getter/setter方法
- 属性可以直接使用点号获取
- 最后一个表达式的值会被作为返回值
- ==等同于equals(),不会有NullPointerExceptions

Groovy语句测试：

```groovy
public class ProjectVersion{
    private int major;
    private int minor;

    ProjectVersion(int major, int minor) {
        this.major = major;
        this.minor = minor;
    }

    int getMajor() {
        return major
    }

    void setMajor(int major) {
        this.major = major
    }

    int getMinor() {
        return minor
    }

    void setMinor(int minor) {
        this.minor = minor
    }
}
```

**高效的Groovy特性：**

- assert语句
- 可选类型定义
- 可选的括号
- 字符串
- 集合API
- 闭包

Groovy高效性测试：

```groovy
//groovy 高效特性
//1、可选的类型定义，def是弱类型的，groovy会自动根据情况来给变量赋予对应的类型
def version = 1
//2、assert
//assert version == 2
//3、括号是可选的
println version
//4、字符串，三种方式
//单引号只是字符串
def s1 = 'liruicong'
//双引号可以插入变量
def s2 = "gradle version is ${version}"
//三引号可以换行
def s3 = '''my
name is
liruicong'''

println  s1
println s2
println s3

//5、集合API
//list
def buildTools = ['ant', 'maven']
buildTools << 'gradle'
assert buildTools.getClass() == ArrayList
assert buildTools.size() == 3

//6、Map
def buildYears = ['ant':2000, 'maven':2004]
buildYears.gradle = 2009
println buildYears.ant
println buildYears['gradle']
println buildYears.getClass()

//7、闭包
//闭包其实就是一段代码块。在Gradle中，我们主要是把闭包当参数来使用
//定义一个闭包，带参数
def c1 = {
    v ->
        println v
}
//定义一个闭包，不带参数
def c2 = {
    println 'hello'
}

//定义一个方法，方法里面需要闭包类型的参数。注意Closure不要导包！！！
def method1(Closure closure){
    closure('param')
}

def method2(Closure closure){
    closure()
}
method1(c1);
method2(c2);
```

### 3、Gradle项目目录

- src/main/java 放置正式代码目录
- src/main/resources 放置正式配置文件目录
- src/test/java 放置单元测试代码目录
- src/test/resources 放置测试配置文件目录
- src/main/webapp 放置页面元素，比如：js,css,img,jsp,html等等