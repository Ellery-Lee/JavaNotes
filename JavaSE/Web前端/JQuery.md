# JQuery

JavaScript的封装版，简化JavaScript对DOM对象定位以及对DOM对象属性操作开发步骤

##一、学习重点

- 熟练背诵JQuery**选择器**和**过滤器**语法
- 熟练掌握JQuery对象提供的功能函数

##二、JQuery对象和DOM对象区别

**DOM对象：**

- 在浏览器加载网页时，由浏览器负责创建
- 一个HTML标签对应一个DOM对象
- 可以通过管理DOM对象，来对关联HTML标签中的属性进行操作

**JQuery对象：**

- 是JQuery函数负责创建的：$();
- JQuery对象就是一个数组
- JQuery对象中存放的是本次定位的DOM对象
- 可以通过JQuery对象中功能函数，来快速对定位的DOM对象进行操作管理

```javascript
var $obj = $(".ah");//JQuery对象名称一般是以$为开头
$obj.prop("checked", flag)l
```

**JQuery对象与DOM对象转换：**

JQuery对象转换成DOM对象：

```javascript
for(var i = 0; i < $obj.length; i++){
	var domObj = $obj[i];
}
```

DOM对象转换成JQuery对象：

```javascript
var $obj = $(domObj);
```

JQuery对象与DOM对象之间属性和函数彼此不能调用

## 三、JQuery选择器

是对DOM对象进行定位的条件，比如根据ID定位，根据标签类型名定位

只有三种选择器：

### 1、基本选择器

【定位条件】可以根据ID编号，根据标签类型名，根据标签采用样式选择器

【使用】：

```
$("#id编号"):代替document.getElementById("id")
```

根据ID编号定位对应的DOM对象，让DOM对象保存到一个数组中并返回。返回这个数组就是JQuery对象

```JavaScript
$(".样式选择器名称"): 代替document.getElementsByClassName("样式选择器名")
```

将使用了指定的延时选择器的dom对象保存到同一个数组中，并返回。返回这个数组就是JQuery对象

```javascript
$("标签类型名"): 代替document.ElementByTagName("input")
```

将所有指定的标签类型关联的DOM对象保存到同一个数组中并返回，返回的这个数组就是JQuery对象

```JavaScript
$("*")
```

定位浏览器中所有的DOM对象保存到同一个数组中并返回，返回的这个数组就是JQuery对象

```JavaScript
$("条件1，条件2")
```

只要DOM对象满足其中的一种条件，就会被定位到数组中

###2、层级选择器

【定位条件】可以根据标签之间父子关系定位，可以根据标签之间兄弟关系定位

```
<tr>
	<td>
		<input>
	</td>
</tr>
```

父子关系：td是tr的直接子标签，input是td的直接子标签，input是tr的间接子标签

兄弟关系：两个拥有相同的父标签，并且彼此之间没有包含关系

【使用】

```
$("定位父标签条件>定位子标签")
```

定位当前父标签下，所有满足条件的直接子标签

```
$("定位父标签条件 定位子标签条件")
```

定位当前父标签下，所有满足条件的直接子标签和间接子标签

```
$("定位当前标签条件~定位兄弟标签条件")
```

定位当前标签后面，所有满足条件的兄弟标签

```
$("定位当前标签条件+定位兄弟标签条件")
```

定位当前标签后面与之紧邻的并且满足条件的所有兄弟标签

```
$("定位当前标签条件").siblings("定位兄弟标签条件")
```

定位当前标签前面与后面所有满足条件的兄弟标签

###3、INPUT标签选择器

【使用】

```
$(":button")
```

定位页面中所有的button关联的DOM

```
$(":checkbox")
```

定位页面中所有的checkbox关联的DOM

#四、JQuery过滤器

过滤器语法介绍：

1. 对已经定位到JQuery对象中DOM对象，进行二次过滤筛选的。
2. 过滤器不能独立使用，必须声明在选择器后面
3. 六种过滤器（三种常见过滤器）
4. 将多个过滤器放在一个选择器执行

### 1、基本过滤器

【过滤器条件】：根据已经定位的DOM对象在JQuery对象中存储位置进行二次过滤筛选

【使用】：

```
$("选择器：first")：留下满足条件的第一个DOM对象
$(":button:first")：定位页面中第一个button
```

```
$("选择器：last")：留下满足条件的最后一个DOM对象
$(":button:last")：定位页面中最后一个button
```

```
$("选择器：eg(下标值)")：留下满足条件中的指定位置的DOM对象
$("div:eq(2)")：定位页面中第三个div
```

```
$("选择器:lt(下标值)")：留下满足条件中的指定位置之前的所有DOM
$(":checkbox:lt(2)")：定为页面中前两个checkbox
```

```
$("选择器:gt(下标值)")：留下满足条件中的指定位置之后的所有DOM
$(":button:gt(3)")：定为页面中第四个之后的所有button
```

