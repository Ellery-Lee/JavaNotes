

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

![image-20210207211924299.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/image-20210207211924299.png?raw=true)

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

![JSX效果.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/JSX%E6%95%88%E6%9E%9C.png?raw=true)

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

**事件处理**

- 1.通过onXxx属性指定事件处理函数(注意大小写)
  - React使用的是自定义(合成)事件, 而不是使用的原生DOM事件——为了更好的兼容性
  - React中的事件是通过事件委托方式处理的(委托给组件最外层的元素)——为了更高效

- 2.通过event.target得到发生事件的DOM元素对象——不要过度使用ref

## 四、收集表单数据



### 1、非受控组件

- 输入类的DOM按照现有输入现用现取

```jsx
<script type="text/babel"> /*此处一定要写babel*/
//创建组件
class Login extends React.Component{
    handleSubmit = (event) =>{
        event.preventDefault()//阻止表单提交
        const {username, password} = this;
        alert(`你输入的用户名是:${username.value}你输入的密码是:${password.value}`)
    }
    render(){
        return(
            <form onSubmit={this.handleSubmit}>
                用户名：<input ref= {c =>this.username = c} type="text" name="username" />
                密码：<input ref= {c =>this.password = c} type="password" name="password"/>
                <button>登录</button>
            </form>
        )
    }
}
//渲染组件
ReactDOM.render(<Login/>, document.getElementById('test'))
</script>
```

### 2、受控组件

- 输入类的DOM随着输入把内容维护到状态中，等需要用的时候从状态中取出

```jsx
<script type="text/babel"> /*此处一定要写babel*/
//创建组件
class Login extends React.Component{
    //初始化状态
    state = {
        username: '', //用户名
        password: '' //密码
    }

    //保存用户名到状态中
    saveUsername = (event) =>{
        // console.log(event.target.value);
        this.setState({username:event.target.value})
    }

    //保存密码到状态中
    savePassword = (event) =>{
        // console.log(event.target.value);
        this.setState({password:event.target.value})
    }

    //表单提交的回调
    handleSubmit = (event) =>{
        event.preventDefault()//阻止表单提交
        const {username, password} = this.state;
        alert(`你输入的用户名是:${username}你输入的密码是:${password}`)
    }
    render(){
        return(
            <form onSubmit={this.handleSubmit}>
                用户名：<input onChange= {this.saveUsername} type="text" name="username" />
                密码：<input onChange= {this.savePassword} type="password" name="password"/>
                <button>登录</button>
            </form>
        )
    }
}
//渲染组件
ReactDOM.render(<Login/>, document.getElementById('test'))
</script>
```

### 3、高阶函数和函数柯里化

**高阶函数**：如果一个函数符合下面两个规范中的任何一个，那该函数就是高阶函数

1、若A函数，接收的参数是一个函数，那么A函数就可以称之为高阶函数
2、若A函数，调用的返回值依然是一个函数，那么A就可以称之为高阶函数

常见的高阶函数有：Promise、setTimeout、arr.map()等等

**函数的柯里化**：通过函数调用继续返回函数的方式，实现多次接收参数最后统一处理的函数编码形式

**高阶函数_函数柯里化**

```jsx
<script type="text/babel"> /*此处一定要写babel*/
//#region
    /*
    * 高阶函数：如果一个函数符合下面两个规范中的任何一个，那该函数就是高阶函数
    *   1、若A函数，接收的参数是一个函数，那么A函数就可以称之为高阶函数
    *   2、若A函数，调用的返回值依然是一个函数，那么A就可以称之为高阶函数
    *   常见的高阶函数有：Promise、setTimeout、arr.map()等等
    * 函数的柯里化：通过函数调用继续返回函数的方式，实现多次接收参数最后统一处理的函数编码形式
    * */
//#endregion
//创建组件
class Login extends React.Component{
    //初始化状态
    state = {
        username: '', //用户名
        password: '' //密码
    }

    //保存用户名到状态中
    saveFormData = (dataType) =>{
        return (event)=>{
            this.setState({[dataType]:event.target.value})
        }
    }

    //表单提交的回调
    handleSubmit = (event) =>{
        event.preventDefault()//阻止表单提交
        const {username, password} = this.state;
        alert(`你输入的用户名是:${username}你输入的密码是:${password}`)
    }
    render(){
        return(
            <form onSubmit={this.handleSubmit}>
                {/*这里this.saveFormData('username')结果还是一个函数，作为了onChange的回调函数*/}
                用户名：<input onChange= {this.saveFormData('username')} type="text" name="username" />
                密码：<input onChange= {this.saveFormData('password')} type="password" name="password"/>
                <button>登录</button>
            </form>
        )
    }
}
//渲染组件
ReactDOM.render(<Login/>, document.getElementById('test'))
</script>
```

**不用函数柯里化的实现**

```jsx
<script type="text/babel"> /*此处一定要写babel*/
//创建组件
class Login extends React.Component{
    //初始化状态
    state = {
        username: '', //用户名
        password: '' //密码
    }

    //保存用户名到状态中
    saveFormData = (dataType, event) =>{
        this.setState({[dataType]:event.target.value})
    }

    //表单提交的回调
    handleSubmit = (event) =>{
        event.preventDefault()//阻止表单提交
        const {username, password} = this.state;
        alert(`你输入的用户名是:${username}你输入的密码是:${password}`)
    }
    render(){
        return(
            <form onSubmit={this.handleSubmit}>
                用户名：<input onChange= {event => this.saveFormData('username', event)} type="text" name="username" />
                密码：<input onChange= {event => this.saveFormData('password', event)} type="password" name="password"/>
                <button>登录</button>
            </form>
        )
    }
}
//渲染组件
ReactDOM.render(<Login/>, document.getElementById('test'))
</script>
```

