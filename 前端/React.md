# [React](https://www.bilibili.com/video/BV1wy4y1D7JT?from=search&seid=8268857936371962336)

# ①、React入门

## 一、React简介

### 1、React是什么？

React：用于构建用户界面的JavaScript库。是一个将数据渲染为HTML视图的开源JavaScript库。

### 2、谁开发的？

由Facebook开发，且开源。

### 3、为什么要学？

- 原生JavaScript操作DOM繁琐、效率低(DOM-API操作UI)
- 使用JavaScript直接操作DOM，浏览器会进行大量的重绘重排
- 原生JavaScript没有组件化编码方案，代码复用率低

### 4、React的特点

- 采用**组件化**模式、**声明式编码**，提高开发效率及组件复用率。
- 在React Native中可以使用React语法进行**移动端开发**
- 使用**虚拟DOM**+优秀的**Diffing算法**，尽量减少与真实DOM的交互

![javaScript.PNG](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/javaScript.PNG?raw=true)

![image-20210207211924299](D:\JavaHub\学习相关\Java笔记\pictures\image-20210207211924299.png)

## 二、React的基本使用

### 1、相关js库

- react.js React核心库
- react-dom.js 提供操作DOM的react扩展库
- babel.min.js 解析JSX语法代码为JS代码的库

### 2、创建虚拟DOM的两种方式

**JSX创建：**

```jsx
<script type="text/babel"> /*此处一定要写babel*/
//1、创建虚拟DOM
const VDOM = <h1 id="title">Hello,React</h1> /*此处一定不要写引号，因为不是字符串*/
//2、渲染虚拟DOM到页面
ReactDOM.render(VDOM,document.getElementById('test'))
</script>
```

**JS创建(一般不用)：**

```jsx
<script type="text/javascript">
//1、创建虚拟DOM
const VDOM = React.createElement('h1', {id:'title'}, 'Hello,React')
//2、渲染虚拟DOM到页面
ReactDOM.render(VDOM,document.getElementById('test'))
</script>
```

一般不使用JS方式创建虚拟DOM，因为如果有多级HTML元素嵌套需要写多个createElement，比较繁琐，所以一般使用JSX创建虚拟DOM。

### 3、虚拟DOM和真实DOM

- React提供了一些API来创建一种 “特别” 的一般js对象

  ```jsx
  const VDOM = React.createElement('xx',{id:'xx'},'xx')
  ```

- 本质是Object类型的对象(一般对象)

- 虚拟DOM比较"轻"，真实DOM比较"重"，因为虚拟DOM是React内部在用，无需真实DOM上那么多的属性

- 虚拟DOM最终会被React转化为真实DOM，呈现在页面上

- 我们编码时基本只需要操作react的虚拟DOM相关数据, react会转换为真实DOM变化而更新界

## 三、React JSX

### 1、效果

![JSX效果](D:\JavaHub\学习相关\Java笔记\pictures\JSX效果.png)

### 2、JSX

- 全称：JavaScript XML

- react定义的一种类似于XML的JS扩展语法: JS + XML本质是**React.createElement(component, props, ...children)**方法的语法糖

- 作用: 用来简化创建虚拟DOM 

  1)   写法：**var** **ele** **=** \<h1>**Hello JSX!**\</h1>

  2)   注意1：它不是字符串, 也不是HTML/XML标签

  3)   注意2：它最终产生的就是一个JS对象

- 语法规则：

  ```jsx
  /*
  * JSX语法规则：
  * 1、定义虚拟DOM时，不要写引号
  * 2、标签中混入JS表达式时要用{}
  * 3、样式的类名指定不要用class,要用className
  * 4、内联样式，要用style={{key:value}}的形式去写
  * 5、只有一个根标签
  * 6、标签必须闭合
  * 7、标签首字母
  *   (1).若小写字母开头，则将该标签转为html中同名元素，若html中无该标签对应的同名元素，则报错。
  *   (2).若大写字母开头，React就去渲染对应的组件，若组件没有定义，则报错。
  * */
  ```

- {}里面的代码必须是JS表达式

  ```jsx
  /*
  * 一定注意区分：【JS语句(代码)】与【JS表达式】
  * 1、表达式：一个表达式会产生一个值，可以放在任何一个需要值的地方
  * 例如：
  *   (1)、a
  *   (2)、a+b
  *   (3)、demo(1)
  *   (4)、arr.map()
  *   (5)、function test(){}
  * 2、语句(代码)：
  * 例如：
  *   (1)、if(){}
  *   (2)、for(){}
  *   (3)、switch(){case:xxx}
  * */
  ```

