#JavaScript

JavaScript是运行在浏览器上的脚本语言，简称js

JavaScript的“目标程序”以普通文本形式保存，这种语言都叫做“脚本语言”

**JSP和JS区别**

JSP：JavaServer Pages（隶属于Java语言的，运行在JVM中）

JS：JavaScript（运行在浏览器上）

#一、在HTML中怎么嵌入JS代码

## 第一种方式

JS是一种事件驱动型编程语言，依靠事件去驱动，然后执行对应的程序。在JS中有很多事件，其中一个事件叫做鼠标单击，单词click。并且任何事件都会对应一个事件句柄叫做：onclick。

**事件和事件句柄的区别：**事件句柄是在事件单词前加一个on，而事件句柄是以HTML标签的属性存在的。

- onclick="js代码"，执行原理：页面打开的时候，js代码并不会执行，只是把这段JS代码注册到按钮的click事件上了。等这个按钮发生click事件之后，注册在onclick后面的js代码会被浏览器自动调用。 

- 怎么使用js代码弹出消息框：在JS中有一个内置对象叫做window，全部小写，可以直接拿来使用。window代表的是浏览器对象。window对象有一个对象alert，用法是：**window.alert("消息");**

- JS中一条语句结束之后可以使用分号";"，也可以不用
- JS中的字符串可以使用双引号，也可以使用单引号
- **window.**可以省略不写

```html
<input type="button" onclick="消息"/>
```

## 第二种方式

脚本块

```html
<script type="text/javascript">
    <!--js代码-->
        window.alert("hello world");
</script>
```

- 暴露在脚本块中的程序，在页面打开的时候执行。并且遵守自上而下的顺序依次逐行执行。（这个代码的执行不需要事件）

- js的脚本块在一个页面中可以出现多次
- js脚本块出现位置没有要求
- js代码的注释  //这是单行注释   /*这是多行注释 */
- alert函数会阻塞整个HTML加载的作用

## 第三种方式

引入外部独立的js文件

```html
<script type="text/javascript" src="xxx.js"></script>
```

- 引入外部独立的js文件的时候，js文件的代码会遵循自上而下的顺序逐行执行。
- 同一个js文件可以被引入多次

<script type="text/javascript" src= "xxx.js">
    <!--js代码
        这里写的代码不会执行，但如果这里再写script代码块还是会执行-->
</script>

#二、JS的变量

JavaScript是一种弱类型语言，没有编译阶段，一个变量可以随意赋值，赋什么类型的值都行

声明变量：

```javascript
var  变量名
```

给变量赋值：

```javascript
变量名 = 值;
```

- 在JS当中，当一个变量没有手动赋值的时候，系统默认赋值undefined，**undefined在JS中是一个具体存在的值**
- 一个变量没有声明/定义，直接访问会报错

```javascript
var a, b, c = 200;
alert("a = " + a); //undefined
alert("b = " + b); //undefined
alert("c = " + c); //200
```

## 1、局部变量和全局变量

**全局变量：**在函数体之外声明的全局变量，全局变量的生命周期：**浏览器打开的时候声明，浏览器关闭的时候销毁。**尽量少用，因为全局变量会一直在浏览器的内存当中，耗费内存空间。能使用局部变量尽量使用局部变量。

- 如果一个变量在声明的时候没有加var关键字，不管这个变量在哪里声明的，都是全局变量

**局部变量：**在函数体中声明的变量，包括一个函数的形参都属于局部变量，局部变量的生命周期是：函数开始执行时局部变量的内存空间开辟，函数执行结束之后，局部变量的内存空间释放。局部变量生命周期较短。

#三、JS函数

JS中的函数等同于java语言中的方法，函数也是一段可以被重复利用的代码片段。函数一般都是可以完成某个特定功能的。

JS中的函数不需要指定返回值类型，返回什么类型都行

##第一种方式

```javascript
function 函数名(形参列表){
	函数体;
}

function sum(a, b){
    return a+b;
}

<input type="button" onclick="sum(1, 2)"/>

<script type="text/javascript">
    var retValue = sum();
	alert(retValue);//NaN

	var retValue = sum(1,2,3);
	alert(retValue);//3,第三个不赋值
</script>
```