## 五、组件的生命周期

- 组件从创建到死亡它会经历一些特定的阶段
- React组件中包含一系列勾子函数(生命周期回调函数), 会在特定的时刻调用
- 我们在定义组件时，会在特定的生命周期回调函数中，做特定的工作

### 1、组件的生命周期(旧)

![react生命周期旧.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/react%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E6%97%A7.png?raw=true)

- **初始化阶段:** 由ReactDOM.render()触发---初次渲染
  - constructor()
  - componentWillMount()
  - render()
  - componentDidMount() =====>常用，一般在这个钩子中做一些初始化的事，例如：开启定时器、发送网络请求、订阅消息

- **更新阶段:** 由组件内部this.setSate()或父组件重新render触发
  - shouldComponentUpdate()
  - componentWillUpdate()
  - render()
  - componentDidUpdate()

- **卸载组件:** 由ReactDOM.unmountComponentAtNode()触发
  - componentWillUnmount() =====>常用，一般在这个钩子中做一些收尾的事，例如：关闭定时器、取消订阅消息

### 2、组件的生命周期(新)

![react生命周期新.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/react%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E6%96%B0.png?raw=true)

- **初始化阶段:** 由ReactDOM.render()触发---初次渲染
  - constructor()
  - **getDerivedStateFromProps()**
  - render()
  - componentDidMount()

- **更新阶段:** 由组件内部this.setSate()或父组件重新render触发
  - **getDerivedStateFromProps()**
  - houldComponentUpdate()
  - render()
  - **getSnapshotBeforeUpdate**
  - componentDidUpdate()

- **卸载组件:** 由ReactDOM.unmountComponentAtNode()触发
  - componentWillUnmount()



**重要的钩子：**

- render：初始化渲染或更新渲染调用

- componentDidMount：开启监听, 发送ajax请求

- componentWillUnmount：做一些收尾工作, 如: 清理定时器

**即将废弃的钩子：**

- componentWillMount

- componentWillReceiveProps

- componentWillUpdate

现在使用会出现警告，下一个大版本需要加上UNSAFE_前缀才能使用，以后可能会被彻底废弃，不建议使用。

### 3、新旧生命周期对比

新生命周期废弃了旧生命周期三个钩子：

- componentWillMount
- componentWillReceiveProps
- componentWillUpdate

新增了两个钩子：

- getDerivedStateFromProps
- getSnapshotBeforeUpdate

## 六、虚拟DOM和DOM Diffing算法

![DOM_Diffing.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/DOM_Diffing.png?raw=true)

```js
经典面试题:
      1). react/vue中的key有什么作用？（key的内部原理是什么？）
      2). 为什么遍历列表时，key最好不要用index?
		1. 虚拟DOM中key的作用：
				1). 简单的说: key是虚拟DOM对象的标识, 在更新显示时key起着极其重要的作用。
				2). 详细的说: 当状态中的数据发生变化时，react会根据【新数据】生成【新的虚拟DOM】, 随后React进行【新虚拟DOM】与【旧虚拟DOM】的diff比较，比较规则如下：
					a. 旧虚拟DOM中找到了与新虚拟DOM相同的key：
						(1).若虚拟DOM中内容没变, 直接使用之前的真实DOM
						(2).若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM
					b. 旧虚拟DOM中未找到与新虚拟DOM相同的key,根据数据创建新的真实DOM，随后渲染到到页面					
		2. 用index作为key可能会引发的问题：
				1. 若对数据进行：逆序添加、逆序删除等破坏顺序操作:会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低。
				2. 如果结构中还包含输入类的DOM：会产生错误DOM更新 ==> 界面有问题。							
				3. 注意！如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，使用index作为key是没有问题的。	
		3. 开发中如何选择key?:
				1.最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值。
				2.如果确定只是简单的展示数据，用index也是可以的。
```

# ③、React应用(基于react脚手架)

## 一、React脚手架

### 1、概念

React脚手架：用来帮助程序员快速创建一个基于React库的模板项目

- 包含了所有需要的配置（语法检查、jsx编译、devServer…）
- 下载好了所有相关的依赖
- 可以直接运行一个简单效果

react提供了一个用于创建react项目的脚手架库: create-react-app

项目的整体技术架构为: react + webpack + es6 + eslint

使用脚手架开发的项目的特点: 模块化, 组件化, 工程化

### 2、项目结构

public ---- 静态资源文件夹

​            favicon.icon ------ 网站页签图标

​            **index.html --------** **主页面**

​            logo192.png ------- logo图

​            logo512.png ------- logo图

​            manifest.json ----- 应用加壳的配置文件

​      	  robots.txt -------- 爬虫协议文件



src ---- 源码文件夹

​            App.css -------- App组件的样式

​            **App.js --------- App****组件**

​            App.test.js ---- 用于给App做测试

​            index.css ------ 样式

​            **index.js -------** **入口文件**

​            logo.svg ------- logo图

​            reportWebVitals.js

​                    --- 页面性能分析文件(需要web-vitals库的支持)

​            setupTests.js

​                    ---- 组件单元测试的文件(需要jest-dom库的支持)