## 四、模块与组件、模块化与组件化的理解

### 1、模块

- 理解：向外提供特定功能的js程序, 一般就是一个js文件
- 为什么要拆成模块：随着业务逻辑增加，代码越来越多且复杂。
- 作用：复用js, 简化js的编写, 提高js运行效率

### 2、组件

- 理解：用来实现局部功能效果的代码和资源的集合(html/css/js/image等等)
- 为什么要用组件： 一个界面的功能更复杂
- 作用：复用编码, 简化项目编码, 提高运行效率

### 3、模块化

- 当应用的js都以模块来编写的, 这个应用就是一个模块化的应用

### 4、组件化

- 当应用是以多组件的方式实现, 这个应用就是一个组件化的应用

# ②、React面向组件编程

## 一、函数式组件

**函数式组件：**

```jsx
/*
* 执行了ReactDOM.render(<Demo/>, document.getElementById('test'))之后，发生了什么？
* 1、React会解析组件标签，找到了Demo组件。
* 2、发现组件是使用函数定义的，随后调用该函数，将返回的虚拟DOM转为真实DOM，随后呈现在页面中。
* */
```

## 二、类式组件

**类相关复习：**

```jsx
/*
* 类的复习：
* 1、类中的构造器不是必须写的,要对实例进行一些初始化操作，如添加指定属性时，才写。
* 2、如果A类继承了B类，且A类中写了构造器，那么A类构造器中的super是必须要调用的。
* 3、类中定义的方法都是放在了类的原型对象上，供实例去使用。
* */
//创建一个Person类
class Person{
    //构造器方法
    constructor(name, age) {
        //构造器中的this是谁？————类的实例对象
        this.name = name
        this.age = age
    }
    //一般方法
    speak(){
        //speak方法放在了哪里？————类的原型对象上，供实例使用
        //通过Person实例调用speak时，speak中的this就是Person实例
        console.log(`我叫${this.name},我的年龄是${this.age}`);
    }
}

//创建一个Student类，继承于Person类
class Student extends Person{
    constructor(name,age,grade) {
        super(name,age);
        this.grade = grade
    }
    //重写从父类继承的方法
    speak() {
        console.log(`我叫${this.name},我的年龄是${this.age},我的年级是${this.grade}`);
    }
    study(){
        //study方法放在了哪里？————类的原型对象上，供实例使用
        //通过Student实例调用study时，speak中的this就是Student实例
        console.log(`我很努力的学习`);
    }
}

class Car{
    constructor(name, price) {
        this.name = name
        this.price = price
    }
    //类中可以直接写赋值语句，如下代码的含义是：给Car的实例对象添加一个属性，名为a，值为1
    a = 1
}
```

**类式组件：**

```jsx
<script type="text/babel"> /*此处一定要写babel*/
//1、创建类式组件
class MyComponent extends React.Component{
    render(){
        //render是放在哪里的？————MyComponent的原型对象上，供实例使用。
        //render中的this是谁？————MyComponent的实例对象<=>MyComponent组件实例对象
        console.log('render中的this:', this);
        return <h2>我是用类定义的组件(适用于【复杂组件】的定义)</h2>
    }
}
//2、渲染组件到页面
ReactDOM.render(<MyComponent/>, document.getElementById('test'))
/*
* 执行了ReactDOM.render(<MyComponent/>, document.getElementById('test'))之后，发生了什么？
* 1、React会解析组件标签，找到了MyComponent组件。
* 2、发现组件是使用类定义的，随后new出来该类的实例，并通过该实例调用到原型上的render方法。
* 3、将render返回的虚拟DOM转为真实DOM，随后呈现在页面中。
* */
</script>
```

## 三、组件实例的三大核心属性

### 1、state

- state是组件对象最重要的属性, 值是对象(可以包含多个key-value的组合)
- 组件被称为"状态机", 通过更新组件的state来更新对应的页面显示(重新渲染组件)

**强烈注意：**

- 组件中render方法中的this为组件实例对象

- 组件自定义的方法中this为undefined，如何解决？

  a)   强制绑定this: 通过函数对象的bind()

  b)   箭头函数

- 状态数据，不能直接修改或更新

**state的基本使用**