##第二种方式

```javascript
函数名 = function(形参列表){
	函数体;
}
```

- 函数必须调用才能执行

- NaN是一个具体存在的值，该值表示不是数字。Not a Number，但事实Number类型

- JS中如果两个函数同名，后声明的会覆盖前面的函数

#四、JS中的数据类型

JS中数据类型有：原始类型和引用类型

**原始类型：**Undefined、Number、String、Boolean、Null

**引用类型：**Object以及Object子类

ES6之后，又基于以上六种类型添加了新类型：symbol

- JS中有一个运算符叫 typeof，这个运算符可以在程序的运行阶段动态地获取变量的数据类型，用法

  ```javascript
  typeof 变量名
  
  typeof(Null) == "object"//无法解释，Null是一种数据类型，typeof结果是object类型
  ```

  运算结果是留个类型字符串，全部是小写

  ```
  "undefined"
  "number"
  "string"
  "boolean"
  "object"
  "function"
  ```

- 在JS中比较字符串用 "==" 完成

##1、Undefined类型

Undefined类型只有一个值，这个值就是Undefined。当一个变量没有手动赋值，系统默认值是undefined，或者也可以给一个变量手动赋值undefined。

## 2、Number类型

Number类型包括：-1 0 1 2 2 3 3 14 100... NaN Infinity

- isNaN()函数：如果是true表示不是一个数字，如果是false表示是一个数字
- parseInt()函数，可以将字符串自动转换成数字，并且取整数位
- parseFloat()函数，可以将字符串自动转换成数字
- Math.ceil()函数（Math是数学类，数学类当中有一个函数叫ceil()，作用是向上取整）

##3、Boolean类型

- JS中布尔类型永远都只有两个值，true和false

- 在Boolean类型中有一个Boolean()函数，语法格式：

  ```javascript
  Boolean(数据)
  ```

  Boolean函数的作用是将非布尔类型转换成布尔类型，规律：**“有”就是true，“没有”就转换成false**

##4、Null类型

Null类型只有一个值，null

##5、String类型

- 在JS当中字符串可以使用单引号，也可以使用双引号

  var s1 = 'abc'

  var s2 = "test"

- 在JS中怎么创建字符串对象？两种方式

  第一种：var s = "abc"，小String，属于String类型

  第二种：var s2 = new String("abc")，使用JS内置的支持类String，String的父类是Object。大String，属于Object类型

  无论小String还是大String，它们的方法和属性都是公用的

- String类型的常用属性和函数

  常用属性：length：获取字符串长度

  常用函数：indexOf、lastIndexOf、replace（只替换第一个匹配符）、substr、substring、toLowerCase、toUpperCase

  **substr和substring区别：**substr(startIndex, length)   substring(startIndex, endIndex)(左闭右开)

##6、Object类型

Object类型是所有类型的超类。自定义任何类型，默认继承Object

Object类有哪些属性？

- prototype属性(常用，主要给类动态地扩展属性和函数)
- constructor属性

Object类包括哪些函数？

- toString()
- valueOf()
- toLocaleString()

在JS当中定义的类默认继承Object，会继承Object类中所有的属性以及函数

**在JS中怎么定义类，怎么定义对象？**

定义类语法：

**第一种方式：**

```JavaScript
function 类名(形参){

}
```

**第二种方式：**

```javascript
类名 = function(形参){

}
```

创建对象的语法：

```
new 构造方法名(实参)；//构造方法名和类名一致
```

当做普通函数调用：

```javascript
类名();
```

**JS中类的定义同时又是一个构造函数的定义**

```javascript
function User(a,b,c){
    this.snu = a;
    this.sno = b;
    this.sage = c;
    this.getSage = function(){
        return this.sage;
    }
}
```

**可以使用prototype这个属性来给类动态地扩展属性以及函数：**

```javascript
User.prototype.getSno = function{
	return this.sno;
}

var n1 = new User();
var sno = n1.getSno();
```

##7、null undefined NaN区别

**数据类型不一致：**

- null：object
- NaN：number
- undefined：undefined

**==判断**

- null==NaN //false
- null==undefined  //true
- undefined==NaN //false