### 2、基本属性过滤器

【过滤条件】：根据标签在声明时是否为指定属性进行手动赋值，根据标签的属性内容与【指定内容】关系进行过滤器

```
<div>1</div1>
<div name = 'two'>1</div1>
<div name = 'three'>1</div1>
```

问题1：哪一个DIV中没有name属性？

- 都有，事先固定好的。

问题2：哪一个DIV中name属性是“”

- 第一个，默认空

```
$("选择器[属性名]")：留下满足定位条件的并且在声明时为指定属性进行手动赋值的DOM对象
$("div[name]")<div name = "two">2</div>
			  <div name = "three">3</div>
```

```
$("选择器[属性名='值']")：留下满足定位条件并且指定属性内容【等于】指定内容DOM对象
$(div[name='two'])<div name = "two">2</div>
```

```
$("选择器[属性名^='值']")：留下满足定位条件的并且指定属性内容以【指定内容为开头】所有的DOM对象
```

```
$("选择器[属性名$='值']")：留下满足定位条件的并且指定属性内容以【指定内容为结尾】所有的DOM对象
```

```
$("选择器[属性名*='值']")：留下满足定位条件的并且指定属性内容【包含】指定内容所有的DOM对象
```

```
$("选择器[属性名1][属性名2*='值'][属性名3*='值']")
```

### 3、工作状态属性过滤器

HTML属性分类：

- 基本属性：绝大多数标签都拥有的属性，【id,name,title,rowspan】

- 样式属性：背景，字体，边框
- value属性：只存在【表单域标签中，(input，select，textarea)】
- 工作状态属性：只存在表单域标签[checked，disabled,selected]
- 监听事件属性：onclick, onchange.....

【使用】

```
$("选择器：enabled")：留下满足条件的并且处于【可用状态】的DOM
$(":button:enabled")：定位所有处于可用的button
```

```
$("选择器：disabled")：留下满足条件的并且处于【不可用状态】的DOM
$(":button:disabled")：定位所有处于不可用的button
```

```
$("选择器：checked")：留下满足条件的并且处于【选中状态】的DOM
$(":checkbox:checked:first"):页面中第一个被选中的checkbox
```

```
$("选择器：selected")：留下满足条件的并且处于【选中状态】的DOM
$("select>option:selected"):下拉列表中选中的option
```

#五、JQuery功能函数

###1、show() & hide()

show():负责让JQuery对象	包含的所有DOM对象关联的标签在浏览器上显示  style="display:block"

hide(): 负责让JQuery对象	包含的所有DOM对象关联的标签在浏览器上隐藏   style="display:none"

### 2、remove() & empty()

empty(): 将当前标签子标签进行清除处理

remove(): 将当前标签及子标签进行清除处理

### 3、append() & appendTo()

共同点：都是操作标签中的innerHTml，为当前标签添加子标签

append()：父标签.append(子标签)：父亲给自己找了一个儿子

appendTo()：子标签.appendTo(父标签)：儿子给自己找了一个父亲

### 4、属性操作函数

#### 1.val函数

```
$obj.val():读取JQuery对象中第一个DOM对象的【value属性值】
```

```
$obj.val(值): 为JQuery对象中所有DOM对象的【value属性】进行统一赋值
```

#### 2.prop函数

```
$obj.prop("checked",true):为JQuery对象中所有DOM对象的checked属性值赋值为true
```

```
$obj.prop("checked"):读取JQuery对象中第一个DOM对象的【checked属性值】
```

####3.attr函数：基本属性 id name title rowspan

```
$obj.attr("name", "ck"):为JQuery对象所有DOM对象的【name属性】统一赋值为[ck]
```

```
$obj.attr("title"):读取JQuery对象中第一个DOM对象的【title属性值】
```

#### 4、text函数：操作属性innerTEXT，双目标签的文本显示内容

```
$(obj.text"123):为JQuery对象中所有DOM对象的innerTEXT属性赋值123
```

```
$obj.text():读取JQUery对象中所有DOM对象的innerTEXT内容，拼接成字符串返回
```

#六、JS中事件绑定方式

### 1、嵌入式绑定

```
<input type="button" onclick="fun1()">
```

缺点：一次只能为一个标签绑定监听事件

##2、基于DOM对象的绑定方式

```
var array = document.getElementsByName("ck");
for循环
```

#七、JQuery中事件绑定方式

##1、$obj.jquery监听事件函数（处理函数）

```
$(":button").click(fun1);
```

为页面中所有的button绑定onclick以及对应的处理函数fun1

## 2、$obj.bind("jquery监听事件函数名", 处理函数)：以这种方式绑定监听事件，可以解除的

```
$obj.unbind("jquery监听事件函数名")：将指定监听事件从DOM对象身上移除
```

```
$obj.unbind():将所有监听事件从DOM对象身上移除
```