```jsx
<script type="text/babel"> /*此处一定要写babel*/
//1、创建组件
class Weather extends React.Component{

    //构造器调用几次？———— 1次
    constructor(props) {
        super(props);
        //初始化状态
        this.state = {isHot:true}
        //解决changeWeather中this指向问题
        //bind函数中参数为绑定的对象，这里把原型对象上的changeWeather方法绑定到了this这个实例上，赋值给this.changeWeather属性
        //之后的changeWeather函数调用的不再是原型对象的changeWeather函数而是this实例的，所以函数中this指向问题就可以解决了
        this.changeWeather = this.changeWeather.bind(this)
    }

    //render调用几次？———— 1+n次 1是初始化的那次 n是状态更新的次数
    render(){
        const {isHot} = this.state
        return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}</h1>
    }

    //changeWeather调用几次？———— 点几次调几次
    changeWeather(){
        //changeWeather放在哪里？————Weather的原型对象上
        //由于changeWeather是作为onClick的回调，所以不是通过实例调用的，是直接调用。
        //类中的方法默认开启了局部的严格模式，所以changeWeather中的this为undefined

        //获取原来的isHot值
        const isHot = this.state.isHot
        //严重注意：状态必须通过setState进行更新，且更新是一种合并，不是替换。
        this.setState({isHot: !isHot})

        //严重注意：状态(state)不可直接更改，下面这行就是直接更改！！！
        // this.state.isHot = !isHot 这是错误的写法
    }
}
//2、渲染组件到页面
ReactDOM.render(<Weather/>, document.getElementById('test'))
</script>
```

- state的简写方式

```jsx
<script type="text/babel"> /*此处一定要写babel*/
//1、创建组件
class Weather extends React.Component{

    state = {isHot:true}

    render(){
        const {isHot} = this.state
        return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}</h1>
    }

    //自定义方法————要用赋值语句的形式 + 箭头函数
    changeWeather = ()=>{
        const isHot = this.state.isHot
        this.setState({isHot: !isHot})
    }
}
//2、渲染组件到页面
ReactDOM.render(<Weather/>, document.getElementById('test'))
</script>
```

### 2、props

- 每个组件对象都会有props(properties的简写)属性
- 组件标签的所有属性都保存在props中

**props的基本使用**

```jsx
<script type="text/babel">
    //创建组件
    class Person extends React.Component{
        render(){
            // console.log(this)
            const {name,age,sex} = this.props
            return (
                <ul>
                    <li>姓名：{name}</li>
                    <li>性别：{sex}</li>
                    <li>年龄：{age+1}</li>
                </ul>
            )
        }
    }
    //对标签属性进行类型、必要性的限制
    Person.propTypes = {
        name: PropTypes.string.isRequired,//限制name必传且为字符串
        sex: PropTypes.string,//限制sex为字符串
        age:PropTypes.number,//限制age为数值
        speak:PropTypes.func//限制speak为函数
    }
    //指定默认标签属性值
    Person.defaultProps = {
        sex: '男',//性别默认值为男
        age: 18//年龄默认值为18
    }
    //渲染组件到页面
    ReactDOM.render(<Person name={"jerry"} age={19}  sex={"男"} speak = {speak}/>, document.getElementById('test1'))
    ReactDOM.render(<Person name={"tom"} age={18} sex={"女"}/>, document.getElementById('test2'))
    const p = {name:'老刘', age:18, sex:'女'}
    //下面的花括号跟原生js的花括号不一样，下面的花括号{}是React分隔符
    //里面...p可以展开p对象是因为上面的type="text/babel"和react核心库二者的原因，并且这种情况只适用于标签属性传递的时候，其他位置不使用
    ReactDOM.render(<Person {...p}/>, document.getElementById('test3'))

    function speak(){
        console.log('我说话了');
    }
</script>
```

**props的简写**