## 8、"==" 和 "\=\=="

==：等同运算符，只判断值是否相等

===：全等运算符，既判断值是否相等，有判断数据类型是否相等

#五、JS的常用事件

- 1、blur失去焦点 
- 2、focus获得焦点
- 3、change下拉列表选中项改变，或文本框内容改变
- 4、click鼠标单击
- 5、dbclick鼠标双击
- 6、keydown键盘按下
- 7、keyup键盘弹起
- 8、mousedown鼠标按下
- 9、mouseup鼠标弹起
- 10、mouseover鼠标经过
- 11、mousemove鼠标移动
- 12、mouseout鼠标离开
- 13、mouseup鼠标弹起
- 14、reset重置表单
- 15、submit表单提交
- 16、load页面加载完毕（整个HTML页面中所有的元素全部加载完毕后发生）
- 17、select文本被选定

**任何一个事件都会对应一个事件句柄，事件句柄是在事件前添加on。**onxxx这个事件句柄出现在一个标签的属性位置上（事件句柄以属性的形式存在）

**注册事件：**

- 第一种方式：直接在标签中使用事件句柄，调用的函数被称为回调函数（callback函数）

- 第二种方式：使用纯JS代码完成事件的注册

  > 第一步：var btnObj = document.getElementById("mybtn");
  >
  > 第二步：btnObj.onclick = doSome;  给按钮对象的onclick属性赋值，将回调函数doSome注册到click上，**千万别加小括号**
  >
  > ​				btnObj.onclick = function(){...}   这个函数没有名字，叫做匿名函数这个匿名函数也是一个回调函数

#六、JS代码执行顺序

```javascript
//页面加载的过程中，将a函数注册给了load事件
//页面加载完毕后，load事件发生了，此时执行回调函数
//函数a执行的过程中，把b函数注册给了id="btn"的click事件
//当id="btn"的节点发生click事件后，b函数被调用并执行
window.onload = function(){ //这个回调函数叫做a
    document.getElementById("btn").onclick = function(){ //这个回调函数叫做b
        alert("hello.js");
    }
}
```

#七、捕捉回车键

```js
window.onload = function(){ //这个回调函数叫做a
    var userElement = document.getElementById("username");
    //回车键键值13
    //ESC键键值27
    userElement.onkeydown = function(event){
        //获取键值
        //对于键盘事件来说，都有keyCode属性用来获取键值
        if(event.keyCode === 13){
            alert("正在验证...")
        }
    }
    userElement.onclick = sayHello;
}
```

# 八、JS运算符

**void运算符语法：**void(表达式)

运算原理：执行表达式，但不返回任何结果。

```javascript
//javascript:void(0),其中JavaScript：作用是告诉浏览器后面是一段JS代码，不能省略
<a href="javascript:void(0)" onclick= "window.alert()">
    既保留了超链接样式，同时用户点击该超链接的时候执行一段JS代码，但页面还不跳转
</a>
```

#九、JS控制语句

- if
- while
- switch
- do...while
- for
- break
- continue
- for...in语句(了解)
- with语句(了解)

创建JS数组：中括号    Java中是{}

```javascript
var arr = [false, 1, 2 "abc", 3.14];
```

```javascript
for(var i in arr){
	alert(arr[i]);//i是下标
}
```

for..in语句可以遍历对象的属性



```javascript
with(u){
	alert(username);//自动在username前面加u
}
```

#DOM编程

JavaScript包括三大块：

- ECMAScript：JS核心语法(ES规范/ECMA-262标准)
- DOM：Document Object Model（文档对象模型：对网页当中的节点进行增删改查，HTML文档被当做一颗DOM树来看待）
- BOM：Browser Object Model（浏览器对象模型：关闭浏览器窗口、打开一个新的浏览器窗口、后退、前进、浏览器地址栏上的地址等，都是BOM编程）

DOM和BOM区别和联系：

BOM的顶级对象是：window

DOM顶级对象是：document

**实际上BOM是包括DOM的**

# 一、innerHTML和innerText属性

**是属性**

相同点：

- 都是设置元素内部的内容

不同点：