```jsx
<script type="text/babel">
    //创建组件
    class Person extends React.Component{
        //构造器是否接收props，是否传递给super，取决于：是否希望在构造器中通过this访问props
        constructor(props) {
            super(props);
        }
        //对标签属性进行类型、必要性的限制
        static propTypes = {
            name: PropTypes.string.isRequired,//限制name必传且为字符串
            sex: PropTypes.string,//限制sex为字符串
            age:PropTypes.number,//限制age为数值
            speak:PropTypes.func//限制speak为函数
        }

        //指定默认标签属性值
        static defaultProps = {
            sex: '男',//性别默认值为男
            age: 18//年龄默认值为18
        }

        render(){
            // console.log(this)
            const {name,age,sex} = this.props
            //props是只读的
            //this.props.name = 'jack'//此行代码会报错，因为props是只读的
            return (
                <ul>
                    <li>姓名：{name}</li>
                    <li>性别：{sex}</li>
                    <li>年龄：{age+1}</li>
                </ul>
            )
        }
    }

    //渲染组件到页面
    ReactDOM.render(<Person name={"jerry"} age={19}  sex={"男"} speak = {speak}/>, document.getElementById('test1'))
    ReactDOM.render(<Person name={"tom"} age={18} sex={"女"}/>, document.getElementById('test2'))
    const p = {name:'老刘', age:18, sex:'女'}
    //下面的花括号跟原生js的花括号不一样，下面的花括号{}是React分隔符
    //里面...p可以展开p对象是因为上面的type="text/babel"和react核心库二者的原因，并且这种情况只适用于标签属性传递的时候，其他位置不使用
    ReactDOM.render(<Person {...p}/>, document.getElementById('test3'))

    function speak(){
        console.log('我说话了');
    }
</script>
```

**函数式组件使用props**

```jsx
function Person (props){
    const {name, age, sex} = props
    return (
        <ul>
            <li>姓名：{name}</li>
            <li>性别：{sex}</li>
            <li>年龄：{age}</li>
        </ul>
    )
}
```

### 3、refs与事件处理

- 组件内的标签可以定义ref属性来标识自己
- 不推荐使用字符串形式的ref，推荐使用回调形式的ref或者createRef创建ref容器

**字符串形式的ref**

```jsx
<script type="text/babel"> /*此处一定要写babel*/
//创建组件
class Demo extends React.Component{
    render(){
        return(
            <div>
                <input ref="input1" type="text" placeholder="点击按钮提示数据"/>&nbsp;
                <button onClick={this.showData}>点我提示左侧数据</button>&nbsp;
                <input ref="input2" onBlur={this.showData2} type="text" placeholder="失去焦点提示数据"/>
            </div>
        )
    }

    //展示左侧输入框的数据
    showData = ()=>{
        const {input1} = this.refs
        alert(input1.value);
    }

    //展示右侧输入框的数据
    showData2 = ()=>{
        const {input2} = this.refs
        alert(input2.value);
    }
}
//渲染组件到页面
ReactDOM.render(<Demo/>, document.getElementById('test'))
</script>
```

**回调函数形式的ref**

```jsx
<script type="text/babel"> /*此处一定要写babel*/
//创建组件
class Demo extends React.Component{
    render(){
        return(
            <div>
                <input ref={(currentNode)=>{this.input1 = currentNode}} type="text" placeholder="点击按钮提示数据"/>&nbsp;
                <button onClick={this.showData}>点我提示左侧数据</button>&nbsp;
                <input ref={(currentNode)=>{this.input2 = currentNode}} onBlur={this.showData2} type="text" placeholder="失去焦点提示数据"/>&nbsp;
            </div>
        )
    }

    //展示左侧输入框的数据
    showData = ()=>{
        const {input1} = this
        alert(input1.value);
    }

    展示右侧输入框的数据
    showData2 = ()=>{
        const {input2} = this
        alert(input2.value);
    }
}
//渲染组件到页面
ReactDOM.render(<Demo/>, document.getElementById('test'))
</script>
```

**createRef**

```jsx
<script type="text/babel"> /*此处一定要写babel*/
//创建组件
class Demo extends React.Component{
    /*
    * React.createRef调用后可以返回一个容器，该容器可以存储被ref所标识的节点
    * */
    myRef = React.createRef();
    myRef2 = React.createRef();

    render(){
        return(
            <div>
                <input ref={this.myRef} type="text" placeholder="点击按钮提示数据"/>&nbsp;
                <button onClick={this.showData}>点我提示左侧数据</button>&nbsp;
                <input onBlur={this.showData2} ref={this.myRef2} type="text" placeholder="失去焦点提示数据"/>&nbsp;
            </div>
        )
    }

    //展示左侧输入框的数据
    showData = ()=>{
        alert(this.myRef.current.value);
    }

    展示右侧输入框的数据
    showData2 = ()=>{
        alert(this.myRef2.current.value);
    }
}
//渲染组件到页面
ReactDOM.render(<Demo/>, document.getElementById('test'))
</script>
```