- innerHTML会把后面的“字符串”当做一段HTML代码解释并执行
- innerText即使后面是一段HTML代码，也只是将其当做普通的字符串来看待

```javascript
window.onload = function(){
    var btn = document.getElementById("btn");
    btn.onclick = function(){
        var divElt = document.getElementById("div1");
        divElt.innerHTML = "fddsfds";
        divElt.innerText = "fddsfds";
    }
}
```

#二、正则表达式(Regular Expression)

正则表达式主要用在字符串匹配方面。

**要求：**

- 常见的正则表达式符号要认识
- 简单的正则表达式要会写
- 他人编写的正则表达式要能看懂
- 在JavaScript当中，怎么创建正则表达式对象（new对象）
- 在JavaScript当中，正则表达式对象有哪些方法（调方法）
- 要能够快速地从网络上知道自己需要的正则表达式，并且测试其有效性

## 1、常见的正则表达式

```
. 匹配除换行符以外的任意字符
\w 匹配字母或数字或下划线或汉字
\s 匹配任意的空白符
\d 匹配数字
\b 匹配单词的开始或结束
^ 匹配字符串的开始
$ 匹配字符串的结束

* 重复零次或更多次
+ 重复一次或更多次
? 重复零次或一次
{n} 重复n次
{n,} 重复n次或更多次
{n,m} 重复n到m次

\W 匹配任意不是字母或数字或下划线或汉字的字符
\S 匹配任意不是的空白符
\D 匹配非数字的字符
\B 匹配不是单词的开始或结束的位置字符
[^x] 匹配除了x以外的任意字符
[^aeiou] 匹配除了aeiou这几个字母以外的任意字符
```

##2、简单的正则表达式要会写

正则表达式当中的小括号（）优先级较高

[1-9] 表示1到9的任意1个数字（次数是1次）

[A-Za-z0-9] 表示A-Za-z0-9中的任意一个字符

[A-Za-z0-9-]表示A-Z、a-z、0-9、-，以上所有字符串中的任意一个字符

QQ号的正则表达式：^[1-9]\[0-9]{4,}$

##3、怎么创建正则表达式对象，怎么调用正则表达式对象的方法

第一种：var regExp = /正则表达式/flags;

第二种：var regExp = newRegExp("正则表达式","flags");

关于flags：

g：全局匹配

i：忽略大小写

m：多行搜索（ES规范之后才支持m）



正则表达式对象的test()方法

true/false = 正则表达式对象.test(用户填写的字符串);

true：匹配成功

false：匹配失败

##4、表单提交（看视频）

#BOM编程

**有哪些方法可以通过浏览器往服务器发请求**

- 表单form提交
- 超链接
- document.location
- window.location
- window.open("url")

#JSON

JavaScript Object Notation，简称JSON。（数据交换格式）

JSON主要的作用是：一种标准的数据交换格式。（目前非常流行，90%以上的系统，系统A与系统B交换数据的话，都是采用JSON）

JSON是一种标准的轻量级的数据交换格式，特点是：体积小、易解析

在实际的开发中有两种数据交换格式，一种是**JSON**，另一个是**XML**

- XML体积较大、解析麻烦，但是有其优点是：语法严谨（通常银行相关的系统之间进行数据交换的话会使用XML）

```javascript
//创建JSON对象(无类型对象)
var studentObj = {
	"sno" : "110",
    "sname" : "张三"，
    "sex" : "男"
};

//访问JSON对象属性
alert(studentObj.sno + "," + studentObj.sname + "," + studentObj.sex);
```

JSON的语法格式：

```javascript
var jsonObj = {
	"属性名" : 属性值，
    "属性名" : 属性值，
    "属性名" : 属性值，
    "属性名" : 属性值，
    ...
};
```

**eval函数：**作用是把字符串当做一段代码解释并执行

```
window.eval("var i = 100;")
```

java连接数据库，查询数据之后，将数据在java程序中拼接成JSON格式的”字符串“，将json格式的字符串响应到浏览器，也就是说java	响应到浏览器上的仅仅是一个”JSON格式的字符串“，还不是一个json对象。

可以用eval函数，将json格式的字符串转换成json对象



**面试题：**

在JS当中，[]和{}有什么区别？

[]是数组，{}是JSON