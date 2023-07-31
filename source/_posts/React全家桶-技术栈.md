---
title: React全家桶(技术栈)
date: 2023-07-16 17:57:21
tags:
---

<a name="nweF1"></a>

# **第1章：React入门**

<a name="X3jju"></a>

## **React简介**

**官网**<br />1. 英文官网: [https://reactjs.org/](https://reactjs.org/)<br />2. 中文官网: https://react.docschina.org/<br />**介绍描述**<br />1. 用于动态构建用户界面的 JavaScript 库(只关注于视图)<br />2. 由Facebook开源<br />**React的特点**<br />1. 声明式编码<br />2. 组件化编码<br />3. React Native 可以使用于移动端开发<br />4. 高效（优秀的Diffing算法）<br />**React高效的原因**<br />1. 使用虚拟(virtual)DOM, 不总是直接操作页面真实DOM。<br />2. DOM Diffing算法, 最小化页面重绘。
<a name="h0Cuu"></a>

## **React的基本使用**

<a name="nprh5"></a>

### **效果**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689059289915-6cd36cbe-e9e4-47f9-ba21-863a40a5bcd7.png#averageHue=%23f4f0ef&clientId=uf6354842-c5be-4&from=paste&height=270&id=u48b20494&originHeight=270&originWidth=298&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13726&status=done&style=shadow&taskId=uc326a1df-1e02-4c83-a95d-6539e8fdb1d&title=&width=298)

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>hello_react</title>
  </head>
  <body>
    <!-- 准备好一个“容器” -->
    <div id="test"></div>

    <!-- 引入react核心库 -->
    <script type="text/javascript" src="../js/react.development.js"></script>
    <!-- 引入react-dom，用于支持react操作DOM -->
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <!-- 引入babel，用于将jsx转为js -->
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel" > /* 此处一定要写babel */
      //1.创建虚拟DOM
      const VDOM = <h1>Hello,React</h1> /* 此处一定不要写引号，因为不是字符串 ,直接写标签就可以了*/
      //2.渲染虚拟DOM到页面
      ReactDOM.render(VDOM,document.getElementById('test'))
    </script>
  </body>
</html>
```

<a name="MK4kL"></a>

### **相关js库**

1. react.js：React核心库。<br />2. react-dom.js：提供操作DOM的react扩展库。<br />3. babel.min.js：解析JSX语法代码转为JS代码的库。
   <a name="JS5Ix"></a>

### **创建虚拟DOM的两种方式**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689059300497-2d8a9ca1-3591-428c-b0cd-2d6892ee13f7.png#averageHue=%23f4efef&clientId=uf6354842-c5be-4&from=paste&height=340&id=ue030772a&originHeight=340&originWidth=642&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24334&status=done&style=shadow&taskId=uf8654150-bd34-468c-8d7f-5c837a77487&title=&width=642)<br />1. 纯JS方式(一般不用,比如写标签嵌套比较麻烦)<br />比如下面创建一个h1标签里面再创建一个span标签

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>2_使用js创建虚拟DOM</title>
  </head>
  <body>
    <!-- 准备好一个“容器” -->
    <div id="test"></div>

    <!-- 引入react核心库 -->
    <script type="text/javascript" src="../js/react.development.js"></script>
    <!-- 引入react-dom，用于支持react操作DOM -->
    <script type="text/javascript" src="../js/react-dom.development.js"></script>

    <script type="text/javascript" > 
      //1.创建虚拟DOM
      const VDOM = React.createElement('h1',{id:'title'},React.createElement('span',{},'Hello,React'))
      //2.渲染虚拟DOM到页面
      ReactDOM.render(VDOM,document.getElementById('test'))
    </script>
  </body>
</html>
```

2. JSX方式

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>1_使用jsx创建虚拟DOM</title>
  </head>
  <body>
    <!-- 准备好一个“容器” -->
    <div id="test"></div>

    <!-- 引入react核心库 -->
    <script type="text/javascript" src="../js/react.development.js"></script>
    <!-- 引入react-dom，用于支持react操作DOM -->
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <!-- 引入babel，用于将jsx转为js -->
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel" > /* 此处一定要写babel */
      //1.创建虚拟DOM
      const VDOM = (  /* 此处一定不要写引号，因为不是字符串 */
        <h1 id="title">
      		/* 这里就可以很容易实现标签嵌套了 */
       	 <span>Hello,React</span>
      	</h1>
      )
      //2.渲染虚拟DOM到页面
      ReactDOM.render(VDOM,document.getElementById('test'))
    </script>
  </body>
</html>
```

<a name="ZqFNh"></a>

### **虚拟DOM与真实DOM**

1. React提供了一些API来创建一种 “特别” 的一般js对象<br />`** const VDOM = React.createElement('xx',{id:'xx'},'xx')**`<br /> 上面创建的就是一个简单的虚拟DOM对象<br />2. 虚拟DOM对象(上面我们在jsx里面写的标签就是虚拟DOM)最终都会被React转换为真实的DOM<br />3. 我们编码时基本只需要操作react的虚拟DOM相关数据, react会转换为真实DOM变化而更新。<br />关于虚拟DOM：<br />1.本质是Object类型的对象（一般对象）<br />2.虚拟DOM比较“轻”，真实DOM比较“重”，因为虚拟DOM是React内部在用，无需真实DOM上那么多的属性。<br />3.虚拟DOM最终会被React转化为真实DOM，呈现在页面上。
   <a name="gSNDJ"></a>

## **React JSX**

<a name="mjtPa"></a>

### **效果**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689059322095-9a4a52d9-6940-4d64-b4a0-6a4a02ecfe98.png#averageHue=%23f7f4f3&clientId=uf6354842-c5be-4&from=paste&height=447&id=u7ecd4e1b&originHeight=447&originWidth=532&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26000&status=done&style=shadow&taskId=ua83bdbbd-e377-4142-a7e9-2d58f8712b9&title=&width=532)
<a name="gvCCO"></a>

### **JSX**

1. 全称:  JavaScript XML<br />2. react定义的一种类似于XML的JS扩展语法: JS + XML本质是`**React.createElement(component, props, ...children)**`方法的语法糖<br />3. 作用: 用来简化创建虚拟DOM <br />1) 写法：`**var ele = <h1>Hello JSX!</h1>**`<br />2) 注意1：它不是字符串, 也不是HTML/XML标签<br />3) 注意2：它最终产生的就是一个JS对象<br />4. 标签名任意: HTML标签或其它标签<br />5. 标签属性任意: HTML标签属性或其它属性<br />6. 基本语法规则<br />1) 遇到 <开头的代码, 以标签的语法解析: html同名标签转换为html同名元素, 其它标签需要特别解析<br />2) 遇到以 { 开头的代码，以JS语法解析: 标签中的js表达式必须用{ }包含<br />7. babel.js的作用<br />1) 浏览器不能直接解析JSX代码, 需要babel转译JSX为纯JS的代码才能运行<br />2) 只要用了JSX，都要加上type="text/babel", 声明需要babel来处理

**jsx语法规则：**<br />  1.定义虚拟DOM时，不要写引号。<br />  2.标签中混入JS表达式时要用{}。<br />  3.样式的类名指定不要用class，要用className。<br />

```
  4.内联样式，要用style={{key:value}}的形式去写。<br />
```

  5.只有一个根标签<br />  6.标签必须闭合<br />  7.标签首字母<br />    (1).若小写字母开头，则将该标签转为html中同名元素，若html中无该标签对应的同名元素，则报错。(小写字母开头说明就是普通的html标签)<br />    (2).若大写字母开头，react就去渲染对应的组件，若组件没有定义，则报错。（大写字母开头就说明是自己自定义的组件）

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>jsx语法规则</title>
    <style>
      .title{
        background-color: orange;
        width: 200px;
      }
    </style>
  </head>
  <body>
    <!-- 准备好一个“容器” -->
    <div id="test"></div>

    <!-- 引入react核心库 -->
    <script type="text/javascript" src="../js/react.development.js"></script>
    <!-- 引入react-dom，用于支持react操作DOM -->
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <!-- 引入babel，用于将jsx转为js -->
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel" >
      const myId = 'aTgUiGu'
      const myData = 'HeLlo,rEaCt'
      //1.创建虚拟DOM
      const VDOM = (
      <div>
        <h2 className="title" id={myId.toLowerCase()}>
<!--       第一个{}的意思是这里面是js表达式，第二个{}表示这里面是一个对象,style的值在虚拟dom里面就是一个对象 -->
        	<span style={{color:'white',fontSize:'29px'}}>{myData.toLowerCase()}</span>
     	  </h2>
        <h2 className="title" id={myId.toUpperCase()}>
          <span style={{color:'white',fontSize:'29px'}}>{myData.toLowerCase()}</span>
        </h2>
        <input type="text"/>
    	</div>
      )
      //2.渲染虚拟DOM到页面
      ReactDOM.render(VDOM,document.getElementById('test'))
    </script>
  </body>
</html>
```

<a name="h8ZiM"></a>

### **渲染虚拟DOM(元素)**

1. 语法:  `**ReactDOM.render(virtualDOM, containerDOM)**`<br />2. 作用: **将虚拟DOM元素**渲染到页面中的**真实容器DOM**中显示<br />3. 参数说明<br />1) 参数一: 纯js或jsx(一般都是jsx)创建的虚拟dom对象<br />2) 参数二: 用来包含虚拟DOM元素的真实dom元素对象(一般是html页面中的一个div)


一定注意区分：【js语句(代码)】与【js表达式】<br />1.表达式：一个表达式会产生一个值，可以放在任何一个需要值的地方<br />下面这些都是表达式：<br />  (1). a<br />  (2). a+b<br />  (3). demo(1)<br />  (4). arr.map() <br />  (5). function test () {}<br />2.语句(代码)：<br />下面这些都是语句(代码)：<br />  (1).if(){}<br />  (2).for(){}<br />  	(3).switch(){case:xxxx}
<a name="V4Pi4"></a>

### **JSX练习**

需求: 动态展示如下列表

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>jsx小练习</title>
  </head>
  <body>
    <!-- 准备好一个“容器” -->
    <div id="test"></div>

    <!-- 引入react核心库 -->
    <script type="text/javascript" src="../js/react.development.js"></script>
    <!-- 引入react-dom，用于支持react操作DOM -->
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <!-- 引入babel，用于将jsx转为js -->
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel" >
      //模拟一些数据
      const data = ['Angular','React','Vue']
      //1.创建虚拟DOM
      const VDOM = (
      <div>
        <h1>前端js框架列表</h1>
        <ul>
          {
						<!--   {}里面是map的遍历方法，return返回的是每个小li，{}得到的结果是遍历出来的全部li，然后组合形成虚拟dom  	   -->
            data.map((item,index)=>{
            	return <li key={index}>{item}</li>
            })
          }
      	</ul>
      </div>
      )
      //2.渲染虚拟DOM到页面
      ReactDOM.render(VDOM,document.getElementById('test'))
    </script>
  </body>
</html>
```

<a name="Mpeza"></a>

## **模块与组件、模块化与组件化的理解**

<a name="FMllj"></a>

### **模块**

1. 理解：向外提供特定功能的js程序, 一般就是一个js文件<br />2. 为什么要拆成模块：随着业务逻辑增加，代码越来越多且复杂。<br />3. 作用：复用js, 简化js的编写, 提高js运行效率
   <a name="ZMwxe"></a>

### **组件**

1. 理解：用来实现局部功能效果的代码和资源的集合(html/css/js/image等等)<br />2. 为什么要用组件： 一个界面的功能更复杂<br />3. 作用：复用编码, 简化项目编码, 提高运行效率
   <a name="IqFoJ"></a>

### **模块化**

当应用的js都以模块来编写的, 这个应用就是一个模块化的应用
<a name="npCCK"></a>

### **组件化**

当应用是以多组件的方式实现, 这个应用就是一个组件化的应用
<a name="hdgu0"></a>

# **第2章：React面向组件编程**

<a name="HU8eD"></a>

## **2.1. 基本理解和使用**

<a name="qDsSG"></a>

### **2.1.1. 使用React开发者工具调试**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689059800830-202ade51-a708-40d5-a2fd-6462d47a0b2e.png#averageHue=%23fdfcfc&clientId=uf6354842-c5be-4&from=paste&height=293&id=u9caddddb&originHeight=293&originWidth=1122&originalType=binary&ratio=1&rotation=0&showTitle=false&size=34059&status=done&style=shadow&taskId=u54a08810-f42c-4204-bc9f-80304cca9e3&title=&width=1122)
<a name="bxh8c"></a>

### **2.1.2. 效果**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689059805386-a4be6c03-a044-4942-a0bf-587fb377e73c.png#averageHue=%23f0eeee&clientId=uf6354842-c5be-4&from=paste&height=408&id=u36cedd10&originHeight=408&originWidth=934&originalType=binary&ratio=1&rotation=0&showTitle=false&size=39159&status=done&style=shadow&taskId=uf9657ffe-12b9-4fc6-aea4-b0bb1bdeab5&title=&width=934)
<a name="eM6W2"></a>

### 创建组件的两种方法

<a name="JC8qA"></a>

#### 函数式组件

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>1_函数式组件</title>
</head>
<body>
<!-- 准备好一个“容器” -->
<div id="test"></div>

<!-- 引入react核心库 -->
<script type="text/javascript" src="../js/react.development.js"></script>
<!-- 引入react-dom，用于支持react操作DOM -->
<script type="text/javascript" src="../js/react-dom.development.js"></script>
<!-- 引入babel，用于将jsx转为js -->
<script type="text/javascript" src="../js/babel.min.js"></script>

<script type="text/babel">
  //1.创建函数式组件
  function MyComponent(){
    console.log(this); //此处的this是undefined，因为babel编译后开启了严格模式，所以函数式组件没有this
    return <h2>我是用函数定义的组件(适用于【简单组件】的定义)</h2>
  }
  //2.渲染组件到页面
  ReactDOM.render(<MyComponent/>,document.getElementById('test'))
</script>
</body>
</html>
```

执行了ReactDOM.render(<MyComponent/>.......之后，发生了什么？<br />1.React解析组件标签，找到了MyComponent组件。<br />2.发现组件是使用函数定义的，随后调用该函数，将返回的虚拟DOM转为真实DOM，随后呈现在页面中。

<a name="ixYGV"></a>

#### 类式组件

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>2_类式组件</title>
</head>
<body>
<!-- 准备好一个“容器” -->
<div id="test"></div>

<!-- 引入react核心库 -->
<script type="text/javascript" src="../js/react.development.js"></script>
<!-- 引入react-dom，用于支持react操作DOM -->
<script type="text/javascript" src="../js/react-dom.development.js"></script>
<!-- 引入babel，用于将jsx转为js -->
<script type="text/javascript" src="../js/babel.min.js"></script>

<script type="text/babel">
//1.创建类式组件
class MyComponent extends React.Component {
  render(){
    //render是放在哪里的？—— MyComponent的原型对象上，供实例（类）使用。
    //render中的this是谁？—— MyComponent的实例对象 <=> MyComponent组件实例对象。
    console.log('render中的this:',this);
    return <h2>我是用类定义的组件(适用于【复杂组件】的定义)</h2>
  }
}
  //2.渲染组件到页面
  ReactDOM.render(<MyComponent/>,document.getElementById('test'))
</script>
</body>
</html>
```

执行了ReactDOM.render(<MyComponent/>.......之后，发生了什么？<br />1.React解析组件标签，找到了MyComponent组件。<br />2.发现组件是使用类定义的，随后new出来该类的实例（React帮我们new），并通过该实例调用到原型上的render方法。<br />3.将render返回的虚拟DOM转为真实DOM，随后呈现在页面中。
<a name="J2KDM"></a>

### **2.1.3. 注意**

1. 组件名必须首字母大写<br />2. 虚拟DOM元素只能有一个根元素<br />3. 虚拟DOM元素必须有结束标签
   <a name="naHlb"></a>

### **2.1.4. 渲染类组件标签的基本流程**

1. React内部会创建组件实例对象<br />2. 调用render()得到虚拟DOM, 并解析为真实DOM<br />3. 插入到指定的页面元素内部
   <a name="ngOwP"></a>

## **2.2. 组件实例（也就是类组件，不是函数组件）的三大核心属性1: state**

<a name="Q6vCY"></a>

### **2.2.1. 效果**

_需求: 定义一个展示天气信息的组件_<br />_1. 默认展示天气炎热或凉爽_<br />_2. 点击文字切换天气_<br />![Weather组件 (2).gif](https://cdn.nlark.com/yuque/0/2023/gif/27086425/1689059828689-82ccb1b3-228d-4217-a0f7-55b2dc77b626.gif#averageHue=%23fbfcfc&clientId=uf6354842-c5be-4&from=paste&height=368&id=u41866b22&originHeight=368&originWidth=540&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14106&status=done&style=shadow&taskId=udbe7f301-bea5-40d5-8839-cab89451a00&title=&width=540)
<a name="lWI0z"></a>

### **2.2.2. 理解 **

1. state是组件对象最重要的属性, 值是对象(可以包含多个key-value的组合)<br />2. 组件被称为"状态机", 通过更新组件的state来更新对应的页面显示(重新渲染组件)
   <a name="PKAPa"></a>

### **2.2.3. 强烈注意**

1. 组件中render方法中的this为组件实例对象<br />2. 组件自定义的方法中this为undefined，如何解决？<br />a) 强制绑定this: 通过函数对象的bind()<br />b) 箭头函数<br />3. 状态数据，不能直接修改或更新（需要调用相关的方法，setState）

State基础写法

```html
<!DOCTYPE html>
<html lang="en">

  <head>
    <meta charset="UTF-8">
    <title>state</title>
  </head>

  <body>
    <!-- 准备好一个“容器” -->
    <div id="test"></div>

    <!-- 引入react核心库 -->
    <script type="text/javascript" src="../js/react.development.js"></script>
    <!-- 引入react-dom，用于支持react操作DOM -->
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <!-- 引入babel，用于将jsx转为js -->
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
      //1.创建组件
      class Weather extends React.Component {

        //构造器调用几次？ ———— 1次
        constructor(props) {
          console.log('constructor');
          super(props)
          //初始化状态
          this.state = { isHot: false, wind: '微风' }
          //解决changeWeather中this指向问题
          this.changeWeather = this.changeWeather.bind(this)
        }
  
        //render调用几次？ ———— 1+n次 1是初始化的那次 n是状态更新的次数
        render() {
          console.log('render');
          // 这里打印的是Weather实例对象，也就是说这个方法是由Weather实例对象调用的
          console.log(this)
          //读取状态
          const { isHot, wind } = this.state
          return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}，{wind}</h1>
        }
  
        //changeWeather调用几次？ ———— 点几次调几次
        // 但是这个方法不是由Weather实例对象调用的，所以this不是Weather实例对象
        changeWeather() {
          //changeWeather放在哪里？ ———— Weather的原型对象上，供实例使用
          //由于changeWeather是作为onClick的回调，所以不是通过实例调用的，是直接调用（直接调用的意思就是直接在js最外层进行调用，和通过window进行调用是一个意思）
          //类中的方法默认开启了局部的严格模式，所以changeWeather中的this为undefined
    
          console.log('changeWeather');
          //获取原来的isHot值
          const isHot = this.state.isHot
          //严重注意：状态必须通过setState进行更新,且更新是一种合并，不是替换。
          //也就是说只会替换isHot这个属性的值，不会改变其他属性的值
          this.setState({ isHot: !isHot })
          console.log(this);//这里指向的是实例对象
    
          //严重注意：状态(state)不可直接更改，下面这行就是直接更改！！！
          //this.state.isHot = !isHot //这是错误的写法
        }
      }
      //2.渲染组件到页面
      ReactDOM.render(<Weather />, document.getElementById('test'))

    </script>
  </body>

</html>
```


state的简写方式

```html
<!DOCTYPE html>
<html lang="en">

  <head>
    <meta charset="UTF-8">
    <title>state简写方式</title>
  </head>

  <body>
    <!-- 准备好一个“容器” -->
    <div id="test"></div>

    <!-- 引入react核心库 -->
    <script type="text/javascript" src="../js/react.development.js"></script>
    <!-- 引入react-dom，用于支持react操作DOM -->
    <script type="text/javascript" src="../js/react-dom.development.js"></script>
    <!-- 引入babel，用于将jsx转为js -->
    <script type="text/javascript" src="../js/babel.min.js"></script>

    <script type="text/babel">
      //1.创建组件
      class Weather extends React.Component {
        //初始化状态
        // 在类里面可以直接写属性名来给属性赋值，不需要用let和var进行声明
        state = { isHot: false, wind: '微风' }
  
        render() {
          const { isHot, wind } = this.state
          return <h1 onClick={this.changeWeather}>今天天气很{isHot ? '炎热' : '凉爽'}，{wind}</h1>
        }
  
        //下面的意思是新建一个函数然后给这个类赋值一个名叫changeWeather的属性，属性值是一个函数
        // 需要注意的是这个函数是箭头函数，没有自己的this，只能去外侧去找，调用这个函数的是react，但是这里是箭头函数就会去找这个组件实例对象
        //自定义方法————要用赋值语句的形式+箭头函数
        changeWeather = () => {
          const isHot = this.state.isHot
          this.setState({ isHot: !isHot })
        }
      }
      //2.渲染组件到页面
      ReactDOM.render(<Weather />, document.getElementById('test'))

    </script>
  </body>

</html>
```

<a name="RtfpA"></a>

## **2.3. 组件三大核心属性2: props**

<a name="Crlvk"></a>

### **2.3.1. 效果**

_需求: 自定义用来显示一个人员信息的组件_<br />_1. 姓名必须指定，且为字符串类型；_<br />_2. 性别为字符串类型，如果性别没有指定，默认为男_<br />_3. 年龄为字符串类型，且为数字类型，默认值为18_<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689059851495-ca06760c-ea9b-4134-8751-044e80db12c8.png#averageHue=%23f9f8f7&clientId=uf6354842-c5be-4&from=paste&height=212&id=u96f5f4a9&originHeight=212&originWidth=229&originalType=binary&ratio=1&rotation=0&showTitle=false&size=5021&status=done&style=shadow&taskId=u1a2df832-1970-4bec-a464-c0eebbdb8af&title=&width=229)
<a name="yiaOl"></a>

### **2.3.2. 理解**

1. 每个组件对象都会有props(properties的简写)属性<br />2. 组件标签的所有属性都保存在props中
   <a name="CG9DO"></a>

### **2.3.3. 作用**

1. 通过标签属性**从组件外**向**组件内（父子传递）**传递变化的数据<br />2. 注意: 组件内部（子组件不能修改props数据，也就是说props是只读的单向数据流）不要修改props数据
   <a name="TK86O"></a>

### **2.3.4. 编码操作**

1. 内部读取某个属性值`**this**.**props**.**name**`<br />2. 对props中的属性值进行类型限制和必要性限制<br />第一种方式（React v15.5 开始已弃用）：<br />`_Person_.**propTypes **= {<br /> **name**: **React**.**PropTypes**.**string**.isRequired,<br /> **age**: **React**.**PropTypes**.**number**<br />}`<br />第二种方式（新）：使用prop-types库进限制（需要引入prop-types库,因为大部分情况下我们都不需要进行类型限制，所以React就没帮我们引入）<br />`_Person_.**propTypes **= {<br />  **name**: **PropTypes**.**string**.isRequired,<br />  **age**: **PropTypes**.**number**. <br />}`<br />3. 扩展属性: 将对象的所有属性通过props传递(这也是js的扩展运算符语法。对于对象的扩展运算符，会把属性名作为等号左边，属性值作为等号右边，不过我觉得没啥用，因为可以直接传一个对象过去。)`<**Person **{...**_person_**}/>`<br />4. 默认属性值：<br />`Person.**defaultProps **= {<br />  **age**: 18,<br />  **sex**:**'男'<br />**}`<br />5. 组件类的构造函数<br />`**constructor**(props){<br />  **super**(props)<br />  **console**.log(props)_//打印所有属性<br />_}`



props的基本使用

```html
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="UTF-8">
	<title>props基本使用</title>
</head>

<body>
	<!-- 准备好一个“容器” -->
	<div id="test1"></div>
	<div id="test2"></div>
	<div id="test3"></div>

	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>

	<script type="text/babel">
		//创建组件
		class Person extends React.Component {
			render() {
				// console.log(this);
				const { name, age, sex } = this.props
				return (
					<ul>
						<li>姓名：{name}</li>
						<li>性别：{sex}</li>
						<li>年龄：{age + 1}</li>
					</ul>
				)
			}
		}
		//渲染组件到页面
		ReactDOM.render(<Person name="jerry" age={19} sex="男" />, document.getElementById('test1'))
		ReactDOM.render(<Person name="tom" age={18} sex="女" />, document.getElementById('test2'))

		const p = { name: '老刘', age: 18, sex: '女' }
		// 批量传递属性到组件
		// ReactDOM.render(<Person name={p.name} age={p.age} sex={p.sex}/>,document.getElementById('test3'))
		ReactDOM.render(<Person {...p} />, document.getElementById('test3'))
	</script>
</body>

</html>
```

或者直接传递一个对象到子组件，就不用用展开运算符了

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>props基本使用</title>
</head>

<body>
  <!-- 准备好一个“容器” -->
  <div id="test1"></div>


  <!-- 引入react核心库 -->
  <script type="text/javascript" src="./js/react.development.js"></script>
  <!-- 引入react-dom，用于支持react操作DOM -->
  <script type="text/javascript" src="./js/react-dom.development.js"></script>
  <!-- 引入babel，用于将jsx转为js -->
  <script type="text/javascript" src="./js/babel.min.js"></script>

  <script type="text/babel">
    //创建组件
    class Person extends React.Component {
      render() {
        console.log(this.props);
        console.log(this.props.p.name)
        const { name, age, sex } = this.props.p
        return (
          <ul>
            <li>姓名：{name}</li>
            <li>性别：{sex}</li>
            <li>年龄：{age + 1}</li>
          </ul>
        )
      }
    }
    const p = { name: '老刘', age: 18, sex: '女' }
    // 直接传递一个对象到props
    ReactDOM.render(<Person p={p} />, document.getElementById('test1'))
  </script>
</body>

</html>
```

对props进行限制（类型，是否必传）

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>对props进行限制</title>
</head>
<body>
	<!-- 准备好一个“容器” -->
	<div id="test1"></div>
	<div id="test2"></div>
	<div id="test3"></div>
	
	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>
	<!-- 引入prop-types，用于对组件标签属性进行限制 -->
	<script type="text/javascript" src="../js/prop-types.js"></script>

	<script type="text/babel">
		//创建组件
		class Person extends React.Component{
			render(){
				// console.log(this);
				const {name,age,sex} = this.props
				//props是只读的
				//this.props.name = 'jack' //此行代码会报错，因为props是只读的
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
			name:PropTypes.string.isRequired, //限制name必传，且为字符串
			sex:PropTypes.string,//限制sex为字符串
			age:PropTypes.number,//限制age为数值
			speak:PropTypes.func,//限制speak为函数
		}
		//指定默认标签属性值
		Person.defaultProps = {
			sex:'男',//sex默认值为男
			age:18 //age默认值为18
		}
		//渲染组件到页面
		ReactDOM.render(<Person name={100} speak={speak}/>,document.getElementById('test1'))
		ReactDOM.render(<Person name="tom" age={18} sex="女"/>,document.getElementById('test2'))

		const p = {name:'老刘',age:18,sex:'女'}
		// console.log('@',...p);
		// ReactDOM.render(<Person name={p.name} age={p.age} sex={p.sex}/>,document.getElementById('test3'))
		ReactDOM.render(<Person {...p}/>,document.getElementById('test3'))

		function speak(){
			console.log('我说话了');
		}
	</script>
</body>
</html>
```

props的简写方式

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>对props进行限制</title>
</head>
<body>
	<!-- 准备好一个“容器” -->
	<div id="test1"></div>
	<div id="test2"></div>
	<div id="test3"></div>
	
	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>
	<!-- 引入prop-types，用于对组件标签属性进行限制 -->
	<script type="text/javascript" src="../js/prop-types.js"></script>

	<script type="text/babel">
		//创建组件
		class Person extends React.Component{

			constructor(props){
				//构造器是否接收props，是否传递给super，取决于：是否希望在构造器中通过this访问props
				// console.log(props);
				super(props)
				console.log('constructor',this.props);
			}

			//对标签属性进行类型、必要性的限制
			static propTypes = {
				name:PropTypes.string.isRequired, //限制name必传，且为字符串
				sex:PropTypes.string,//限制sex为字符串
				age:PropTypes.number,//限制age为数值
			}

			//指定默认标签属性值
			static defaultProps = {
				sex:'男',//sex默认值为男
				age:18 //age默认值为18
			}
			
			render(){
				// console.log(this);
				const {name,age,sex} = this.props
				//props是只读的
				//this.props.name = 'jack' //此行代码会报错，因为props是只读的
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
		ReactDOM.render(<Person name="jerry"/>,document.getElementById('test1'))
	</script>
</body>
</html>
```

函数组件使用props

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>对props进行限制</title>
</head>
<body>
	<!-- 准备好一个“容器” -->
	<div id="test1"></div>
	<div id="test2"></div>
	<div id="test3"></div>
	
	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>
	<!-- 引入prop-types，用于对组件标签属性进行限制 -->
	<script type="text/javascript" src="../js/prop-types.js"></script>

	<script type="text/babel">
		//创建组件,这里react会自动把props给我们封装到参数中
		function Person (props){
			const {name,age,sex} = props
			return (
					<ul>
						<li>姓名：{name}</li>
						<li>性别：{sex}</li>
						<li>年龄：{age}</li>
					</ul>
				)
		}
		Person.propTypes = {
			name:PropTypes.string.isRequired, //限制name必传，且为字符串
			sex:PropTypes.string,//限制sex为字符串
			age:PropTypes.number,//限制age为数值
		}

		//指定默认标签属性值
		Person.defaultProps = {
			sex:'男',//sex默认值为男
			age:18 //age默认值为18
		}
		//渲染组件到页面
		ReactDOM.render(<Person name="jerry"/>,document.getElementById('test1'))
	</script>
</body>
</html>
```

<a name="thJp3"></a>

## **2.4. 组件三大核心属性3: refs与事件处理**

<a name="vCKQ4"></a>

### **2.4.1. 效果**

_需求: 自定义组件, 功能说明如下:_<br />_1. 点击按钮, 提示第一个输入框中的值_<br />_2. 当第2个输入框失去焦点时, 提示这个输入框中的值_<br />效果如下：<br />![props_event (2).gif](https://cdn.nlark.com/yuque/0/2023/gif/27086425/1689059916127-4f318f7c-183d-48c9-bdab-7c242c5ab8f2.gif#averageHue=%23fbfbfa&clientId=uf6354842-c5be-4&from=paste&height=343&id=u24215be4&originHeight=343&originWidth=556&originalType=binary&ratio=1&rotation=0&showTitle=false&size=62079&status=done&style=shadow&taskId=u33bc94a4-c832-4595-b04a-47e2c91c1c5&title=&width=556)
<a name="Bud1o"></a>

### **2.4.2. 理解**

组件内的标签可以定义ref属性来标识自己
<a name="XzbAS"></a>

### **2.4.3. 编码**

1. 字符串形式的ref`**<input ref="input1"/>**`<br />2. 回调形式的ref`**<input ref={(c)=>{this.input1 = c}}/>**`<br />3. createRef创建ref容器<br />**myRef = React.createRef() **<br />**<input ref={this.myRef}/>**

<a name="Ov3os"></a>

### 2.4.4 示例

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>1_字符串形式的ref</title>
</head>
<body>
	<!-- 准备好一个“容器” -->
	<div id="test"></div>
	
	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>

	<script type="text/babel">
		//创建组件
		class Demo extends React.Component{
			//展示左侧输入框的数据
			showData = ()=>{
        //获取这个输入框虚拟dom对象
				const {input1} = this.refs
        //可以从这个虚拟dom对象拿到值
				alert(input1.value)
			}
			//展示右侧输入框的数据
			showData2 = ()=>{
				const {input2} = this.refs
				alert(input2.value)
			}
			render(){
				return(
					<div>
						<input ref="input1" type="text" placeholder="点击按钮提示数据"/>&nbsp;
            //给虚拟dom绑定点击事件是onClick,然后值是一个函数，注意只需要函数名就可以了
						<button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
						<input ref="input2" onBlur={this.showData2} type="text" placeholder="失去焦点提示数据"/>
					</div>
				)
			}
		}
		//渲染组件到页面
		ReactDOM.render(<Demo a="1" b="2"/>,document.getElementById('test'))
	</script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>1_字符串形式的ref</title>
</head>

<body>
  <!-- 准备好一个“容器” -->
  <div id="test"></div>

  <!-- 引入react核心库 -->
  <script type="text/javascript" src="../js/react.development.js"></script>
  <!-- 引入react-dom，用于支持react操作DOM -->
  <script type="text/javascript" src="../js/react-dom.development.js"></script>
  <!-- 引入babel，用于将jsx转为js -->
  <script type="text/javascript" src="../js/babel.min.js"></script>

  <script type="text/babel">
    //创建组件
    class Demo extends React.Component {
      //展示左侧输入框的数据
      showData = () => {
        const { input1 } = this
        alert(input1.value)
      }
      //展示右侧输入框的数据
      showData2 = () => {
        const { input2 } = this
        alert(input2.value)
      }
      render() {
        return (
          <div>
            // 下面这一行中的ref中的回调函数会被react默认调用，也就是默认赋值，之后就可以通过实例对象的input1属性进行操作document对象了
            //这个参数传的c就是这个标签的虚拟dom对象，然后再给这个实例对象身上的input1属性进行赋值
            <input ref={c => this.input1 = c} type="text" placeholder="点击按钮提示数据" />&nbsp;
            <button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
            <input onBlur={this.showData2} ref={c => this.input2 = c} type="text" placeholder="失去焦点提示数据" />&nbsp;
          </div>
        )
      }
    }
    //渲染组件到页面
    ReactDOM.render(<Demo a="1" b="2" />, document.getElementById('test'))
  </script>
</body>

</html>
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="UTF-8">
	<title>3_回调ref中回调执行次数的问题</title>
</head>

<body>
	<!-- 准备好一个“容器” -->
	<div id="test"></div>

	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>

	<script type="text/babel">
		//创建组件
		class Demo extends React.Component {

			state = { isHot: false }

			showInfo = () => {
				const { input1 } = this
				alert(input1.value)
			}

			changeWeather = () => {
				//获取原来的状态
				const { isHot } = this.state
				//更新状态
				this.setState({ isHot: !isHot })
			}

			// 以类的方式
			saveInput = (c) => {
				this.input1 = c;
				console.log('@', c);
			}

			render() {
				const { isHot } = this.state
				return (
					<div>
						<h2>今天天气很{isHot ? '炎热' : '凉爽'}</h2>
						{/*以内联函数的方式进行，这样的话如果发生修改操作的话会调用两次函数，第一次参数为null，第二次才为实际传的参数
								<input ref={(c)=>{this.input1 = c;console.log('@',c);}} type="text"/><br/><br/>*/}

						{/*下面这个是定义成class的绑定函数，就是在class里面写一个函数进行调用*/}
						<input ref={this.saveInput} type="text" /><br /><br />
						<button onClick={this.showInfo}>点我提示输入的数据</button>
						<button onClick={this.changeWeather}>点我切换天气</button>
					</div>
				)
			}
		}
		//渲染组件到页面
		ReactDOM.render(<Demo />, document.getElementById('test'))
	</script>
</body>

</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>4_createRef</title>
</head>
<body>
	<!-- 准备好一个“容器” -->
	<div id="test"></div>
	
	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>

	<script type="text/babel">
		//创建组件
		class Demo extends React.Component{
			/* React.createRef调用后可以返回一个容器，该容器可以存储被ref所标识的节点,该容器是“专人专用”的*/
			myRef = React.createRef()
			myRef2 = React.createRef()
			//展示左侧输入框的数据
			showData = ()=>{
				alert(this.myRef.current.value);
			}
			//展示右侧输入框的数据
			showData2 = ()=>{
				alert(this.myRef2.current.value);
			}
			render(){
				return(
					<div>
						<input ref={this.myRef} type="text" placeholder="点击按钮提示数据"/>&nbsp;
						<button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
						<input onBlur={this.showData2} ref={this.myRef2} type="text" placeholder="失去焦点提示数据"/>&nbsp;
					</div>
				)
			}
		}
		//渲染组件到页面
		ReactDOM.render(<Demo/>,document.getElementById('test'))
	</script>
</body>
</html>
```

<a name="JLSE0"></a>

### **2.4.5. 事件处理**

1. 通过onXxx属性指定事件处理函数（比如点击事件、失去焦点事件等等）(注意大小写)<br />1) React使用的是自定义(合成)事件, 而不是使用的原生DOM事件—————— 为了更好的兼容性<br />2) React中的事件是通过事件委托方式处理的(委托给组件最外层的元素)————————为了的高效<br />2. 通过event.target得到发生事件的DOM元素对象 ——————————不要过度使用ref，可以直接使用event就好辣

```html
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="UTF-8">
	<title>事件处理</title>
</head>

<body>
	<!-- 准备好一个“容器” -->
	<div id="test"></div>

	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>

	<script type="text/babel">
		//创建组件
		class Demo extends React.Component {
			//创建ref容器
			myRef = React.createRef()
			myRef2 = React.createRef()

			showData = (event) => {
				console.log(event.target);
				alert(this.myRef.current.value);
			}

			showData2 = (event) => {
				// 这里通过事件源也能拿到input表单中输入的值
				// 可以省略ref
				alert(event.target.value);
			}

			render() {
				return (
					<div>
						<input ref={this.myRef} type="text" placeholder="点击按钮提示数据" />&nbsp;
						<button onClick={this.showData}>点我提示左侧的数据</button>&nbsp;
						<input onBlur={this.showData2} type="text" placeholder="失去焦点提示数据" />&nbsp;
					</div>
				)
			}
		}
		//渲染组件到页面
		ReactDOM.render(<Demo />, document.getElementById('test'))
	</script>
</body>

</html>
```

<a name="yj6e7"></a>

## **2.5. 收集表单数据**

<a name="gXXJi"></a>

### **2.5.1. 效果**

_需求: 定义一个包含表单的组件_<br />_输入用户名密码后, 点击登录提示输入信息_<br />![收集表单数据 (2).gif](https://cdn.nlark.com/yuque/0/2023/gif/27086425/1689059947366-0e138011-2c40-4f61-a4e6-abd88e107f8a.gif#averageHue=%23fbfbfb&clientId=uf6354842-c5be-4&from=paste&height=178&id=u21f22345&originHeight=178&originWidth=650&originalType=binary&ratio=1&rotation=0&showTitle=false&size=30086&status=done&style=shadow&taskId=uca0e1b48-40aa-4add-bc47-05b1d6e9f6b&title=&width=650)
<a name="Z0kzq"></a>

### **2.5.2. 理解**

包含表单的组件分类<br />1. 受控组件(vue里面的双向数据绑定)

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>2_受控组件</title>
</head>
<body>
	<!-- 准备好一个“容器” -->
	<div id="test"></div>
	
	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>

	<script type="text/babel">
		//创建组件
		class Login extends React.Component{

			//初始化状态
			state = {
				username:'', //用户名
				password:'' //密码
			}

			//保存用户名到状态中
			saveUsername = (event)=>{
				this.setState({username:event.target.value})
			}

			//保存密码到状态中
			savePassword = (event)=>{
				this.setState({password:event.target.value})
			}

			//表单提交的回调
			handleSubmit = (event)=>{
				event.preventDefault() //阻止表单提交
				const {username,password} = this.state
				alert(`你输入的用户名是：${username},你输入的密码是：${password}`)
			}

			render(){
				return(
					<form onSubmit={this.handleSubmit}>
						用户名：<input onChange={this.saveUsername} type="text" name="username"/>
						密码：<input onChange={this.savePassword} type="password" name="password"/>
						<button>登录</button>
					</form>
				)
			}
		}
		//渲染组件
		ReactDOM.render(<Login/>,document.getElementById('test'))
	</script>
</body>
</html>
```

2. 非受控组件

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>1_非受控组件</title>
</head>
<body>
	<!-- 准备好一个“容器” -->
	<div id="test"></div>
	
	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>

	<script type="text/babel">
		//创建组件
		class Login extends React.Component{
			handleSubmit = (event)=>{
				event.preventDefault() //阻止表单提交
				const {username,password} = this
				alert(`你输入的用户名是：${username.value},你输入的密码是：${password.value}`)
			}
			render(){
				return(
					<form onSubmit={this.handleSubmit}>
						用户名：<input ref={c => this.username = c} type="text" name="username"/>
						密码：<input ref={c => this.password = c} type="password" name="password"/>
						<button>登录</button>
					</form>
				)
			}
		}
		//渲染组件
		ReactDOM.render(<Login/>,document.getElementById('test'))
	</script>
</body>
</html>
```

<a name="QBvZL"></a>

## 高阶函数和函数的柯里化

**高阶函数**：如果一个函数符合下面2个规范中的任何一个，那该函数就是高阶函数。<br />        1.若A函数，接收的参数是一个函数，那么A就可以称之为高阶函数。<br />        2.若A函数，调用的返回值依然是一个函数，那么A就可以称之为高阶函数。<br />        常见的高阶函数有：Promise、setTimeout、arr.map()等等<br />**函数的柯里化**：通过函数调用继续返回函数的方式，实现多次接收参数最后统一处理的函数编码形式。 <br /> function sum(a){<br />    return(b)=>{<br />      return (c)=>{<br />        return a+b+c<br />      }<br />    }<br />  } 

```html
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="UTF-8">
	<title>高阶函数_函数柯里化</title>
</head>

<body>
	<!-- 准备好一个“容器” -->
	<div id="test"></div>

	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>

	<script type="text/babel">

		//创建组件
		class Login extends React.Component {
			//初始化状态
			state = {
				username: '', //用户名
				password: '' //密码
			}

			//保存表单数据到状态中
			saveFormData = (dataType) => {
				return (event) => {
					// 这里使用的还是JavaScript对象相关的知识
					/*
								let a = 'name'
								let obj = {} 
								obj[a] = 'tom'
								console.log(obj);// {name:'tom'}
					*/
					this.setState({ [dataType]: event.target.value })
				}
			}

			//表单提交的回调
			handleSubmit = (event) => {
				event.preventDefault() //阻止表单提交
				const { username, password } = this.state
				alert(`你输入的用户名是：${username},你输入的密码是：${password}`)
			}
			render() {
				return (
					<form onSubmit={this.handleSubmit}>
						用户名：<input onChange={this.saveFormData('username')} type="text" name="username" />
						密码：<input onChange={this.saveFormData('password')} type="password" name="password" />
						<button>登录</button>
					</form>
				)
			}
		}
		//渲染组件
		ReactDOM.render(<Login />, document.getElementById('test'))
	</script>
</body>

</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>2_不用函数柯里化的实现</title>
</head>
<body>
	<!-- 准备好一个“容器” -->
	<div id="test"></div>
	
	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>

	<script type="text/babel">
		//创建组件
		class Login extends React.Component{
			//初始化状态
			state = {
				username:'', //用户名
				password:'' //密码
			}

			//保存表单数据到状态中
			saveFormData = (dataType,event)=>{
				this.setState({[dataType]:event.target.value})
			}

			//表单提交的回调
			handleSubmit = (event)=>{
				event.preventDefault() //阻止表单提交
				const {username,password} = this.state
				alert(`你输入的用户名是：${username},你输入的密码是：${password}`)
			}
			render(){
				return(
					<form onSubmit={this.handleSubmit}>
						用户名：<input onChange={event => this.saveFormData('username',event) } type="text" name="username"/>
						密码：<input onChange={event => this.saveFormData('password',event) } type="password" name="password"/>
						<button>登录</button>
					</form>
				)
			}
		}
		//渲染组件
		ReactDOM.render(<Login/>,document.getElementById('test'))
	</script>
</body>
</html>
```

<a name="OTub5"></a>

## **2.6. 组件的生命周期**

<a name="uribW"></a>

### **2.6.1. 效果**

_需求:定义组件实现以下功能：_<br />_1. 让指定的文本做显示 / 隐藏的渐变动画_<br />_2. 从完全可见，到彻底消失，耗时2S_<br />_3. 点击“不活了”按钮从界面中卸载组件_<br />![component    生命周期.gif](https://cdn.nlark.com/yuque/0/2023/gif/27086425/1689059961921-cd8cceb8-c573-42f9-b2c9-d2c21e3fdd64.gif#averageHue=%23fafafa&clientId=uf6354842-c5be-4&from=paste&height=215&id=ueefa5503&originHeight=215&originWidth=417&originalType=binary&ratio=1&rotation=0&showTitle=false&size=35904&status=done&style=shadow&taskId=u721e813a-4782-49ab-b36a-45c5a041dc1&title=&width=417)

```html
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="UTF-8">
	<title>1_引出生命周期</title>
</head>

<body>
	<!-- 准备好一个“容器” -->
	<div id="test"></div>

	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>

	<script type="text/babel">
		//创建组件
		//生命周期回调函数 <=> 生命周期钩子函数 <=> 生命周期函数 <=> 生命周期钩子
		class Life extends React.Component {

			state = { opacity: 1 }

			death = () => {
				//卸载组件
				ReactDOM.unmountComponentAtNode(document.getElementById('test'))
			}

			//组件挂完毕，这个是生命周期函数，在合适的时间由react调用
			componentDidMount() {
				console.log('componentDidMount');
				this.timer = setInterval(() => {
					//获取原状态
					let { opacity } = this.state
					//减小0.1
					opacity -= 0.1
					if (opacity <= 0) opacity = 1
					//设置新的透明度
					this.setState({ opacity })
				}, 200);
			}

			//组件将要卸载.这个是生命周期函数，在合适的时间由react调用
			componentWillUnmount() {
				//清除定时器
				clearInterval(this.timer)
			}

			//初始化渲染、状态更新之后
			render() {
				console.log('render');
				return (
					<div>
						<h2 style={{ opacity: this.state.opacity }}>React学不会怎么办？</h2>
						<button onClick={this.death}>不活了</button>
					</div>
				)
			}
		}
		//渲染组件
		ReactDOM.render(<Life />, document.getElementById('test'))
	</script>
</body>

</html>
```

<a name="p2DIi"></a>

### **2.6.2. 理解**

1. 组件从创建到死亡它会经历一些特定的阶段。<br />2. React组件中包含一系列勾子函数(生命周期回调函数), 会在特定的时刻调用。<br />3. 我们在定义组件时，会在特定的生命周期回调函数中，做特定的工作。
   <a name="zAoSe"></a>

### **2.6.3. 生命周期流程图(旧)**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689059974669-fae265c8-e30e-4768-9f00-5e4a3f3a4092.png#averageHue=%23f5f5f5&clientId=uf6354842-c5be-4&from=paste&height=670&id=ud5e2e44c&originHeight=670&originWidth=841&originalType=binary&ratio=1&rotation=0&showTitle=false&size=57959&status=done&style=shadow&taskId=u9e6f8fb5-0fab-4711-b038-975b791253a&title=&width=841)<br />生命周期的三个阶段（旧）<br />**	1. 初始化阶段:** 由ReactDOM.render()触发---初次渲染<br />1. constructor()<br />2. componentWillMount()<br />3. render()<br />4. componentDidMount()<br />**	2. 更新阶段: **由组件内部this.setSate()或父组件重新render触发<br />1. shouldComponentUpdate()    组件是否应该更新的阀门  返回true为放行，为false就不往下执行了(默认返回为真)<br />2. componentWillUpdate()    组件将要更新（强制更新，这里可以不更改state就强制更新）<br />3. render()<br />4. componentDidUpdate()   更新完毕的钩子<br />	**3. 卸载组件: **由ReactDOM.unmountComponentAtNode()触发<br />1. componentWillUnmount()


```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>2_react生命周期(旧)</title>
</head>
<body>
	<!-- 准备好一个“容器” -->
	<div id="test"></div>
	
	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/babel.min.js"></script>

	<script type="text/babel">
		//创建组件
		class Count extends React.Component{

			//构造器
			constructor(props){
				console.log('Count---constructor');
				super(props)
				//初始化状态
				this.state = {count:0}
			}

			//加1按钮的回调
			add = ()=>{
				//获取原状态
				const {count} = this.state
				//更新状态
				this.setState({count:count+1})
			}

			//卸载组件按钮的回调
			death = ()=>{
				ReactDOM.unmountComponentAtNode(document.getElementById('test'))
			}

			//强制更新按钮的回调
			force = ()=>{
				this.forceUpdate()
			}

			//组件将要挂载的钩子
			componentWillMount(){
				console.log('Count---componentWillMount');
			}

			//组件挂载完毕的钩子
			componentDidMount(){
				console.log('Count---componentDidMount');
			}

			//组件将要卸载的钩子
			componentWillUnmount(){
				console.log('Count---componentWillUnmount');
			}

			//控制组件更新的“阀门”
			shouldComponentUpdate(){
				console.log('Count---shouldComponentUpdate');
				return true
			}

			//组件将要更新的钩子
			componentWillUpdate(){
				console.log('Count---componentWillUpdate');
			}

			//组件更新完毕的钩子
			componentDidUpdate(){
				console.log('Count---componentDidUpdate');
			}

			render(){
				console.log('Count---render');
				const {count} = this.state
				return(
					<div>
						<h2>当前求和为：{count}</h2>
						<button onClick={this.add}>点我+1</button>
						<button onClick={this.death}>卸载组件</button>
						<button onClick={this.force}>不更改任何状态中的数据，强制更新一下</button>
					</div>
				)
			}
		}
		
		//父组件A
		class A extends React.Component{
			//初始化状态
			state = {carName:'奔驰'}

			changeCar = ()=>{
				this.setState({carName:'奥拓'})
			}

			render(){
				return(
					<div>
						<div>我是A组件</div>
						<button onClick={this.changeCar}>换车</button>
						<B carName={this.state.carName}/>
					</div>
				)
			}
		}
		
		//子组件B
		class B extends React.Component{
			//组件将要接收新的props的钩子
      //这个生命周期钩子的名字其实不太到位，应该是子组件接收到新的属性才会调用这个钩子，这也算一个坑吧
			componentWillReceiveProps(props){
				console.log('B---componentWillReceiveProps',props);
			}

			//控制组件更新的“阀门”
			shouldComponentUpdate(){
				console.log('B---shouldComponentUpdate');
				return true
			}
			//组件将要更新的钩子
			componentWillUpdate(){
				console.log('B---componentWillUpdate');
			}

			//组件更新完毕的钩子
			componentDidUpdate(){
				console.log('B---componentDidUpdate');
			}

			render(){
				console.log('B---render');
				return(
					<div>我是B组件，接收到的车是:{this.props.carName}</div>
				)
			}
		}
		
		//渲染组件，这里如果看AB的关系需要调整下面的代码为A就好啦
		ReactDOM.render(<Count/>,document.getElementById('test'))
	</script>
</body>
</html>
```

<a name="DNF3t"></a>

### **2.6.4. 生命周期流程图(新)**

旧版生命周期流程图<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689085374201-83fae897-e52b-42e2-88a5-dac4c3da242b.png#averageHue=%23f5f4f4&clientId=ud5533b38-1a7d-4&from=paste&height=589&id=u16c42be4&originHeight=736&originWidth=924&originalType=binary&ratio=1&rotation=0&showTitle=false&size=126516&status=done&style=shadow&taskId=u3b48b8cf-34ca-451b-aeba-6ed979b20ac&title=&width=739.2)<br />新版生命周期流程图<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689060000071-25d23dc0-4e13-4190-b5a0-acd91c650773.png#averageHue=%23f5f4f1&clientId=uf6354842-c5be-4&from=paste&height=654&id=ubf310ffd&originHeight=654&originWidth=941&originalType=binary&ratio=1&rotation=0&showTitle=false&size=76538&status=done&style=shadow&taskId=u7bc58820-f5f7-4ca2-84b5-48e7f2da1e1&title=&width=941)<br />生命周期的三个阶段（新）<br />**1. 初始化阶段:** 由ReactDOM.render()触发---初次渲染<br />1. constructor()<br />**2. getDerivedStateFromProps               当state的值在任何时候都取决于props的时候才用这个钩子**<br />3. render()<br />4. componentDidMount()<br />**	2. 更新阶段: **由组件内部this.setSate()或父组件重新render触发<br />**1. getDerivedStateFromProps**<br />2. shouldComponentUpdate()<br />3. render()<br />**4. getSnapshotBeforeUpdate    快照值可以是任何类型的值**<br />5. componentDidUpdate()   这里函数里面可以有两个参数，一个preProps，一个preState <br />	**3. 卸载组件: **由ReactDOM.unmountComponentAtNode()触发<br />1. componentWillUnmount()
<a name="Am9pP"></a>

#### 2.6.4.1 具体实例

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>3_react生命周期(新)</title>
</head>
<body>
	<!-- 准备好一个“容器” -->
	<div id="test"></div>
	
	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/17.0.1/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/17.0.1/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/17.0.1/babel.min.js"></script>

	<script type="text/babel">
		//创建组件
		class Count extends React.Component{
			//构造器
			constructor(props){
				console.log('Count---constructor');
				super(props)
				//初始化状态
				this.state = {count:0}
			}

			//加1按钮的回调
			add = ()=>{
				//获取原状态
				const {count} = this.state
				//更新状态
				this.setState({count:count+1})
			}

			//卸载组件按钮的回调
			death = ()=>{
				ReactDOM.unmountComponentAtNode(document.getElementById('test'))
			}

			//强制更新按钮的回调
			force = ()=>{
				this.forceUpdate()
			}
			
			//若state的值在任何时候都取决于props，那么可以使用getDerivedStateFromProps
			static getDerivedStateFromProps(props,state){
				console.log('getDerivedStateFromProps',props,state);
				return null
			}

			//在更新之前获取快照
			getSnapshotBeforeUpdate(){
				console.log('getSnapshotBeforeUpdate');
				return 'atguigu'
			}

			//组件挂载完毕的钩子
			componentDidMount(){
				console.log('Count---componentDidMount');
			}

			//组件将要卸载的钩子
			componentWillUnmount(){
				console.log('Count---componentWillUnmount');
			}

			//控制组件更新的“阀门”
			shouldComponentUpdate(){
				console.log('Count---shouldComponentUpdate');
				return true
			}

			//组件更新完毕的钩子
			componentDidUpdate(preProps,preState,snapshotValue){
				console.log('Count---componentDidUpdate',preProps,preState,snapshotValue);
			}
			
			render(){
				console.log('Count---render');
				const {count} = this.state
				return(
					<div>
						<h2>当前求和为：{count}</h2>
						<button onClick={this.add}>点我+1</button>
						<button onClick={this.death}>卸载组件</button>
						<button onClick={this.force}>不更改任何状态中的数据，强制更新一下</button>
					</div>
				)
			}
		}
		
		//渲染组件
		ReactDOM.render(<Count count={199}/>,document.getElementById('test'))
	</script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>4_getSnapShotBeforeUpdate的使用场景</title>
	<style>
		.list{
			width: 200px;
			height: 150px;
			background-color: skyblue;
			overflow: auto;
		}
		.news{
			height: 30px;
		}
	</style>
</head>
<body>
	<!-- 准备好一个“容器” -->
	<div id="test"></div>
	
	<!-- 引入react核心库 -->
	<script type="text/javascript" src="../js/17.0.1/react.development.js"></script>
	<!-- 引入react-dom，用于支持react操作DOM -->
	<script type="text/javascript" src="../js/17.0.1/react-dom.development.js"></script>
	<!-- 引入babel，用于将jsx转为js -->
	<script type="text/javascript" src="../js/17.0.1/babel.min.js"></script>

	<script type="text/babel">
		class NewsList extends React.Component{

			state = {newsArr:[]}

			componentDidMount(){
				setInterval(() => {
					//获取原状态
					const {newsArr} = this.state
					//模拟一条新闻
					const news = '新闻'+ (newsArr.length+1)
					//更新状态
					this.setState({newsArr:[news,...newsArr]})
				}, 1000);
			}

			getSnapshotBeforeUpdate(){
				return this.refs.list.scrollHeight
			}

			componentDidUpdate(preProps,preState,height){
				this.refs.list.scrollTop += this.refs.list.scrollHeight - height
			}

			render(){
				return(
					<div className="list" ref="list">
						{
							this.state.newsArr.map((n,index)=>{
								return <div key={index} className="news">{n}</div>
							})
						}
					</div>
				)
			}
		}
		ReactDOM.render(<NewsList/>,document.getElementById('test'))
	</script>
</body>
</html>
```

<a name="tq2kE"></a>

### **2.6.5. 重要的勾子**

1. render：初始化渲染或更新渲染调用<br />2. componentDidMount：开启监听, 发送ajax请求<br />3. componentWillUnmount：做一些收尾工作, 如: 清理定时器
   <a name="HYwAV"></a>

### **2.6.6. 即将废弃的勾子**

1. componentWillMount<br />2. componentWillReceiveProps<br />3. componentWillUpdate<br />现在使用会出现警告，下一个大版本需要加上UNSAFE_前缀才能使用，以后可能会被彻底废弃，不建议使用。虽然这三个基本都不用
   <a name="bIeUP"></a>

## **2.7. 虚拟DOM与DOM Diffing算法**

<a name="AGibb"></a>

### **2.7.1. 效果**

_需求：验证虚拟DOM Diffing算法的存在_<br />![component    虚拟DOM.gif](https://cdn.nlark.com/yuque/0/2023/gif/27086425/1689060027452-e3fb8d7a-df1e-4abb-9942-2a3a9fc90ad9.gif#averageHue=%23faf9f8&clientId=uf6354842-c5be-4&from=paste&height=303&id=ufe9d2845&originHeight=303&originWidth=578&originalType=binary&ratio=1&rotation=0&showTitle=false&size=317303&status=done&style=shadow&taskId=ub6df3f7f-ce77-47b3-bd54-ae2f3c83c9c&title=&width=578)<br />我们可以看见，上面的图片是span标签内部一直在更新，diff算法的最小粒度是标签，所以最好把内容都写在标签里面，也就是说时间改变了，前面的It is也是跟着改变的，但是前面的输入框和Hello不会进行改变，因为根本就没变,如果输入框也改变的话输入框的内容会被清空
<a name="XYQjw"></a>

### **2.7.2. 基本原理图**

经典面试题:<br />1). react/vue中的key有什么作用？（key的内部原理是什么？）<br />2). 为什么遍历列表时，key最好不要用index?<br />1. 虚拟DOM中key的作用：<br />  1). 简单的说: key是虚拟DOM对象的标识, 在更新显示时key起着极其重要的作用<br />  2). 详细的说: 当状态中的数据发生变化时，react会根据【新数据】生成【新的虚拟DOM】, <br />                随后React进行【新虚拟DOM】与【旧虚拟DOM】的diff比较，比较规则如下：<br />      a. 旧虚拟DOM中找到了与新虚拟DOM相同的key：<br />            (1).若虚拟DOM中内容没变, 直接使用之前的真实DOM<br />            (2).若虚拟DOM中内容变了, 则生成新的真实DOM，随后替换掉页面中之前的真实DOM<br />      b. 旧虚拟DOM中未找到与新虚拟DOM相同的key<br />            根据数据创建新的真实DOM，随后渲染到到页面<br />2. 用index作为key可能会引发的问题：<br />  1. 若对数据进行：逆序添加、逆序删除等破坏顺序操作:<br />          会产生没有必要的真实DOM更新 ==> 界面效果没问题, 但效率低。<br />  2. 如果结构中还包含输入类的DOM：<br />          会产生错误DOM更新 ==> 界面有问题。<br />  3. 注意！如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，<br />            仅用于渲染列表用于展示，使用index作为key是没有问题的。<br />3. 开发中如何选择key?:<br />  1.最好使用每条数据的唯一标识作为key, 比如id、手机号、身份证号、学号等唯一值。<br />  2.如果确定只是简单的展示数据，用index也是可以的。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689060033381-f4508a74-f35f-4859-9378-aa0eaebbab73.png#averageHue=%23dfdede&clientId=uf6354842-c5be-4&from=paste&height=534&id=u7a41eaa6&originHeight=534&originWidth=1334&originalType=binary&ratio=1&rotation=0&showTitle=false&size=102973&status=done&style=shadow&taskId=ua8ed6da8-aaae-4ed1-8c2c-7ef10232c05&title=&width=1334)<br />验证

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>key的作用</title>
</head>
<body>
<div id="test"></div>
<!-- 引入react核心库 -->
<script type="text/javascript" src="../js/react.development.js"></script>
<!-- 引入react-dom -->
<script type="text/javascript" src="../js/react-dom.development.js"></script>
<!-- 引入babel -->
<script type="text/javascript" src="../js/babel.min.js"></script>

<script type="text/babel">
	
	/* 
		慢动作回放----使用index索引值作为key

			初始数据：
					{id:1,name:'小张',age:18},
					{id:2,name:'小李',age:19},
			初始的虚拟DOM：
					<li key=0>小张---18<input type="text"/></li>
					<li key=1>小李---19<input type="text"/></li>

			更新后的数据：
					{id:3,name:'小王',age:20},
					{id:1,name:'小张',age:18},
					{id:2,name:'小李',age:19},
			更新数据后的虚拟DOM：
					<li key=0>小王---20<input type="text"/></li>
					<li key=1>小张---18<input type="text"/></li>
					<li key=2>小李---19<input type="text"/></li>

	-----------------------------------------------------------------

	慢动作回放----使用id唯一标识作为key

			初始数据：
					{id:1,name:'小张',age:18},
					{id:2,name:'小李',age:19},
			初始的虚拟DOM：
					<li key=1>小张---18<input type="text"/></li>
					<li key=2>小李---19<input type="text"/></li>

			更新后的数据：
					{id:3,name:'小王',age:20},
					{id:1,name:'小张',age:18},
					{id:2,name:'小李',age:19},
			更新数据后的虚拟DOM：
					<li key=3>小王---20<input type="text"/></li>
					<li key=1>小张---18<input type="text"/></li>
					<li key=2>小李---19<input type="text"/></li>


	 */
	class Person extends React.Component{

		state = {
			persons:[
				{id:1,name:'小张',age:18},
				{id:2,name:'小李',age:19},
			]
		}

		add = ()=>{
			const {persons} = this.state
			const p = {id:persons.length+1,name:'小王',age:20}
			this.setState({persons:[p,...persons]})
		}

		render(){
			return (
				<div>
					<h2>展示人员信息</h2>
					<button onClick={this.add}>添加一个小王</button>
					<h3>使用index（索引值）作为key</h3>
					<ul>
						{
							this.state.persons.map((personObj,index)=>{
								return <li key={index}>{personObj.name}---{personObj.age}<input type="text"/></li>
							})
						}
					</ul>
					<hr/>
					<hr/>
					<h3>使用id（数据的唯一标识）作为key</h3>
					<ul>
						{
							this.state.persons.map((personObj)=>{
								return <li key={personObj.id}>{personObj.name}---{personObj.age}<input type="text"/></li>
							})
						}
					</ul>
				</div>
			)
		}
	}

	ReactDOM.render(<Person/>,document.getElementById('test'))
</script>
</body>
</html>

```

<a name="mtxqu"></a>

# 第3章：React应用(基于React脚手架)

<a name="x9qrp"></a>

## **3.1. 使用create-react-app创建react应用**

<a name="OWNvD"></a>

### **3.1.1. react脚手架**

1. xxx脚手架: 用来帮助程序员快速创建一个基于xxx库的模板项目<br />1. 包含了所有需要的配置（语法检查、jsx编译、devServer…）<br />2. 下载好了所有相关的依赖<br />3. 可以直接运行一个简单效果<br />2. react提供了一个用于创建react项目的脚手架库: create-react-app<br />3. 项目的整体技术架构为:  react + webpack + es6 + eslint<br />4. 使用脚手架开发的项目的特点: 模块化, 组件化, 工程化
   <a name="qcuMp"></a>

### **3.1.2. 创建项目并启动**

**第一步**，全局安装：npm i -g create-react-app<br />**第二步**，切换到想创项目的目录，使用命令：create-react-app hello-react<br />**第三步**，进入项目文件夹：cd hello-react<br />**第四步**，启动项目：npm start
<a name="FQhiL"></a>

### **3.1.3. react脚手架项目结构**

	public ---- 静态资源文件夹<br />		favicon.icon ------ 网站页签图标<br />		**index.html -------- 主页面**<br />		logo192.png ------- logo图<br />		logo512.png ------- logo图<br />		manifest.json ----- 应用加壳（套壳ios和Android）的配置文件<br />		robots.txt -------- 爬虫协议文件<br />src ---- 源码文件夹<br />		App.css -------- App组件的样式<br />		**App.js --------- App组件**<br />		App.test.js ---- 用于给App做测试<br />		index.css ------ 样式<br />		**index.js ------- 入口文件(区别于vue，vue的是main.js)**<br />		logo.svg ------- logo图<br />		reportWebVitals.js<br />			--- 页面性能分析文件(需要web-vitals库的支持)<br />		setupTests.js<br />			---- 组件单元测试的文件(需要jest-dom库的支持)<br />需要注意的是，组件的名称和Vue的一样，首字母大写，并且每个组件需要遵守相应书写的格式<br />编写组件的快捷键是类组件rcc。函数组件rfc

<a name="ppEWF"></a>

### **3.1.4. 功能界面的组件化编码流程（通用）**

1. 拆分组件: 拆分界面,抽取组件<br />2. 实现静态组件: 使用组件实现静态页面效果<br />3. 实现动态组件<br />3.1 动态显示初始化数据<br />3.1.1 数据类型<br />3.1.2 数据名称<br />3.1.2 保存在哪个组件?<br />3.2 交互(从绑定事件监听开始)
   <a name="SGxBS"></a>

## **3.2. 组件的组合使用-TodoList **

_功能: 组件化实现此功能_<br />_1. 显示所有todo列表_<br />_2. 输入文本, 点击按钮显示到列表的首位, 并清除输入的文本_<br />![demo2_todo list (4).gif](https://cdn.nlark.com/yuque/0/2023/gif/27086425/1689060070107-2b265dcf-fa03-468f-b828-930c58f2dd3b.gif#averageHue=%23fdfdfc&clientId=uf6354842-c5be-4&from=paste&height=493&id=u2b507557&originHeight=493&originWidth=1026&originalType=binary&ratio=1&rotation=0&showTitle=false&size=129898&status=done&style=shadow&taskId=u5c23324c-0c77-4929-ac44-3b52df5836f&title=&width=1026)<br />    1.拆分组件、实现静态组件，注意：className、style的写法<br />    2.动态初始化列表，如何确定将数据放在哪个组件的state中？<br />          ——某个组件使用：放在其自身的state中<br />          ——某些组件使用：放在他们共同的父组件state中（官方称此操作为：状态提升）<br />    3.关于父子之间通信：<br />        1.【父组件】给【子组件】传递数据：通过props传递<br />        2.【子组件】给【父组件】传递数据：通过props传递，要求父提前给子传递一个函数<br />    4.注意defaultChecked 和 checked的区别，类似的还有：defaultValue 和 value<br />    5.状态在哪里，操作状态的方法就在哪里
<a name="xhDhA"></a>

# 第4章：React ajax

<a name="OH6MK"></a>

## **4.1. 理解**

<a name="qXStJ"></a>

### **4.1.1. 前置说明**

1. React本身只关注于界面, 并不包含发送ajax请求的代码<br />2. 前端应用需要通过ajax请求与后台进行交互(json数据)<br />3. react应用中需要集成第三方ajax库(或自己封装)
   <a name="iMnm0"></a>

### **4.1.2. 常用的ajax请求库**

1. jQuery: 比较重, 如果需要另外引入不建议使用<br />2. axios: 轻量级, 建议使用<br />1) 封装XmlHttpRequest对象的ajax<br />2)  promise风格<br />3) 可以用在浏览器端和node服务器端
   <a name="0b484e39"></a>

## react脚手架配置代理总结

<a name="2eed9a76"></a>

### 方法一

> 在package.json中追加如下配置

```json
"proxy":"http://localhost:5000"
```

说明：

1. 优点：配置简单，前端请求资源时可以不加任何前缀。
2. 缺点：不能配置多个代理。
3. 工作方式：上述方式配置代理，当请求了3000不存在的资源时，那么该请求会转发给5000 （优先匹配前端资源）
4. 此外，所有的请求本来需要5000端口的数据的，现在需要给3000（自己开前端的服务端口）端口发。也相当于给自己的代理服务器发请求，让代理服务器去发请求

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689153325294-16172d7b-ceba-4d29-8088-a3148dca4920.png#averageHue=%23352c4f&clientId=uf0ceb405-b03f-4&from=paste&height=382&id=u4de0f46d&originHeight=477&originWidth=829&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=429378&status=done&style=shadow&taskId=u78cf4167-04b3-405f-992e-3a7ff050b0f&title=&width=663.2)
<a name="f3deb6e7"></a>

### 方法二

1.  第一步：创建代理配置文件 (这个配置文件会由React加载，所以名称不能变)

```
在src下创建配置文件：src/setupProxy.js
```

2.  编写setupProxy.js配置具体代理规则： 

```javascript
const proxy = require('http-proxy-middleware')//这个库react里面就有，所以我们不用单独下载了

module.exports = function(app) {
  app.use(
    proxy('/api1', {  //api1是需要转发的请求(所有带有/api1前缀的请求都会转发给5000)
      target: 'http://localhost:5000', //配置转发目标地址(能返回数据的服务器地址)
      changeOrigin: true, //控制后端服务器接收到的请求头中host字段（host字段是标识这个请求是从哪发出的）的值
      /*
       changeOrigin设置为true时，后端服务器收到的请求头中的host为：localhost:5000，欺骗一下服务器，这样会避免一些问题，哈哈哈哈
       changeOrigin设置为false时，后端服务器收到的请求头中的host为：localhost:3000
       changeOrigin默认值为false，但我们一般将changeOrigin值设为true
      */
      pathRewrite: {'^/api1': ''} //去除请求前缀，保证交给后台服务器的是正常请求地址(必须配置)
    }),
    proxy('/api2', {   
      target: 'http://localhost:5001',
      changeOrigin: true,
      pathRewrite: {'^/api2': ''}
    })
  )
}
```

说明：

1. 优点：可以配置多个代理，可以灵活的控制请求是否走代理。
2. 缺点：配置繁琐，前端请求资源时必须加前缀。
   <a name="Y3T8t"></a>

## **4.2. axios**

<a name="z2T1m"></a>

### **4.2.1. 文档**

[https://github.com/axios/axios](https://github.com/axios/axios)
<a name="m0AJq"></a>

### **4.2.2. 相关API**

GET请求

```javascript
axios.get('/user?ID=12345')
  .then(function (response) {
    console.log(response.data);
  })
  .catch(function (error) {
    console.log(error);
  });

axios.get('/user', {
  params: {
    ID: 12345
  }
})
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

```

POST请求

```javascript
axios.post('/user', {
  firstName: 'Fred',
  lastName: 'Flintstone'
})
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

<a name="DTNjg"></a>

## **4.3. 案例—github用户搜索**

<a name="bw4qW"></a>

### **4.3.1. 效果**

![demo_users (2).gif](https://cdn.nlark.com/yuque/0/2023/gif/27086425/1689060117755-ea7fdc7f-fef0-478b-b08b-d0ac755f43e7.gif#averageHue=%23f7f7f6&clientId=uf6354842-c5be-4&from=paste&height=569&id=u548e4ee5&originHeight=569&originWidth=776&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1479070&status=done&style=shadow&taskId=u032be6ac-f8e6-4acc-a052-f6cd681bd45&title=&width=776)<br />请求地址: [https://api.github.com/search/users?q=xxxxxx](https://api.github.com/search/users?q=xxxxxx)
<a name="hilnx"></a>

## **4.4. 消息订阅-发布机制**

这个东西适用于任何组件的沟通！！！<br />1. 工具库: PubSubJS<br />2. 下载:  npm install pubsub-js --save   <br />yarn add pubsub-js --save<br />3. 使用: <br />1) import PubSub from 'pubsub-js' //引入,这个引入是在组件中引入。<br />2) PubSub.subscribe('delete', function(data){ }); //订阅<br />3) PubSub.publish('delete', data) //发布消息

具体代码

```javascript
import React, { Component } from 'react'
import Search from './components/Search'
import List from './components/List'

export default class App extends Component {
	render() {
		return (
			<div className="container">
				<Search/>
				<List/>
			</div>
		)
	}
}

```

```javascript
import React, { Component } from 'react'
import PubSub from 'pubsub-js'
import axios from 'axios'

export default class Search extends Component {

	search = ()=>{
		//获取用户的输入(连续解构赋值+重命名)
		const {keyWordElement:{value:keyWord}} = this
		//发送请求前通知List更新状态
		PubSub.publish('atguigu',{isFirst:false,isLoading:true})
		//发送网络请求
		axios.get(`/api1/search/users?q=${keyWord}`).then(
			response => {
				//请求成功后通知List更新状态
				PubSub.publish('atguigu',{isLoading:false,users:response.data.items})
			},
			error => {
				//请求失败后通知App更新状态
				PubSub.publish('atguigu',{isLoading:false,err:error.message})
			}
		)
	}

	render() {
		return (
			<section className="jumbotron">
				<h3 className="jumbotron-heading">搜索github用户</h3>
				<div>
					<input ref={c => this.keyWordElement = c} type="text" placeholder="输入关键词点击搜索"/>&nbsp;
					<button onClick={this.search}>搜索</button>
				</div>
			</section>
		)
	}
}

```

```javascript
import React, { Component } from 'react'
import PubSub from 'pubsub-js'
import './index.css'

export default class List extends Component {

	state = { //初始化状态
		users:[], //users初始值为数组
		isFirst:true, //是否为第一次打开页面
		isLoading:false,//标识是否处于加载中
		err:'',//存储请求相关的错误信息
	} 

	componentDidMount(){
		this.token = PubSub.subscribe('atguigu',(_,stateObj)=>{
			this.setState(stateObj)
		})
	}

	componentWillUnmount(){
		PubSub.unsubscribe(this.token)
	}

	render() {
		const {users,isFirst,isLoading,err} = this.state
		return (
			<div className="row">
				{
					isFirst ? <h2>欢迎使用，输入关键字，随后点击搜索</h2> :
					isLoading ? <h2>Loading......</h2> :
					err ? <h2 style={{color:'red'}}>{err}</h2> :
					users.map((userObj)=>{
						return (
							<div key={userObj.id} className="card">
								<a rel="noreferrer" href={userObj.html_url} target="_blank">
									<img alt="head_portrait" src={userObj.avatar_url} style={{width:'100px'}}/>
								</a>
								<p className="card-text">{userObj.login}</p>
							</div>
						)
					})
				}
			</div>
		)
	}
}

```

<a name="Ou9VE"></a>

## **4.5. 扩展：Fetch**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689212957694-1a32d640-86fe-4b26-9677-534d442794c0.png#averageHue=%2322223d&clientId=uc25a49b6-3eac-4&from=paste&height=346&id=u56a7c77f&originHeight=433&originWidth=548&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=277869&status=done&style=shadow&taskId=u95d7115b-f6e2-46f8-9233-1b1104c3b47&title=&width=438.4)
<a name="zt1GX"></a>

### **4.5.1. 文档**

1. [https://github.github.io/fetch/](https://github.github.io/fetch/)<br />2. [https://segmentfault.com/a/1190000003810652](https://segmentfault.com/a/1190000003810652)
   <a name="QYM2o"></a>

### **4.5.2. 特点**

1. fetch: 原生函数，不再使用XmlHttpRequest对象提交ajax请求<br />2. 老版本浏览器可能不支持
   <a name="QCUGw"></a>

### **4.5.3. 相关API**

1) GET请求

```javascript
fetch(url).then(function(response) {
  //联系数据库成功
  return response.json()//这里返回的是一个promise对象
}).then(function(data) {
  //得到返回的数据
  console.log(data)
}).catch(function(e) {//统一处理错误
  console.log(e)
});
```

2) POST请求

```javascript
fetch(url, {
  method: "POST",
  body: JSON.stringify(data),
}).then(function(data) {
  console.log(data)
}).catch(function(e) {
  console.log(e)
})
```

<a name="8f066fa0"></a>

## github搜索案例相关知识点

1.设计状态时要考虑全面，例如带有网络请求的组件，要考虑请求失败怎么办。<br />2.ES6小知识点：解构赋值+重命名<br />let obj = {a:{b:1}}<br />const {a} = obj; //传统解构赋值<br />const {a:{b}} = obj; //连续解构赋值<br />const {a:{b:value}} = obj; //连续解构赋值+重命名<br />3.消息订阅与发布机制<br />	1.先订阅，再发布（理解：有一种隔空对话的感觉）<br />	2.适用于任意组件间通信<br />	3.要在组件的componentWillUnmount中取消订阅<br />	4.fetch发送请求（关注分离的设计思想）

```javascript
try {
  const response= await fetch(`/api1/search/users2?q=${keyWord}`)//连接服务器
  const data = await response.json()//得到数据
  console.log(data);
} catch (error) {//统一处理错误
  console.log('请求出错',error);
}
```

<a name="vYxev"></a>

# 第5章：React路由

<a name="x0vhe"></a>

## 5.1. 相关理解

<a name="uhjkE"></a>

### **5.1.1. SPA的理解**

1. 单页Web应用（single page web application，SPA）。<br />2. 整个应用只有**一个完整的页面**。<br />3. 点击页面中的链接**不会刷新**页面，只会做页面的**局部更新。**<br />4. 数据都需要通过ajax请求获取, 并在前端异步展现。
   <a name="MeIry"></a>

### **5.1.2. 路由的理解**

**1. 什么是路由?**<br />1. 一个路由就是一个映射关系(key:value)<br />2. key为路径, value可能是function或component<br />**2. 路由分类**<br />1. 后端路由：<br />1) 理解： value是function, 用来处理客户端提交的请求。<br />2) 注册路由：router.get(path, function(req, res))<br />3) 工作过程：当node接收到一个请求时, 根据请求路径找到匹配的路由, 调用路由中的函数来处理请求, 返回响应数据<br />2. 前端路由：<br />1) 浏览器端路由，value是component，用于展示页面内容。<br />2) 注册路由: <Route path="/test" component={Test}><br />3) 工作过程：当浏览器的path变为/test时, 当前路由组件就会变为Test组件<br />history的结构是栈的结构，如果push会压栈，回去是弹栈，如果是replace是会替换栈顶的那一条数据
<a name="pftLI"></a>

### **5.1.3. react-router-dom的理解**

1. react的一个插件库。<br />2. 专门用来实现一个SPA应用。<br />3. 基于react的项目基本都会用到此库。
   <a name="uVlzW"></a>

## **5.2. react-router-dom相关API**

这个是5版本的，后面有6版本的
<a name="jsn1A"></a>

### **5.2.1. 内置组件**

1. <BrowserRouter><br />2. <HashRouter><br />3. <Route><br />4. <Redirect><br />5. <Link><br />6. <NavLink><br />7. <Switch>
   <a name="EOoc8"></a>

### **5.2.2. 其它**

1. history对象<br />2. match对象<br />3. withRouter函数
   <a name="oMAhY"></a>

## **5.3. 基本路由使用**

<a name="cnKwl"></a>

### **5.3.1. 效果**

![react-router demo1 (3).gif](https://cdn.nlark.com/yuque/0/2023/gif/27086425/1689060149516-59719ab2-dff0-4f67-8dd6-713f118a9fa0.gif#averageHue=%23a4ccbf&clientId=uf6354842-c5be-4&from=paste&height=458&id=u69c6430f&originHeight=458&originWidth=669&originalType=binary&ratio=1&rotation=0&showTitle=false&size=105122&status=done&style=shadow&taskId=uaeab813e-7743-40fd-a05a-348ff11d94b&title=&width=669)
<a name="aPwYt"></a>

### **5.3.2. 准备**

1. 下载react-router-dom: npm install --save react-router-dom@5<br />2. 引入bootstrap.css: <link rel="stylesheet" href="/css/bootstrap.css">

<a name="CafKN"></a>

### 总结

  1.明确好界面中的导航区、展示区<br />  2.导航区的a标签改为Link标签<br />        `<Link to="/xxxxx">Demo</Link>`<br />  3.展示区写Route标签进行路径的匹配<br />       ` <Route path='/xxxx' component={Demo}/>`<br />  4.<App>的最外侧包裹了一个<BrowserRouter>或<HashRouter>

```jsx
import React, { Component } from 'react'
import {Link,Route} from 'react-router-dom'
import Home from './components/Home'
import About from './components/About'

export default class App extends Component {
  render() {
    return (
      <div>
        <div className="row">
          <div className="col-xs-offset-2 col-xs-8">
            <div className="page-header"><h2>React Router Demo</h2></div>
          </div>
        </div>
        <div className="row">
          <div className="col-xs-2 col-xs-offset-2">
            <div className="list-group">

              {/* 原生html中，靠<a>跳转不同的页面 */}
              {/* <a className="list-group-item" href="./about.html">About</a>
							<a className="list-group-item active" href="./home.html">Home</a> */}

              {/* 在React中靠路由链接实现切换组件--编写路由链接 */}
              <Link className="list-group-item" to="/about">About</Link>
              <Link className="list-group-item" to="/home">Home</Link>
            </div>
          </div>
          <div className="col-xs-6">
            <div className="panel">
              <div className="panel-body">
                {/* 注册路由 */}
                <Route path="/about" component={About}/>
                <Route path="/home" component={Home}/>
              </div>
            </div>
          </div>
        </div>
      </div>
    )
  }
}

```

```javascript
//引入react核心库
import React from 'react'
//引入ReactDOM
import ReactDOM from 'react-dom'
//这个可以换成HashRouter
import {BrowserRouter} from 'react-router-dom'
//引入App
import App from './App'

ReactDOM.render(
  // 在这里全部包括路由
  <BrowserRouter>
  <App/>
  </BrowserRouter>,
document.getElementById('root')
)
```

<a name="3ac52e85"></a>

### 路由组件与一般组件

1.写法不同：<br />	一般组件：<Demo/><br />	路由组件：<Route path="/demo" component={Demo}/><br />2.存放位置不同：<br />	一般组件：components文件夹下<br />	路由组件：pages文件夹下<br />3.接收到的props不同：<br />	一般组件：写组件标签时传递了什么，就能收到什么<br />	路由组件：程序员没有机会进行传递，由react固定给我们传递，所以会接收到三个固定的属性

```javascript
history:
      go: ƒ go(n)
      goBack: ƒ goBack()
      goForward: ƒ goForward()
      push: ƒ push(path, state)
      replace: ƒ replace(path, state)
location:
      pathname: "/about"
      search: ""
      state: undefined
match:
      params: {}
      path: "/about"
      url: "/about"
```

<a name="I9qd3"></a>

### 封装NavLink

```javascript
import React, { Component } from 'react'
import {Route} from 'react-router-dom'
import Home from './pages/Home' //Home是路由组件
import About from './pages/About' //About是路由组件
import Header from './components/Header' //Header是一般组件
import MyNavLink from './components/MyNavLink'

export default class App extends Component {
	render() {
		return (
			<div>
				<div className="row">
					<div className="col-xs-offset-2 col-xs-8">
						<Header/>
					</div>
				</div>
				<div className="row">
					<div className="col-xs-2 col-xs-offset-2">
						<div className="list-group">

							{/* 原生html中，靠<a>跳转不同的页面 */}
							{/* <a className="list-group-item" href="./about.html">About</a>
							<a className="list-group-item active" href="./home.html">Home</a> */}

							{/* 在React中靠路由链接实现切换组件--编写路由链接 */}
							{/* 标签体内的About会传入子组件的props属性中，属性名称叫children */}
							<MyNavLink to="/about">About</MyNavLink>
							<MyNavLink to="/home">Home</MyNavLink>
						</div>
					</div>
					<div className="col-xs-6">
						<div className="panel">
							<div className="panel-body">
								{/* 注册路由 */}
								<Route path="/about" component={About}/>
								<Route path="/home" component={Home}/>
							</div>
						</div>
					</div>
				</div>
			</div>
		)
	}
}

```

```javascript
import React, { Component } from 'react'
import {NavLink} from 'react-router-dom'

export default class MyNavLink extends Component {
	render() {
		// console.log(this.props);
		return (
			// <NavLink  children="Demo"/>
			// <NavLink  >Demo</NavLink>
			// 上面这两个是等价的
			// 所以下面如果props里面有children属性会直接传递到子组件的属性中
			// 下面这个会直接把父组件传递过来的属性全部接收并展开，是JavaScript的语法
			<NavLink activeClassName="atguigu" className="list-group-item" {...this.props}/>
		)
	}
}

```

NavLink 与封装 NavLink<br />    1.NavLink可以实现路由链接的高亮，通过activeClassName指定样式名
<a name="sxER8"></a>

### Switch的使用

通常状况下，path和component是一一对应的关系，但是如果我们不配置switch的话，会出现以下问题

```jsx
<div className="panel-body">
  {/* 注册路由 */}
  // 这里如果不配置Switch，如果匹配到home的话，仍然会往下去匹配，最终页面上面呈现的是Test组件
  <Route path="/about" component={About}/>
  <Route path="/home" component={Home}/>
  <Route path="/home" component={Test}/>
</div>
```

但是如果配置了Switch的话

```jsx
<div className="panel-body">
  {/* 注册路由 */}
  // 配置了Swich的话，如果匹配到了home，就不会往下匹配了，提升效率
  <Switch>
    <Route path="/about" component={About}/>
    <Route path="/home" component={Home}/>
    <Route path="/home" component={Test}/>
  </Switch>
</div>
```

<a name="HEJLC"></a>

### 解决多级路径刷新页面样式丢失的问题

1.public/index.html 中 引入样式时不写 ./ 写 / （常用）<br />2.public/index.html 中 引入样式时不写 ./ 写 %PUBLIC_URL% （常用）<br />3.使用HashRouter，因为锚点后面的就不算路径了

```jsx
<div className="panel-body">
  {/* 注册路由 */}
  // 下面这个才会出现页面样式丢失的问题，并且我们默认使用的是	<BrowserRouter>这个路由器。所以会出现这个问题
    <Route path="/haohao/about" component={About}/>
    <Route path="/haohao/home" component={Home}/>
</div>
```

shift+刷新=强制刷新

<a name="p3AFW"></a>

### 路由的严格匹配和模糊匹配

1.默认使用的是模糊匹配（简单记：【输入的路径】必须包含要【匹配的路径】，且顺序要一致）<br />2.开启严格匹配：`<Route exact={true} path="/about" component={About}/>`<br />3.严格匹配不要随便开启，需要的时候再开，有些时候开启会导致无法继续匹配二级路由
<a name="WUb1B"></a>

#### 举例

模糊匹配：给多了可以，给少了不行，默认开启这个

```jsx
<div className="row">
  <div className="col-xs-2 col-xs-offset-2">
    <div className="list-group">
      <MyNavLink to="/about">About</MyNavLink>
      <MyNavLink to="/home/a/b">Home</MyNavLink>
    </div>
  </div>
  <div className="col-xs-6">
    <div className="panel">
      <div className="panel-body">
        {/* 注册路由 */}
        <Switch>
          {/* 这里其实是模糊匹配，下面的home是可以匹配上的 */}
          <Route  path="/about" component={About}/>
          <Route  path="/home" component={Home}/>
        </Switch>
      </div>
    </div>
  </div>
</div>
```

精准匹配：主打的就是一个多了不行少了也不行，但是尽量不要用这个

```jsx
<div className="row">
  <div className="col-xs-2 col-xs-offset-2">
    <div className="list-group">
      <MyNavLink to="/about">About</MyNavLink>
      <MyNavLink to="/home/a/b">Home</MyNavLink>
    </div>
  </div>
  <div className="col-xs-6">
    <div className="panel">
      <div className="panel-body">
        {/* 注册路由 */}
        <Switch>
          {/* 这里开启了严格匹配，下面的home是匹配不到的 */}
          <Route exact path="/about" component={About}/>
          <Route exact path="/home" component={Home}/>
        </Switch>
      </div>
    </div>
  </div>
</div>
```

<a name="WO9EW"></a>

### Redirect的使用

1.一般写在所有路由注册的最下方，当所有路由都无法匹配时，跳转到Redirect指定的路由<br />2.具体编码：

```jsx
<Switch>
  <Route path="/about" component={About}/>
  <Route path="/home" component={Home}/>
  <Redirect to="/about"/>
</Switch>
```

3.完整代码：

```jsx
import React, { Component } from 'react'
// 引入Redirect
import {Route,Switch,Redirect} from 'react-router-dom'
import Home from './pages/Home' //Home是路由组件
import About from './pages/About' //About是路由组件
import Header from './components/Header' //Header是一般组件
import MyNavLink from './components/MyNavLink'

export default class App extends Component {
	render() {
		return (
      <div>
        <div className="row">
          <div className="col-xs-offset-2 col-xs-8">
            <Header />
          </div>
        </div>
        <div className="row">
          <div className="col-xs-2 col-xs-offset-2">
            <div className="list-group">
              <MyNavLink to="/about">About</MyNavLink>
              <MyNavLink to="/home">Home</MyNavLink>
            </div>
          </div>
          <div className="col-xs-6">
            <div className="panel">
              <div className="panel-body">
                {/* 注册路由 */}
                <Switch>
                  <Route path="/about" component={About} />
                  <Route path="/home" component={Home} />
                  {/* Redirect相当于一个兜底的人，如果访问的路径都匹配不上，就访问about */}
                  <Redirect to="/about" />
                </Switch>
              </div>
            </div>
          </div>
        </div>
      </div>
    );
	}
}

```

<a name="PIpu7"></a>

## **5.4. 嵌套路由使用**

注意注意！嵌套路由不能使用严格匹配，开启会无法导致二级路由

   1.注册子路由时要写上父路由的path值<br />   2.路由的匹配是按照注册路由的顺序进行的
<a name="NfAAX"></a>

### **效果**

![react-router demo2.gif](https://cdn.nlark.com/yuque/0/2023/gif/27086425/1689060156411-e9357986-754a-44d5-826b-6a575ef5879f.gif#averageHue=%2399c8b8&clientId=uf6354842-c5be-4&from=paste&height=458&id=u339f7e9b&originHeight=458&originWidth=669&originalType=binary&ratio=1&rotation=0&showTitle=false&size=127643&status=done&style=shadow&taskId=u12fc5f72-19d5-4547-8946-a5807801e64&title=&width=669)<br />**代码**<br />一级路由是/home<br />二级路由是/home/news和/home/message

```jsx
import React, { Component } from "react";
import MyNavLink from "../../components/MyNavLink";
import { Route, Switch, Redirect } from "react-router-dom";
import News from "./News";
import Message from "./Message";

export default class Home extends Component {
  render() {
    return (
      <div>
        <h3>我是Home的内容</h3>
        <div>
          <ul className="nav nav-tabs">
            {/* 需要注意的是，这里的一级路由的路径不能省略，需要跟在二级路由的前面 */}
            <li>
              <MyNavLink to="/home/news">News</MyNavLink>
            </li>
            {/* 需要注意的是，这里的一级路由的路径不能省略，需要跟在二级路由的前面 */}
            <li>
              <MyNavLink to="/home/message">Message</MyNavLink>
            </li>
          </ul>
          {/* 注册路由 */}
          <Switch>
            {/* 需要注意的是，这里的一级路由的路径不能省略，需要跟在二级路由的前面 */}
            <Route path="/home/news" component={News} />
            <Route path="/home/message" component={Message} />
            <Redirect to="/home/news" />
          </Switch>
        </div>
      </div>
    );
  }
}

```

<a name="LMZ4a"></a>

## **5.5. 向路由组件传递参数数据**

<a name="naZ0l"></a>

### 向路由组件传递params参数

1.params参数<br />      路由链接(携带参数)：<Link to='/demo/test/tom/18'}>详情</Link><br />      注册路由(声明接收)：<Route path="/demo/test/:name/:age" component={Test}/><br />      接收参数：this.props.match.params<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689243221805-6810905d-b5f3-4901-aaff-93e39e619e84.png#averageHue=%23fbf9f4&clientId=uc25a49b6-3eac-4&from=paste&height=475&id=u46cf8e4b&originHeight=594&originWidth=474&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=101067&status=done&style=shadow&taskId=ufbcad374-8cec-4b92-840e-cb12672f5fd&title=&width=379.2)<br />所以是三级路由并且根据三级路由进行传递数据

```jsx
import React, { Component } from "react";
import { Link, Route } from "react-router-dom";
import Detail from "./Detail";

export default class Message extends Component {
  state = {
    messageArr: [
      { id: "01", title: "消息1" },
      { id: "02", title: "消息2" },
      { id: "03", title: "消息3" },
    ],
  };
  render() {
    const { messageArr } = this.state;
    return (
      <div>
        <ul>
          {messageArr.map((msgObj) => {
            return (
              <li key={msgObj.id}>
                {/* 向路由组件传递params参数 */}
                <Link to={`/home/message/detail/${msgObj.id}/${msgObj.title}`}>
                  {msgObj.title}
                </Link>
              </li>
            );
          })}
        </ul>
        <hr />
        {/* 声明接收params参数 */}
        {/* 下面这个参数可以在子组件中以props的形式接收，props直接给封装好了 */}
        <Route path="/home/message/detail/:id/:title" component={Detail} />
      </div>
    );
  }
}

```

```jsx
import React, { Component } from "react";

const DetailData = [
  { id: "01", content: "你好，中国" },
  { id: "02", content: "你好，尚硅谷" },
  { id: "03", content: "你好，未来的自己" },
];
export default class Detail extends Component {
  render() {
    console.log(this.props);
    // 接收params参数，这里可以得到之前我们在url里面的参数
    const { id, title } = this.props.match.params;
    const findResult = DetailData.find((detailObj) => {
      return detailObj.id === id;
    });
    return (
      <ul>
        <li>ID:{id}</li>
        <li>TITLE:{title}</li>
        <li>CONTENT:{findResult.content}</li>
      </ul>
    );
  }
}

```

<a name="KIsOg"></a>

### 向路由组件传递search参数

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689243963591-581edf07-5a6b-4514-84e6-598dd6a30005.png#averageHue=%23fbf9f5&clientId=uc25a49b6-3eac-4&from=paste&height=426&id=u33fafc62&originHeight=532&originWidth=517&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=100885&status=done&style=shadow&taskId=u0576a789-cf0d-4602-aaff-c7c0bdfba10&title=&width=413.6)<br />路由链接(携带参数)：`<Link to='/demo/test?name=tom&age=18'}>详情</Link>`<br />注册路由(无需声明，正常注册即可)：`<Route path="/demo/test" component={Test}/>`<br />接收参数：`this.props.location.search `<br />备注：获取到的 search 是 urlencoded 编码字符串（key=value&key=value），需要借助 querystring 解析![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689243824544-6e8f8315-111b-476a-be4b-7bd91f66c4b5.png#averageHue=%23fbfaf8&clientId=uc25a49b6-3eac-4&from=paste&height=150&id=ude52790d&originHeight=187&originWidth=738&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=91903&status=done&style=shadow&taskId=uc373be53-abf6-4eef-8fcb-c77aa950b40&title=&width=590.4)<br />具体代码：

```jsx
import React, { Component } from "react";
import { Link, Route } from "react-router-dom";
import Detail from "./Detail";

export default class Message extends Component {
  state = {
    messageArr: [
      { id: "01", title: "消息1" },
      { id: "02", title: "消息2" },
      { id: "03", title: "消息3" },
    ],
  };
  render() {
    const { messageArr } = this.state;
    return (
      <div>
        <ul>
          {messageArr.map((msgObj) => {
            return (
              <li key={msgObj.id}>
                {/* 向路由组件传递search参数 */}
                {/* 这里的search参数会落在子组件的props属性的location下的search属性下，所以叫search属性不叫query属性 */}
                <Link
                  to={`/home/message/detail/?id=${msgObj.id}&title=${msgObj.title}`}
                >
                  {msgObj.title}
                </Link>
              </li>
            );
          })}
        </ul>
        <hr />
        {/* search参数无需声明接收，正常注册路由即可 */}
        <Route path="/home/message/detail" component={Detail} />
      </div>
    );
  }
}

```

```jsx
import React, { Component } from "react";
import qs from "querystring";

const DetailData = [
  { id: "01", content: "你好，中国" },
  { id: "02", content: "你好，尚硅谷" },
  { id: "03", content: "你好，未来的自己" },
];
export default class Detail extends Component {
  render() {
    console.log(this.props);
    // 接收search参数
    const { search } = this.props.location;
    // 截取问号
    const { id, title } = qs.parse(search.slice(1));
    const findResult = DetailData.find((detailObj) => {
      return detailObj.id === id;
    });
    return (
      <ul>
        <li>ID:{id}</li>
        <li>TITLE:{title}</li>
        <li>CONTENT:{findResult.content}</li>
      </ul>
    );
  }
}

```

<a name="nEC6U"></a>

### 向路由组件传递state参数

这个state和组件的state不一样<br />**这个state是存储在history的，这说明我们如果清理历史记录的话，再刷新页面就看不到这个state了  **<br />**之前那两个就算是清除历史记录数据也不会丢，因为又通过url进行传过去了**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689244921534-8e8f0304-42b3-4ce4-8a97-de1039d29496.png#averageHue=%23fbf8f3&clientId=uc25a49b6-3eac-4&from=paste&height=434&id=u8ea20490&originHeight=542&originWidth=514&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=97661&status=done&style=shadow&taskId=u14aeca3d-911c-433a-bf65-e1d1d41d07e&title=&width=411.2)

- 路由链接(携带参数)：`<Link to={{pathname:'/demo/test',state:{name:'tom',age:18}}}>详情</Link>`
- 注册路由(无需声明，正常注册即可)：`<Route path="/demo/test" component={Test}/>`
- 接收参数：`this.props.location.state`
- 备注：刷新也可以保留住参数，但是清楚历史就保不住了

具体代码：

```jsx
import React, { Component } from "react";
import { Link, Route } from "react-router-dom";
import Detail from "./Detail";

export default class Message extends Component {
  state = {
    messageArr: [
      { id: "01", title: "消息1" },
      { id: "02", title: "消息2" },
      { id: "03", title: "消息3" },
    ],
  };
  render() {
    const { messageArr } = this.state;
    return (
      <div>
        <ul>
          {messageArr.map((msgObj) => {
            return (
              <li key={msgObj.id}>
                {/* 向路由组件传递state参数 */}
                {/* 这里通过对象进行传输数据，state可以在子组件的location中的state中获取 
                pathname之前没写是因为他是默认属性，spring里面也有这种情况*/}
                <Link
                  to={{
                    pathname: "/home/message/detail",
                    state: { id: msgObj.id, title: msgObj.title },
                  }}
                >
                  {msgObj.title}
                </Link>
              </li>
            );
          })}
        </ul>
        <hr />
        {/* state参数无需声明接收，正常注册路由即可 */}
        <Route path="/home/message/detail" component={Detail} />
      </div>
    );
  }
}

```

```jsx
import React, { Component } from 'react'
// import qs from 'querystring'

const DetailData = [
	{id:'01',content:'你好，中国'},
	{id:'02',content:'你好，尚硅谷'},
	{id:'03',content:'你好，未来的自己'}
]
export default class Detail extends Component {
	render() {
		console.log(this.props);
		// 接收state参数,这里||{}空对象是为了如果清楚历史后刷新这里会为空
		const {id,title} = this.props.location.state || {}

    //这里||{}空对象是为了如果清楚历史后刷新这里会为空
		const findResult = DetailData.find((detailObj)=>{
			return detailObj.id === id
		}) || {}
		return (
			<ul>
				<li>ID:{id}</li>
				<li>TITLE:{title}</li>
				<li>CONTENT:{findResult.content}</li>
			</ul>
		)
	}
}

```

<a name="bB7cH"></a>

## **5.6. 多种路由跳转方式**

<a name="F3yNb"></a>

### push和replace

开启replace模式，替换push模式<br />`<MyNavLink replace to="/about">About</MyNavLink>`
<a name="OmuB2"></a>

### **编程式导航**

借助 this.prosp.history 对象上的 API 对操作路由跳转、前进、后退（但是这也意味着非路由组件就用不了这个API了）

- -this.prosp.history.push()
- -this.prosp.history.replace()
- -this.prosp.history.goBack()
- -this.prosp.history.goForward()
- -this.prosp.history.go()

![react-router demo4.gif](https://cdn.nlark.com/yuque/0/2023/gif/27086425/1689060170904-876f0cb6-6047-4cbb-84ef-2e55ba70668e.gif#averageHue=%23fbfbfa&clientId=uf6354842-c5be-4&from=paste&height=540&id=u088ba895&originHeight=540&originWidth=814&originalType=binary&ratio=1&rotation=0&showTitle=false&size=407491&status=done&style=shadow&taskId=uda15a94b-db5f-4cc3-bdd2-b6a1dad9d21&title=&width=814)

```jsx
import React, { Component } from 'react'
import {Link,Route} from 'react-router-dom'
import Detail from './Detail'

export default class Message extends Component {
	state = {
		messageArr:[
			{id:'01',title:'消息1'},
			{id:'02',title:'消息2'},
			{id:'03',title:'消息3'},
		]
	}

	replaceShow = (id,title)=>{
		//replace跳转+携带params参数
		//this.props.history.replace(`/home/message/detail/${id}/${title}`)

		//replace跳转+携带search参数
		// this.props.history.replace(`/home/message/detail?id=${id}&title=${title}`)

		//replace跳转+携带state参数
		this.props.history.replace(`/home/message/detail`,{id,title})
	}

	pushShow = (id,title)=>{
		//push跳转+携带params参数
		// this.props.history.push(`/home/message/detail/${id}/${title}`)

		//push跳转+携带search参数
		// this.props.history.push(`/home/message/detail?id=${id}&title=${title}`)

		//push跳转+携带state参数
		this.props.history.push(`/home/message/detail`,{id,title})
		
	}

	back = ()=>{
		this.props.history.goBack()
	}

	forward = ()=>{
		this.props.history.goForward()
	}

	go = ()=>{
		this.props.history.go(-2)
	}

	render() {
		const {messageArr} = this.state
		return (
			<div>
				<ul>
					{
						messageArr.map((msgObj)=>{
							return (
								<li key={msgObj.id}>

									{/* 向路由组件传递params参数 */}
									{/* <Link to={`/home/message/detail/${msgObj.id}/${msgObj.title}`}>{msgObj.title}</Link> */}

									{/* 向路由组件传递search参数 */}
									{/* <Link to={`/home/message/detail/?id=${msgObj.id}&title=${msgObj.title}`}>{msgObj.title}</Link> */}

									{/* 向路由组件传递state参数 */}
									<Link to={{pathname:'/home/message/detail',state:{id:msgObj.id,title:msgObj.title}}}>{msgObj.title}</Link>

									&nbsp;<button onClick={()=> this.pushShow(msgObj.id,msgObj.title)}>push查看</button>
									&nbsp;<button onClick={()=> this.replaceShow(msgObj.id,msgObj.title)}>replace查看</button>
								</li>
							)
						})
					}
				</ul>
				<hr/>
				{/* 声明接收params参数 */}
				{/* <Route path="/home/message/detail/:id/:title" component={Detail}/> */}

				{/* search参数无需声明接收，正常注册路由即可 */}
				{/* <Route path="/home/message/detail" component={Detail}/> */}

				{/* state参数无需声明接收，正常注册路由即可 */}
				<Route path="/home/message/detail" component={Detail}/>

				<button onClick={this.back}>回退</button>&nbsp;
				<button onClick={this.forward}>前进</button>&nbsp;
				<button onClick={this.go}>go</button>

			</div>
		)
	}
}

```

<a name="DVaxu"></a>

### WithRouter

withRouter就可以使一般组件用上路由组件的API<br />withRouter可以加工一般组件，让一般组件具备路由组件所特有的API<br />withRouter的返回值是一个新组件，里面有路由组件中含有的props属性。之后就可以像路由组件一样操作了

```jsx
import React, { Component } from "react";
// 引入withRouter
import { withRouter } from "react-router-dom";

class Header extends Component {
  back = () => {
    this.props.history.goBack();
  };

  forward = () => {
    this.props.history.goForward();
  };

  go = () => {
    this.props.history.go(-2);
  };

  render() {
    console.log("Header组件收到的props是", this.props);
    return (
      <div className="page-header">
        <h2>React Router Demo</h2>
        <button onClick={this.back}>回退</button>&nbsp;
        <button onClick={this.forward}>前进</button>&nbsp;
        <button onClick={this.go}>go</button>
      </div>
    );
  }
}

//withRouter可以加工一般组件，让一般组件具备路由组件所特有的API
//withRouter的返回值是一个新组件
export default withRouter(Header);

```

<a name="CKN8g"></a>

### BrowserRouter 与 HashRouter 的区别

  1.底层原理不一样：<br />        BrowserRouter使用的是H5的history API，不兼容IE9及以下版本。<br />        HashRouter使用的是URL的哈希值。<br />  2.path表现形式不一样<br />        BrowserRouter的路径中没有#,例如：localhost:3000/demo/test<br />        HashRouter的路径包含#,例如：localhost:3000/#/demo/test<br />  3.刷新后对路由state参数的影响<br />        (1).BrowserRouter没有任何影响，因为state保存在history对象中。<br />        (2).HashRouter刷新后会导致路由state参数的丢失！！！，但是如果不刷新只使用的话没有问题<br />  4.备注：HashRouter可以用于解决一些路径错误相关的问题。
<a name="yZ6if"></a>

# 第6章：React UI组件库

<a name="Kg2uD"></a>

## **6.1.流行的开源React UI组件库**

<a name="Tocah"></a>

### **6.1.1. material-ui(国外)**

1. 官网: [http://www.material-ui.com/#/](#/)<br />2. github: [https://github.com/callemall/material-ui](https://github.com/callemall/material-ui)
   <a name="YChhb"></a>

### **6.1.2. ant-design(国内蚂蚁金服)**

1. 官网: [https://ant.design/index-cn](https://ant.design/index-cn)<br />2. Github: [https://github.com/ant-design/ant-design/](https://github.com/ant-design/ant-design/)

<a name="zPT2y"></a>

## antd 的按需引入+自定义主题

1.安装依赖：yarn add react-app-rewired customize-cra babel-plugin-import less less-loader<br />2.修改package.json

```jsx
"scripts": {
  "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    "eject": "react-scripts eject"
},
```

3.根目录下创建config-overrides.js

```jsx
//配置具体的修改规则
const { override, fixBabelImports,addLessLoader} = require('customize-cra');
module.exports = override(
  fixBabelImports('import', {
    libraryName: 'antd',
    libraryDirectory: 'es',
    style: true,
  }),
  // 自定义主题色
  addLessLoader({
    lessOptions:{
      javascriptEnabled: true,
      modifyVars: { '@primary-color': 'green' },
    }
  }),
);
```

4.备注：不用在组件里亲自引入样式了，即：import 'antd/dist/antd.css'应该删掉<br />但是这些推荐查文档，但是文档可能会有些落后，到时候就只能查看lessLoader的文档了
<a name="Ne8nk"></a>

# 第7章：redux

其实对标的就是Vue的vuex和pinia
<a name="itLc7"></a>

## **7.1. redux理解**

<a name="HKBX5"></a>

### **7.1.1. 学习文档**

1. 英文文档: [https://redux.js.org/](https://redux.js.org/)<br />2. 中文文档: [http://www.redux.org.cn/](http://www.redux.org.cn/)<br />3. Github: [https://github.com/reactjs/redux](https://github.com/reactjs/redux)
   <a name="NXHOS"></a>

### **7.1.2. redux是什么**

1. redux是一个专门用于做**状态管理**的JS库(不是react插件库)。<br />2. 它可以用在react, angular, vue等项目中, 但基本与react配合使用。<br />3. 作用: 集中式管理react应用中多个组件**共享**的状态。
   <a name="tjgDu"></a>

### **7.1.3. 什么情况下需要使用redux**

1. 某个组件的状态，需要让其他组件可以随时拿到（共享）。<br />2. 一个组件需要改变另一个组件的状态（通信）。<br />3. 总体原则：能不用就不用, 如果不用比较吃力才考虑使用。
   <a name="h0HcK"></a>

### **7.1.4. redux工作流程**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689257737341-4fe236c4-375e-4c46-a135-c60c18621229.png#averageHue=%23f7f3ec&clientId=uc25a49b6-3eac-4&from=paste&height=604&id=u3420433f&originHeight=755&originWidth=1341&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=224770&status=done&style=shadow&taskId=u63a38075-e8f0-4ad0-9137-795ee82617f&title=&width=1072.8)
<a name="fpNJS"></a>

## **7.2. redux的三个核心概念**

<a name="y9vqQ"></a>

### **7.2.1. action**

1. 动作的对象<br />2. 包含2个属性

-  type：标识属性, 值为字符串, 唯一, 必要属性
-  data：数据属性, 值类型任意, 可选属性

3. 例子：{ type: 'ADD_STUDENT',data:{name: 'tom',age:18} }
   <a name="OAH0C"></a>

### **7.2.2. reducer **

1. 用于初始化状态、加工状态。<br />2. 加工时，根据旧的state和action， 产生新的state的**纯函数。**
   <a name="iWIU3"></a>

### **7.2.3. store**

1. 将state、action、reducer联系在一起的对象<br />2. 如何得到此对象?<br />1) import {createStore} from 'redux'<br />2) import reducer from './reducers'<br />3) const store = createStore(reducer)<br />3. 此对象的功能?<br />1) getState(): 得到state<br />2) dispatch(action): 分发action, 触发reducer调用, 产生新的state<br />3) subscribe(listener): 注册监听, 当产生了新的state时, 自动调用
   <a name="bZXxA"></a>

## **7.3. redux的核心API**

<a name="w7Xmh"></a>

### **7.3.1. createstore()**

作用：创建包含指定reducer的store对象
<a name="sAlTX"></a>

### **7.3.2. store对象**

1. 作用: redux库最核心的管理对象<br />2. 它内部维护着:<br />1) state<br />2) reducer<br />3. 核心方法:<br />1) getState()<br />2) dispatch(action)<br />3) subscribe(listener)<br />4. 具体编码:<br />1) store.getState()<br />2) store.dispatch({type:'INCREMENT', number})<br />3) store.subscribe(render)
   <a name="UQLQA"></a>

### **7.3.3. applyMiddleware()**

作用：应用上基于redux的中间件(插件库)
<a name="tFbiN"></a>

### **7.3.4. combineReducers()**

作用：合并多个reducer函数
<a name="BA7NO"></a>

## **7.4. 使用redux编写应用**

**	效果**<br />![redux.gif](https://cdn.nlark.com/yuque/0/2023/gif/27086425/1689060227376-751cdeee-55f4-421c-a838-65dd7bbc408e.gif#averageHue=%23fafaf9&clientId=uf6354842-c5be-4&from=paste&height=259&id=u883a71e1&originHeight=259&originWidth=368&originalType=binary&ratio=1&rotation=0&showTitle=false&size=66249&status=done&style=shadow&taskId=uc30d1c31-1085-46c5-adc3-4bbc054807a&title=&width=368)
<a name="SFQlQ"></a>

### 精简版

(1).去除Count组件自身的状态<br />(2).src下建立:<br />        -redux<br />          -store.js<br />          -count_reducer.js<br />(3).store.js：<br />      1).引入redux中的createStore函数，创建一个store<br />      2).createStore调用时要传入一个为其服务的reducer<br />      3).记得暴露store对象<br />(4).count_reducer.js：<br />      1).reducer的本质是一个函数，接收：preState,action，返回加工后的状态<br />      2).reducer有两个作用：初始化状态，加工状态<br />      3).reducer被第一次调用时，是store自动触发的，<br />              传递的preState是undefined,<br />              传递的action是:{type:'@@REDUX/INIT_a.2.b.4}<br />(5).在index.js中监测store中状态的改变，一旦发生改变重新渲染<App/><br />    备注：redux只负责管理状态，至于状态的改变驱动着页面的展示，要靠我们自己写。<br />目录结构<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689296654499-3c510752-10a3-463a-97a0-97f3d589b9ff.png#averageHue=%2324211c&clientId=ueb9972e6-a3a0-4&from=paste&height=150&id=u3d297283&originHeight=187&originWidth=244&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10012&status=done&style=shadow&taskId=u076b3c76-c70a-4d40-9f5c-5a9a86ae330&title=&width=195.2)

```jsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import store from "./redux/store";

ReactDOM.render(<App />, document.getElementById("root"));

// 可以直接写到这里
// 这里的话效率也不会太低，因为有diff算法
store.subscribe(() => {
  ReactDOM.render(<App />, document.getElementById("root"));
});

```

```jsx
import React, { Component } from 'react'
import Count from './components/Count'

export default class App extends Component {
	render() {
		return (
			<div>
				<Count/>
			</div>
		)
	}
}

```

```jsx
/* 
	该文件专门用于暴露一个store对象，整个应用只有一个store对象
*/

//引入createStore，专门用于创建redux中最为核心的store对象
import { createStore } from "redux";
//引入为Count组件服务的reducer
import countReducer from "./count_reducer";
//暴露store
export default createStore(countReducer);

```

```jsx
/* 
	1.该文件是用于创建一个为Count组件服务的reducer，reducer的本质就是一个函数
	2.reducer函数会接到两个参数，分别为：之前的状态(preState)，动作对象(action)
*/

const initState = 0; //初始化状态
export default function countReducer(preState = initState, action) {
  // console.log(preState);
  //从action对象中获取：type、data
  const { type, data } = action;
  //根据type决定如何加工数据
  switch (type) {
    case "increment": //如果是加
      return preState + data;
    case "decrement": //若果是减
      return preState - data;
    default:
      return preState;
  }
}

```

```jsx
import React, { Component } from "react";
//引入store，用于获取redux中的state
import store from "../../redux/store";

export default class Count extends Component {
  // 这里想表达的意思是不被redux管理的state，可以放在组件里面
  state = { carName: "奔驰c63" };

  /* componentDidMount(){
		//检测redux中状态的变化，只要变化，就调用render
    //其实就是虚晃render一下，更新state会调用render
    // 但是在这里写的话我们需要在每个用到redux的组件里面都得写，所以为了精简，可以直接写到外侧的index.js下。
		store.subscribe(()=>{
			this.setState({})
		})
	} */

  //加法
  increment = () => {
    const { value } = this.selectNumber;
    store.dispatch({ type: "increment", data: value * 1 });
  };
  //减法
  decrement = () => {
    const { value } = this.selectNumber;
    store.dispatch({ type: "decrement", data: value * 1 });
  };
  //奇数再加
  incrementIfOdd = () => {
    const { value } = this.selectNumber;
    const count = store.getState();
    if (count % 2 !== 0) {
      store.dispatch({ type: "increment", data: value * 1 });
    }
  };
  //异步加
  incrementAsync = () => {
    const { value } = this.selectNumber;
    setTimeout(() => {
      store.dispatch({ type: "increment", data: value * 1 });
    }, 500);
  };

  render() {
    return (
      <div>
        <h1>当前求和为：{store.getState()}</h1>
        <select ref={(c) => (this.selectNumber = c)}>
          <option value="1">1</option>
          <option value="2">2</option>
          <option value="3">3</option>
        </select>
        &nbsp;
        <button onClick={this.increment}>+</button>&nbsp;
        <button onClick={this.decrement}>-</button>&nbsp;
        <button onClick={this.incrementIfOdd}>当前求和为奇数再加</button>&nbsp;
        <button onClick={this.incrementAsync}>异步加</button>&nbsp;
      </div>
    );
  }
}

```

<a name="l6DCS"></a>

### 完整版

新增文件：<br />  1.count_action.js 专门用于创建action对象<br />  2.constant.js 放置容易写错的type值<br />文件结构<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689297550996-73d7aa3e-7ecb-4f8e-95ba-18520e81b596.png#averageHue=%2323201c&clientId=ueb9972e6-a3a0-4&from=paste&height=175&id=uf9ca68f7&originHeight=219&originWidth=268&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10562&status=done&style=shadow&taskId=uaa0332a9-c26b-4867-8782-0ed638c7652&title=&width=214.4)

```jsx
/* 
	该模块是常量模块，用于定义常量
	该模块是用于定义action对象中type类型的常量值，目的只有一个：便于管理的同时防止程序员单词写错
*/
export const INCREMENT = 'increment'
export const DECREMENT = 'decrement'
```

```jsx
/* 
	该文件专门为Count组件生成action对象
*/
import { INCREMENT, DECREMENT } from "./constant";

// 这里的箭头函数里面返回的对象是一个对象！！
// 这个对象是{type:INCREMENT,data}
export const createIncrementAction = (data) => ({ type: INCREMENT, data });
export const createDecrementAction = (data) => ({ type: DECREMENT, data });

```

```jsx
import React, { Component } from "react";
import store from "../../redux/store";
//引入actionCreator，专门用于创建action对象
import {
  createIncrementAction,
  createDecrementAction,
} from "../../redux/count_action";

export default class Count extends Component {

  //加法
  increment = () => {
    const { value } = this.selectNumber;
    // 这里通过action进行包装对象
    store.dispatch(createIncrementAction(value * 1));
  };
  //减法
  decrement = () => {
    const { value } = this.selectNumber;
    store.dispatch(createDecrementAction(value * 1));
  };
  //奇数再加
  incrementIfOdd = () => {
    const { value } = this.selectNumber;
    const count = store.getState();
    if (count % 2 !== 0) {
      store.dispatch(createIncrementAction(value * 1));
    }
  };
  //异步加
  incrementAsync = () => {
    const { value } = this.selectNumber;
    setTimeout(() => {
      store.dispatch(createIncrementAction(value * 1));
    }, 500);
  };

  render() {
    return (
      <div>
        <h1>当前求和为：{store.getState()}</h1>
        <select ref={(c) => (this.selectNumber = c)}>
          <option value="1">1</option>
          <option value="2">2</option>
          <option value="3">3</option>
        </select>
        &nbsp;
        <button onClick={this.increment}>+</button>&nbsp;
        <button onClick={this.decrement}>-</button>&nbsp;
        <button onClick={this.incrementIfOdd}>当前求和为奇数再加</button>&nbsp;
        <button onClick={this.incrementAsync}>异步加</button>&nbsp;
      </div>
    );
  }
}

```

其他的无变化
<a name="uBZNz"></a>

### 异步Action版

(1).明确：延迟的动作不想交给组件自身，想交给action<br />(2).何时需要异步action：想要对状态进行操作，但是具体的数据靠异步任务返回。<br />(3).具体编码：<br />    1).yarn add redux-thunk，并配置在store中<br />    2).创建action的函数不再返回一般对象，而是一个函数，该函数中写异步任务。<br />    3).异步任务有结果后，分发一个同步的action去真正操作数据。<br />(4).备注：异步action不是必须要写的，完全可以自己等待异步任务的结果了再去分发同步action。

```jsx
/* 
	该文件专门用于暴露一个store对象，整个应用只有一个store对象
*/

//引入createStore，专门用于创建redux中最为核心的store对象
import { createStore, applyMiddleware } from "redux";
//引入为Count组件服务的reducer
import countReducer from "./count_reducer";
//引入redux-thunk，用于支持异步action
import thunk from "redux-thunk";
//暴露store，创建Store的时候引入中间件
export default createStore(countReducer, applyMiddleware(thunk));

```

```jsx
/* 
	该文件专门为Count组件生成action对象
*/
import { INCREMENT, DECREMENT } from "./constant";

//同步action，就是指action的值为Object类型的一般对象
export const createIncrementAction = (data) => ({ type: INCREMENT, data });
export const createDecrementAction = (data) => ({ type: DECREMENT, data });

//异步action，就是指action的值为函数,异步action中一般都会调用同步action，异步action不是必须要用的。
export const createIncrementAsyncAction = (data, time) => {
  // 这个函数返回的是一个函数，参数react会自动帮我们封装进去
  return (dispatch) => {
    setTimeout(() => {
      dispatch(createIncrementAction(data));
    }, time);
  };
};

```

<a name="IWe8G"></a>

## **7.5. redux异步编程**

上面的就是实例！
<a name="hPoiI"></a>

### **7.5.1理解：**

1. redux默认是不能进行异步处理的, <br />2. 某些时候应用中需要在**redux中执行异步任务**(ajax, 定时器)
   <a name="z4PdY"></a>

### **7.5.2. 使用异步中间件**

npm install --save redux-thunk
<a name="jDiKG"></a>

## **7.6. react-redux**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689301965288-d84ce086-2afa-4c06-8705-e48584e1ab82.png#averageHue=%23f4f2f1&clientId=ueb9972e6-a3a0-4&from=paste&height=495&id=u61ac0e06&originHeight=619&originWidth=1114&originalType=binary&ratio=1&rotation=0&showTitle=false&size=237597&status=done&style=shadow&taskId=ue3738544-812d-4ec0-a2f4-c6de289d132&title=&width=891.2)
<a name="xeyOV"></a>

### **7.6.1. 理解**

1. 一个react插件库<br />2. 专门用来简化react应用中使用redux
   <a name="KcJHB"></a>

### **7.6.2. react-Redux将所有组件分成两大类**

1. UI组件<br />1) 只负责 UI 的呈现，不带有任何业务逻辑<br />2) 通过props接收数据(一般数据和函数)<br />3) 不使用任何 Redux 的 API<br />4) 一般保存在components文件夹下<br />2. 容器组件<br />1) 负责管理数据和业务逻辑，不负责UI的呈现<br />2) 负责使用 Redux 的 API，和Redux进行通信<br />3) 一般保存在containers文件夹下<br />4)这个组件通过connect()()方法进行生成
   <a name="SOo0p"></a>

### **7.6.3. 相关API**

1. Provider：让所有组件都可以得到state数据<br />2. connect：用于包装UI 组件生成容器组件<br />3. mapStateToprops：将外部的数据（即state对象）转换为UI组件的标签属性<br />4. mapDispatchToProps：将分发action的函数转换为UI组件的标签属性



(1).明确两个概念：<br />      1).UI组件:不能使用任何redux的api，只负责页面的呈现、交互等。<br />      2).容器组件：负责和redux通信，将结果交给UI组件。<br />(2).如何创建一个容器组件————靠react-redux 的 connect函数<br />        connect(mapStateToProps,mapDispatchToProps)(UI组件)<br />          -mapStateToProps:映射状态，返回值是一个对象<br />          -mapDispatchToProps:映射操作状态的方法，返回值是一个对象<br />(3).备注1：容器组件中的store是靠props传进去的，而不是在容器组件中直接引入<br />(4).备注2：mapDispatchToProps，也可以是一个对象

```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'
import store from './redux/store'

ReactDOM.render(<App/>,document.getElementById('root'))

//监测redux中状态的改变，如redux的状态发生了改变，那么重新渲染App组件
// 但是我们使用了react-redux，里面的connect就帮我们进行检测然后重新渲染了，我们就不用操心这个了
// store.subscribe(()=>{
// 	ReactDOM.render(<App/>,document.getElementById('root'))
// })
```

```javascript
import React, { Component } from "react";
import Count from "./containers/Count";
import store from "./redux/store";

export default class App extends Component {
  render() {
    return (
      <div>
        {/* 
          给容器组件传递store 
          这样可以方便 容器组件 得到store中的state和dispatch
        */}
        <Count store={store} />
      </div>
    );
  }
}

```

```javascript
//引入Count的UI组件
import CountUI from "../../components/Count";
//引入action
import {
  createIncrementAction,
  createDecrementAction,
  createIncrementAsyncAction,
} from "../../redux/count_action";

//引入connect用于连接UI组件与redux
import { connect } from "react-redux";

//mapStateToProps和mapDispatchToProps这两个函数的函数名称可以随便起，
//但是为了命名规范我们还是起成这两个名字就好啦

/* 
	1.mapStateToProps函数返回的是一个对象；
	2.返回的对象中的key就作为传递给UI组件props的key,value就作为传递给UI组件props的value
	3.mapStateToProps用于传递状态
  至于参数的state是react-redux给我们自动调用的store.getstate()方法，然后帮我们封装进参数里了
  至于store是哪里来的，在App.jsx里面，通过props自动给我们传递过来了。
*/
function mapStateToProps(state) {
  return { count: state };
}

/* 
	1.mapDispatchToProps函数返回的是一个对象；
	2.返回的对象中的key就作为传递给UI组件props的key,value就作为传递给UI组件props的value
	3.mapDispatchToProps用于传递操作状态的方法
  至于dispatch这个参数，这个是react-redux给我们自动传入的，不用关系，直接用就可以了
*/
function mapDispatchToProps(dispatch) {
  return {
    jia: (number) => dispatch(createIncrementAction(number)),
    jian: (number) => dispatch(createDecrementAction(number)),
    jiaAsync: (number, time) =>
      dispatch(createIncrementAsyncAction(number, time)),
  };
}

//使用connect()()连接UI组件，并且向UI组件中传入state和操作state的方法。
//并暴露一个Count的容器组件
export default connect(mapStateToProps, mapDispatchToProps)(CountUI);

```

```javascript
import React, { Component } from 'react'

export default class Count extends Component {

	state = {carName:'奔驰c63'}

	//加法
	increment = ()=>{
		const {value} = this.selectNumber
		this.props.jia(value*1)
	}
	//减法
	decrement = ()=>{
		const {value} = this.selectNumber
		this.props.jian(value*1)
	}
	//奇数再加
	incrementIfOdd = ()=>{
		const {value} = this.selectNumber
		if(this.props.count % 2 !== 0){
			this.props.jia(value*1)
		}
	}
	//异步加
	incrementAsync = ()=>{
		const {value} = this.selectNumber
		this.props.jiaAsync(value*1,500)
	}

	render() {
		//console.log('UI组件接收到的props是',this.props);
		return (
			<div>
				<h1>当前求和为：{this.props.count}</h1>
				<select ref={c => this.selectNumber = c}>
					<option value="1">1</option>
					<option value="2">2</option>
					<option value="3">3</option>
				</select>&nbsp;
				<button onClick={this.increment}>+</button>&nbsp;
				<button onClick={this.decrement}>-</button>&nbsp;
				<button onClick={this.incrementIfOdd}>当前求和为奇数再加</button>&nbsp;
				<button onClick={this.incrementAsync}>异步加</button>&nbsp;
			</div>
		)
	}
}

```

<a name="E0ziK"></a>

### 优化版

(1).容器组件和UI组件整合一个文件<br />(2).无需自己给容器组件传递store，给<App/>包裹一个<Provider store={store}>即可。<br />(3).使用了react-redux后也不用再自己检测redux中状态的改变了，容器组件可以自动完成这个工作。<br />(4).mapDispatchToProps也可以简单的写成一个对象<br />(5).一个组件要和redux“打交道”要经过哪几步？<br />        (1).定义好UI组件---不暴露<br />        (2).引入connect生成一个容器组件，并暴露，写法如下：<br />            connect(<br />              state => ({key:value}), //映射状态<br />              {key:xxxxxAction} //映射操作状态的方法<br />            )(UI组件)<br />        (4).在UI组件中通过this.props.xxxxxxx读取和操作状态

```javascript
import React, { Component } from 'react'
//引入action
import {
	createIncrementAction,
	createDecrementAction,
	createIncrementAsyncAction
} from '../../redux/count_action'
//引入connect用于连接UI组件与redux
import {connect} from 'react-redux'

//定义UI组件
class Count extends Component {

	state = {carName:'奔驰c63'}

	//加法
	increment = ()=>{
		const {value} = this.selectNumber
		this.props.jia(value*1)
	}
	//减法
	decrement = ()=>{
		const {value} = this.selectNumber
		this.props.jian(value*1)
	}
	//奇数再加
	incrementIfOdd = ()=>{
		const {value} = this.selectNumber
		if(this.props.count % 2 !== 0){
			this.props.jia(value*1)
		}
	}
	//异步加
	incrementAsync = ()=>{
		const {value} = this.selectNumber
		this.props.jiaAsync(value*1,500)
	}

	render() {
		//console.log('UI组件接收到的props是',this.props);
		return (
			<div>
				<h1>当前求和为：{this.props.count}</h1>
				<select ref={c => this.selectNumber = c}>
					<option value="1">1</option>
					<option value="2">2</option>
					<option value="3">3</option>
				</select>&nbsp;
				<button onClick={this.increment}>+</button>&nbsp;
				<button onClick={this.decrement}>-</button>&nbsp;
				<button onClick={this.incrementIfOdd}>当前求和为奇数再加</button>&nbsp;
				<button onClick={this.incrementAsync}>异步加</button>&nbsp;
			</div>
		)
	}
}

//使用connect()()创建并暴露一个Count的容器组件
export default connect(
	state => ({count:state}),

	//mapDispatchToProps的一般写法
	/* dispatch => ({
		jia:number => dispatch(createIncrementAction(number)),
		jian:number => dispatch(createDecrementAction(number)),
		jiaAsync:(number,time) => dispatch(createIncrementAsyncAction(number,time)),
	}) */

	//mapDispatchToProps的简写
	{
		jia:createIncrementAction,
		jian:createDecrementAction,
		jiaAsync:createIncrementAsyncAction,
	}
)(Count)


```

```javascript
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import store from "./redux/store";
import { Provider } from "react-redux";

ReactDOM.render(
  // 通过Provider进行自动传输store
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);

```

<a name="ghSQk"></a>

### 数据共享版

目录结构<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689317489041-f4f7de31-2790-4e06-b09f-576b9e2dc967.png#averageHue=%2322201c&clientId=ueb9972e6-a3a0-4&from=paste&height=366&id=u1ea3527a&originHeight=457&originWidth=263&originalType=binary&ratio=1&rotation=0&showTitle=false&size=20432&status=done&style=shadow&taskId=ub7cc12f2-9f4f-46ae-ad0f-2f55c614e92&title=&width=210.4)<br />相关代码

```javascript
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import store from "./redux/store";
import { Provider } from "react-redux";

ReactDOM.render(
  /* 此处需要用Provider包裹App，目的是让App所有的后代容器组件都能接收到store */
  // 仍然是把store传入进去，store里面有person和count里面的数据
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);

```

```javascript
import React, { Component } from "react";
import Count from "./containers/Count";
import Person from "./containers/Person";

export default class App extends Component {
  render() {
    return (
      <div>
        {/* 可以在这里引入多个组件 */}
        <Count />
        <hr />
        <Person />
      </div>
    );
  }
}

```

```javascript
/* 
	该文件专门为Count组件生成action对象
*/
import {INCREMENT,DECREMENT} from '../constant'

//同步action，就是指action的值为Object类型的一般对象
export const createIncrementAction = data => ({type:INCREMENT,data})
export const createDecrementAction = data => ({type:DECREMENT,data})

//异步action，就是指action的值为函数,异步action中一般都会调用同步action，异步action不是必须要用的。
export const createIncrementAsyncAction = (data,time) => {
	return (dispatch)=>{
		setTimeout(()=>{
			dispatch(createIncrementAction(data))
		},time)
	}
}
```

```javascript
import {ADD_PERSON} from '../constant'

//创建增加一个人的action动作对象
export const createAddPersonAction = personObj => ({type:ADD_PERSON,data:personObj})
```

```javascript
/* 
	1.该文件是用于创建一个为Count组件服务的reducer，reducer的本质就是一个函数
	2.reducer函数会接到两个参数，分别为：之前的状态(preState)，动作对象(action)
*/
import { INCREMENT, DECREMENT } from "../constant";

const initState = 0; //初始化状态
export default function countReducer(preState = initState, action) {
  //从action对象中获取：type、data
  const { type, data } = action;
  //根据type决定如何加工数据
  switch (type) {
    case INCREMENT: //如果是加
      return preState + data;
    case DECREMENT: //若果是减
      return preState - data;
    default:
      return preState;
  }
}

```

```javascript
import { ADD_PERSON } from "../constant";

//初始化人的列表
const initState = [{ id: "001", name: "tom", age: 18 }];

export default function personReducer(preState = initState, action) {
  const { type, data } = action;
  switch (type) {
    case ADD_PERSON: //若是添加一个人
      //preState.unshift(data) //此处不可以这样写，这样会导致preState被改写了，personReducer就不是纯函数了。纯函数的定义是原数组不能发生数据改变，改变了就不纯了。并且reducer必须是纯函数。
      //如果使用上面那种方法，虽然数组的内容发生改变了，但是react不会检测出数组内容发生改变，下面这种方式是新建了一个数组，地址都改变了，所以会被监测到，这里也符合纯函数的定义，所以我们一般都不用数组自带的方法进行push什么的，而是用展开运算符进行重新创建一个数组，但是这里我觉得不太好，会有效率问题。。。
      return [data, ...preState];
    default:
      return preState;
  }
}

```

```javascript
/* 
	该文件专门用于暴露一个store对象，整个应用只有一个store对象
*/

//引入createStore，专门用于创建redux中最为核心的store对象
import { createStore, applyMiddleware, combineReducers } from "redux";
//引入为Count组件服务的reducer
import countReducer from "./reducers/count";
//引入为Count组件服务的reducer
import personReducer from "./reducers/person";
//引入redux-thunk，用于支持异步action
import thunk from "redux-thunk";
//引入redux-devtools-extension
import { composeWithDevTools } from "redux-devtools-extension";

//汇总所有的reducer变为一个总的reducer
const allReducer = combineReducers({
  he: countReducer,
  rens: personReducer,
});

//暴露store
// 第一个参数是汇总所有的reducer为一个参数。
// 这里的第二个参数是引入Redux的工具，个人感觉这个写法很差劲，一点都不优雅，不如pinia那样简便，这里还需要下载依赖和进行代码的配置，太麻烦了
export default createStore(
  allReducer,
  composeWithDevTools(applyMiddleware(thunk))
);

```

```javascript
import React, { Component } from "react";
//引入action
import {
  createIncrementAction,
  createDecrementAction,
  createIncrementAsyncAction,
} from "../../redux/actions/count";
//引入connect用于连接UI组件与redux
import { connect } from "react-redux";

//定义UI组件
class Count extends Component {
  //加法
  increment = () => {
    const { value } = this.selectNumber;
    this.props.jia(value * 1);
  };
  //减法
  decrement = () => {
    const { value } = this.selectNumber;
    this.props.jian(value * 1);
  };
  //奇数再加
  incrementIfOdd = () => {
    const { value } = this.selectNumber;
    if (this.props.count % 2 !== 0) {
      this.props.jia(value * 1);
    }
  };
  //异步加
  incrementAsync = () => {
    const { value } = this.selectNumber;
    this.props.jiaAsync(value * 1, 500);
  };

  render() {
    //console.log('UI组件接收到的props是',this.props);
    return (
      <div>
        <h2>我是Count组件,下方组件总人数为:{this.props.renshu}</h2>
        <h4>当前求和为：{this.props.count}</h4>
        <select ref={(c) => (this.selectNumber = c)}>
          <option value="1">1</option>
          <option value="2">2</option>
          <option value="3">3</option>
        </select>
        &nbsp;
        <button onClick={this.increment}>+</button>&nbsp;
        <button onClick={this.decrement}>-</button>&nbsp;
        <button onClick={this.incrementIfOdd}>当前求和为奇数再加</button>&nbsp;
        <button onClick={this.incrementAsync}>异步加</button>&nbsp;
      </div>
    );
  }
}

//使用connect()()创建并暴露一个Count的容器组件
export default connect(
  (state) => ({
    count: state.he,
    renshu: state.rens.length,
  }),
  {
    jia: createIncrementAction,
    jian: createDecrementAction,
    jiaAsync: createIncrementAsyncAction,
  }
)(Count);

```

```javascript
import React, { Component } from "react";
import { nanoid } from "nanoid";
import { connect } from "react-redux";
import { createAddPersonAction } from "../../redux/actions/person";

class Person extends Component {
  addPerson = () => {
    const name = this.nameNode.value;
    const age = this.ageNode.value * 1;
    const personObj = { id: nanoid(), name, age };
    this.props.jiaYiRen(personObj);
    this.nameNode.value = "";
    this.ageNode.value = "";
  };

  render() {
    return (
      <div>
        <h2>我是Person组件,上方组件求和为{this.props.he}</h2>
        <input
          ref={(c) => (this.nameNode = c)}
          type="text"
          placeholder="输入名字"
        />
        <input
          ref={(c) => (this.ageNode = c)}
          type="text"
          placeholder="输入年龄"
        />
        <button onClick={this.addPerson}>添加</button>
        <ul>
          {this.props.yiduiren.map((p) => {
            return (
              <li key={p.id}>
                {p.name}--{p.age}
              </li>
            );
          })}
        </ul>
      </div>
    );
  }
}

export default connect(
  (state) => ({ yiduiren: state.rens, he: state.he }), //映射状态
  { jiaYiRen: createAddPersonAction } //映射操作状态的方法
)(Person);

```

运行效果<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689318437871-f3e49e9e-4889-42f7-b788-3089861c0c30.png#averageHue=%23f3f2f1&clientId=ueb9972e6-a3a0-4&from=paste&height=358&id=ub11d0d12&originHeight=447&originWidth=471&originalType=binary&ratio=1&rotation=0&showTitle=false&size=30251&status=done&style=shadow&taskId=ud4de08b5-998b-4cb0-8e49-0da1a72e70c&title=&width=376.8)
<a name="Dg4k5"></a>

### 最终版

(1).所有变量名字要规范，尽量触发对象的简写形式。<br />(2).reducers文件夹中，编写index.js专门用于汇总并暴露所有的reducer

```javascript
/* 
	该文件用于汇总所有的reducer为一个总的reducer
*/
//引入combineReducers，用于汇总多个reducer
import {combineReducers} from 'redux'
//引入为Count组件服务的reducer
import count from './count'
//引入为Person组件服务的reducer
import persons from './person'

//汇总所有的reducer变为一个总的reducer
export default  combineReducers({
	count,
	persons
})

```

<a name="bxbgX"></a>

## **7.7. 使用上redux调试工具**

(1).yarn add redux-devtools-extension<br />(2).store中进行配置<br />    import {composeWithDevTools} from 'redux-devtools-extension'<br />    const store = createStore(allReducer,composeWithDevTools(applyMiddleware(thunk)))
<a name="kImz7"></a>

### **7.7.1. 安装chrome浏览器插件**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689060259802-c285da74-7bc8-42ed-a953-c7f0ff72aa8f.png#averageHue=%23f2efed&clientId=uf6354842-c5be-4&from=paste&height=375&id=u27e35bc5&originHeight=375&originWidth=1206&originalType=binary&ratio=1&rotation=0&showTitle=false&size=97300&status=done&style=shadow&taskId=uab15e449-e258-4e30-bf54-e0c5d9df5c3&title=&width=1206)
<a name="gU18B"></a>

### **7.7.2. 下载工具依赖包**

npm install --save-dev redux-devtools-extension
<a name="kpHGV"></a>

### 7.7.3. 配置

```javascript
/* 
	该文件专门用于暴露一个store对象，整个应用只有一个store对象
*/

//引入createStore，专门用于创建redux中最为核心的store对象
import { createStore, applyMiddleware, combineReducers } from "redux";
//引入为Count组件服务的reducer
import countReducer from "./reducers/count";
//引入为Count组件服务的reducer
import personReducer from "./reducers/person";
//引入redux-thunk，用于支持异步action
import thunk from "redux-thunk";
//引入redux-devtools-extension
import { composeWithDevTools } from "redux-devtools-extension";

//汇总所有的reducer变为一个总的reducer
const allReducer = combineReducers({
  he: countReducer,
  rens: personReducer,
});

//暴露store
// 第一个参数是汇总所有的reducer为一个参数。
// 这里的第二个参数是引入Redux的工具，个人感觉这个写法很差劲，一点都不优雅，不如pinia那样简便，这里还需要下载依赖和进行代码的配置，太麻烦了
export default createStore(
  allReducer,
  composeWithDevTools(applyMiddleware(thunk))
);

```

<a name="trdi0"></a>

## **7.8. 纯函数和高阶函数**

<a name="Hap0w"></a>

### **7.8.1. 纯函数**

1. 一类特别的函数: 只要是同样的输入(实参)，必定得到同样的输出(返回)<br />2. 必须遵守以下一些约束  <br />1) 不得改写参数数据<br />2) 不会产生任何副作用，例如网络请求，输入和输出设备<br />3) 不能调用Date.now()或者Math.random()等不纯的方法  <br />3. redux的reducer函数必须是一个纯函数
   <a name="NgRLq"></a>

### **7.8.2. 高阶函数**

1. 理解: 一类特别的函数<br />1) 情况1: 参数是函数<br />2) 情况2: 返回是函数<br />2. 常见的高阶函数: <br />1) 定时器设置函数<br />2) 数组的forEach()/map()/filter()/reduce()/find()/bind()<br />3) promise<br />4) react-redux中的connect函数<br />3. 作用: 能实现更加动态, 更加可扩展的功能

<a name="Kc9cR"></a>

# 扩展

<a name="UPnno"></a>

## 1. setState

<a name="WJLaw"></a>

### setState更新状态的2种写法

(1). setState(stateChange, [callback])------对象式的setState<br />  1.stateChange为状态改变对象(该对象可以体现出状态的更改)<br />  2.callback是可选的回调函数, 它在状态更新完毕、界面也更新后(render调用后)才被调用<br />(2). setState(updater, [callback])------函数式的setState<br />  1.updater为返回stateChange对象的函数。<br />  2.updater可以接收到state和props。<br />  3.callback是可选的回调函数, 它在状态更新、界面也更新后(render调用后)才被调用。<br />总结:<br />  1.对象式的setState是函数式的setState的简写方式(语法糖)<br />  2.使用原则：<br />          (1).如果新状态不依赖于原状态 ===> 使用对象方式<br />          (2).如果新状态依赖于原状态 ===> 使用函数方式<br />          (3).如果需要在setState()执行后获取最新的状态数据,(因为react在更新state的时候是异步的), 要在第二个callback函数中读取

```jsx
import React, { Component } from "react";

export default class Demo extends Component {
  state = { count: 0 };

  add = () => {
    //对象式的setState
    //1.获取原来的count值
    const { count } = this.state;
    //2.更新状态
    this.setState({ count: count + 1 }, () => {
      console.log(this.state.count); //这里会获得更新后的值，更新后会调用这段代码
    });
    console.log("12行的输出", this.state.count); //还是更新前的值，原因是react更新state是异步的，所以这句代码会比更新先执行。

    //函数式的setState
    this.setState((state) => ({ count: state.count + 1 })); //这里当然也可以传一个回调。也就是第二个参数，和上面的一样
  };

  render() {
    return (
      <div>
        <h1>当前求和为：{this.state.count}</h1>
        <button onClick={this.add}>点我+1</button>
      </div>
    );
  }
}

```

<a name="H7rcY"></a>

## 2. lazyLoad

<a name="C1qpL"></a>

### 路由组件的lazyLoad

一般用作路由组件的懒加载，平时的路由是只要启动的话就会全部加载，懒加载处理后就会用到哪个加载哪个

```jsx
import React, { Component, lazy, Suspense } from "react";
import { NavLink, Route } from "react-router-dom";

// 原始的引入，这里会直接引入进去，没有实现懒加载
// import Home from './Home'
// import About from './About'

// Loading组件是下面某个组件正在加载中的时候呈现的，所以需要直接引入
import Loading from "./Loading";
// 这两个可以用懒加载
const Home = lazy(() => import("./Home"));
const About = lazy(() => import("./About"));

export default class Demo extends Component {
  render() {
    return (
      <div>
        <div className="row">
          <div className="col-xs-offset-2 col-xs-8">
            <div className="page-header">
              <h2>React Router Demo</h2>
            </div>
          </div>
        </div>
        <div className="row">
          <div className="col-xs-2 col-xs-offset-2">
            <div className="list-group">
              {/* 在React中靠路由链接实现切换组件--编写路由链接 */}
              <NavLink className="list-group-item" to="/about">
                About
              </NavLink>
              <NavLink className="list-group-item" to="/home">
                Home
              </NavLink>
            </div>
          </div>
          <div className="col-xs-6">
            <div className="panel">
              <div className="panel-body">
                {/* 
                  这里必须要指定这个标签，意思是下面路由组件正在加载的时候显示的组件
                  如果没有这个标签的话会报错
                */}
                <Suspense fallback={<Loading />}>
                  {/* 注册路由 */}
                  <Route path="/about" component={About} />
                  <Route path="/home" component={Home} />
                </Suspense>
              </div>
            </div>
          </div>
        </div>
      </div>
    );
  }
}

```

<a name="dTiZT"></a>

## 3. Hooks

<a name="BrVF9"></a>

### 1. React Hook/Hooks是什么?

(1). Hook是React 16.8.0版本增加的新特性/新语法<br />(2). 可以让你在函数组件中使用 state 以及其他的 React 特性
<a name="eBpAp"></a>

### 2. 三个常用的Hook

(1). State Hook: React.useState()<br />(2). Effect Hook: React.useEffect() 使用生命周期<br />(3). Ref Hook: React.useRef()
<a name="dwiLm"></a>

### 3. State Hook

(1). State Hook让函数组件也可以有state状态, 并进行状态数据的读写操作<br />(2). 语法: const [xxx, setXxx] = React.useState(initValue)  <br />(3). useState()说明:<br />        参数: 第一次初始化指定的值在内部作缓存<br />        返回值: 包含2个元素的数组, 第1个为内部当前状态值, 第2个为更新状态值的函数<br />(4). setXxx()2种写法:<br />        setXxx(newValue): 参数为非函数值, 直接指定新的状态值, 内部用其覆盖原来的状态值<br />        setXxx(value => newValue): 参数为函数, 接收原本的状态值, 返回新的状态值, 内部用其覆盖原来的状态值
<a name="hcyS3"></a>

### 4. Effect Hook

(1). Effect Hook 可以让你在函数组件中执行副作用操作(用于模拟类组件中的生命周期钩子)<br />(2). React中的副作用操作:<br />        发ajax请求数据获取<br />        设置订阅 / 启动定时器<br />        手动更改真实DOM<br />(3). 语法和说明: <br />        useEffect(() => { <br />          // 在此可以执行任何带副作用操作<br />          return () => { // 在组件卸载前执行<br />            // 在此做一些收尾工作, 比如清除定时器/取消订阅等<br />          }<br />        }, [stateValue]) //这里可以指定是检测哪个state的update操作。 如果指定的是[], 回调函数只会在第一次render()后执行<br />    <br />(4). 可以把 useEffect Hook 看做如下三个函数的组合<br />        componentDidMount()<br />        componentDidUpdate()<br />        componentWillUnmount()
<a name="vXfnF"></a>

### 5. Ref Hook

这个很像creatRef,专人专用<br />(1). Ref Hook可以在函数组件中存储/查找组件内的标签或任意其它数据<br />(2). 语法: const refContainer = useRef()<br />(3). 作用:保存标签对象,功能与React.createRef()一样

```jsx
import React from "react";
import ReactDOM from "react-dom";


function Demo() {
  // 这里react底层做了处理，再调用这个的时候不会再改变count的值了
  const [count, setCount] = React.useState(0);
  const myRef = React.useRef();

  React.useEffect(() => {
    let timer = setInterval(() => {
      setCount((count) => count + 1);
    }, 1000);
    return () => {
      clearInterval(timer);
    };
  }, []);

  //加的回调
  function add() {
    //setCount(count+1) //第一种写法
    setCount((count) => count + 1);
  }

  //提示输入的回调
  function show() {
    alert(myRef.current.value);
  }

  //卸载组件的回调
  function unmount() {
    ReactDOM.unmountComponentAtNode(document.getElementById("root"));
  }

  return (
    <div>
      <input type="text" ref={myRef} />
      <h2>当前求和为：{count}</h2>
      <button onClick={add}>点我+1</button>
      <button onClick={unmount}>卸载组件</button>
      <button onClick={show}>点我提示数据</button>
    </div>
  );
}

export default Demo;

```

<a name="PjPTf"></a>

## 4. Fragment

jsx语法是不能出现多个根标签，Fragment可以做那个根标签，并且react在渲染真实dom的时候会直接忽略这个标签<br />这个很像vue里面的template标签，但是相比较template不能做根标签
<a name="BcLiH"></a>

### 使用

<Fragment><Fragment><br /><></>

```
import React, { Component, Fragment } from "react";

export default class Demo extends Component {
  render() {
    return (
      // <Fragment key={1}>
      // 	<input type="text"/>
      // 	<input type="text"/>
      // </Fragment>
      //这个和上面的区别只有上面可以传递key属性，其他的没什么区别
      <>
        <input type="text" />
        <input type="text" />
      </>
    );
  }
}

```

这两个是差不多的，但是Fragment标签可以传输key属性
<a name="dNXiO"></a>

### 作用

可以不用必须有一个真实的DOM根标签了
<a name="UkVSs"></a>

## 5. Context

<a name="cj6Vx"></a>

### 理解

一种组件间通信方式, 常用于【祖组件】与【后代组件】间通信![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689407629805-8e6159a7-93b8-45a2-bf6d-1781813f22bf.png#averageHue=%23fbf8f0&clientId=ubf169963-208c-4&from=paste&height=259&id=u9cdccca2&originHeight=324&originWidth=417&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=29201&status=done&style=shadow&taskId=u16eca9b6-9ca5-4da3-9684-c10c72dd9f7&title=&width=333.6)也就是A和C、D间的通信
<a name="kJAmW"></a>

### 使用

```javascript
1) 创建Context容器对象：
const XxxContext = React.createContext()  

2) 渲染子组时，外面包裹xxxContext.Provider, 通过value属性给后代组件传递数据：
  <xxxContext.Provider value={数据}>
  子组件
  </xxxContext.Provider>

3) 后代组件读取数据：

  //第一种方式:仅适用于类组件 
  static contextType = xxxContext  // 声明接收context
  this.context // 读取context中的value数据

  //第二种方式: 函数组件与类组件都可以
  <xxxContext.Consumer>
  {
    value => ( // value就是context中的value数据
      要显示的内容
    )
}
</xxxContext.Consumer>
```

<a name="kf8oK"></a>

### 具体代码

```jsx
import React, { Component } from "react";
import "./index.css";

//创建Context对象
const MyContext = React.createContext();
const { Provider, Consumer } = MyContext;
export default class A extends Component {
  state = { username: "tom", age: 18 };

  render() {
    const { username, age } = this.state;
    return (
      <div className="parent">
        <h3>我是A组件</h3>
        <h4>我的用户名是:{username}</h4>
        <Provider value={{ username, age }}>
          <B />
        </Provider>
      </div>
    );
  }
}

class B extends Component {
  render() {
    return (
      <div className="child">
        <h3>我是B组件</h3>
        <C />
      </div>
    );
  }
}

//第一种方式:仅适用于类组件
/* class C extends Component {
	//声明接收context
	static contextType = MyContext
	render() {
		const {username,age} = this.context
		return (
			<div className="grand">
				<h3>我是C组件</h3>
				<h4>我从A组件接收到的用户名:{username},年龄是{age}</h4>
			</div>
		)
	}
} */

//第二种方式: 函数组件与类组件都可以
function C() {
  return (
    <div className="grand">
      <h3>我是C组件</h3>
      <h4>
        我从A组件接收到的用户名:
        <Consumer>{(value) => `${value.username},年龄是${value.age}`}</Consumer>
      </h4>
    </div>
  );
}

```

<a name="uYkI3"></a>

### 注意

在应用开发中一般不用context, 一般都用它的封装react插件（react-redux）
<a name="KD48D"></a>

## 6. 组件优化

<a name="hWd4b"></a>

### Component的2个问题 

1. 只要执行setState(),即使不改变状态数据, 组件也会重新render() ==> 效率低
2. 只要当前组件重新render(), 就会自动重新render子组件（父组件render，子组件也得render），纵使子组件没有用到父组件的任何数据 ==> 效率低
   <a name="qGwek"></a>

### 效率高的做法

只有当组件的state或props数据发生改变时才重新render()
<a name="gLO4g"></a>

### 原因

Component中的shouldComponentUpdate()总是返回true
<a name="JwhIH"></a>

### 解决

办法1: <br />	重写shouldComponentUpdate()方法<br />	比较新旧state或props数据, 如果有变化才返回true, 如果没有返回false<br />办法2:  <br />	使用PureComponent<br />	PureComponent重写了shouldComponentUpdate(), 只有state或props数据有变化才返回true<br />	注意: <br />		只是进行state和props数据的浅比较, 如果只是数据对象内部数据变了, 返回false  <br />		不要直接修改state数据, 而是要产生新数据（创建新对象）<br />项目中一般使用PureComponent来优化

```jsx
import React, { Component } from 'react'
import './index.css'

export default class Parent extends Component {

  state = {carName:"奔驰c36"}

  changeCar = ()=>{
    this.setState({carName:'迈巴赫'})
  }

  shouldComponentUpdate(nextProps,nextState){
    // console.log(this.props,this.state); //目前的props和state
    // console.log(nextProps,nextState); //接下要变化的目标props，目标state
    return !this.state.carName === nextState.carName
  } 

  render() {
    console.log('Parent---render');
    const {carName} = this.state
    return (
      <div className="parent">
        <h3>我是Parent组件</h3>
        {this.state.stus}&nbsp;
        <span>我的车名字是：{carName}</span><br/>
        <button onClick={this.changeCar}>点我换车</button>
        <Child carName="奥拓"/>
      </div>
    )
  }
}

class Child extends Component {

  shouldComponentUpdate(nextProps,nextState){
    console.log(this.props,this.state); //目前的props和state
    console.log(nextProps,nextState); //接下要变化的目标props，目标state
    return !this.props.carName === nextProps.carName
  } 

  render() {
    console.log('Child---render');
    return (
      <div className="child">
        <h3>我是Child组件</h3>
        <span>我接到的车是：{this.props.carName}</span>
      </div>
    )
  }
}

```

```jsx
import React, { PureComponent } from "react";
import "./index.css";

export default class Parent extends PureComponent {
  state = { carName: "奔驰c36"};
  changeCar = () => {
    this.setState({carName:'迈巴赫'})
  };
  render() {
    console.log("Parent---render");
    const { carName } = this.state;
    return (
      <div className="parent">
        <h3>我是Parent组件</h3>
        <span>我的车名字是：{carName}</span>
        <br />
        <button onClick={this.changeCar}>点我换车</button>
        <Child carName="奥拓" />
      </div>
    );
  }
}

class Child extends PureComponent {
  render() {
    console.log("Child---render");
    return (
      <div className="child">
        <h3>我是Child组件</h3>
        <span>我接到的车是：{this.props.carName}</span>
      </div>
    );
  }
}

```

<a name="EpzUb"></a>

## 7. render props

<a name="dS4Ve"></a>

### 如何向组件内部动态传入带内容的结构(标签)?

Vue中: <br />	使用slot技术, 也就是通过组件标签体传入结构  <A><B/></A><br />React中:<br />	使用children props: 通过组件标签体传入结构<br />	使用render props: 通过组件标签属性传入结构,而且可以携带数据，一般用render函数属性
<a name="H8OTB"></a>

### children props

```jsx
<A>
  <B>xxxx</B>
</A>
{this.props.children}
问题: 如果B组件需要A组件内的数据, ==> 做不到
```

具体代码

```jsx
import React, { Component } from "react";
import "./index.css";

export default class Parent extends Component {
  render() {
    return (
      <div className="parent">
        <h3>我是Parent组件</h3>
        <A>
          <B />
        </A>
      </div>
    );
  }
}

class A extends Component {
  state = { name: "tom" };
  render() {
    console.log(this.props);
    const { name } = this.state;
    return (
      <div className="a">
        <h3>我是A组件</h3>
        {/* 渲染B，但是这里没办法传递props */}
        {this.props.children()}
      </div>
    );
  }
}

class B extends Component {
  render() {
    console.log("B--render");
    return (
      <div className="b">
        <h3>我是B组件</h3>
      </div>
    );
  }
}

```

<a name="QLtN5"></a>

### render props

```jsx
<A render={(data) => <C data={data}></C>}></A>
A组件: {this.props.render(内部state数据)}
C组件: 读取A组件传入的数据显示 {this.props.data}
```

具体代码

```jsx
import React, { Component } from "react";
import "./index.css";

export default class Parent extends Component {
  render() {
    return (
      <div className="parent">
        <h3>我是Parent组件</h3>
        {/* 渲染A，并且把B作为A的子组件传入进去，并且可以把A的name属性传入进去 */}
        <A render={(name) => <B name={name} />} />
      </div>
    );
  }
}

class A extends Component {
  state = { name: "tom" };
  render() {
    console.log(this.props);
    const { name } = this.state;
    return (
      <div className="a">
        <h3>我是A组件</h3>
        {/* 在这里进行渲染B并且把A的name给传入进去 */}
        {this.props.render(name)}
      </div>
    );
  }
}

class B extends Component {
  render() {
    console.log("B--render");
    return (
      <div className="b">
        {/* 接收到A的name属性 */}
        <h3>我是B组件,{this.props.name}</h3>
      </div>
    );
  }
}

```

<a name="qc4DC"></a>

## 8. 错误边界

**开发环境还是会报错**，**生产环境不会报错**
<a name="vTddl"></a>

### 理解：

错误边界(Error boundary)：用来捕获后代组件错误，渲染出备用页面<br />错误边界需要在容易发生错误的子组件的父组件做一些配置，如果子组件出现了错误，父组件仍然可以展示，并且会把子组件渲染成一个专门用于展示错误的组件，只把错误限制到子组件中，父组件仍然可以展示
<a name="EPa14"></a>

### 特点：

只能捕获后代组件生命周期函数中产生的错误，不能捕获自己组件产生的错误和其他组件在合成事件、定时器中产生的错误
<a name="LUkUh"></a>

### 使用方式：

getDerivedStateFromError配合componentDidCatch

```jsx
// 生命周期函数，一旦后台组件报错，就会触发
static getDerivedStateFromError(error) {
  console.log(error);
  // 在render之前触发
  // 返回新的state
  return {
    hasError: true,
  };
}

componentDidCatch(error, info) {
  // 统计页面的错误。发送请求发送到后台去
  console.log(error, info);
}
```

<a name="Cov0j"></a>

### 具体实例：

```jsx
import React, { Component } from "react";
import Child from "./Child";

export default class Parent extends Component {
  state = {
    hasError: "", //用于标识子组件是否产生错误
  };

  //当Parent的子组件出现报错时候，会触发getDerivedStateFromError调用，并携带错误信息
  static getDerivedStateFromError(error) {
    console.log("@@@", error); //这里会打印错误信息
    return { hasError: error }; //这里会调用setState(),之后会改变state里面的hasError属性
  }

  //捕获异常的时候会触发这个函数
  componentDidCatch() {
    console.log("此处统计错误，反馈给服务器，用于通知编码人员进行bug的解决");
  }

  render() {
    return (
      <div>
        <h2>我是Parent组件</h2>
        {/* 下面判断hasError是否为null或者是空字符串进行显示 */}
        {this.state.hasError ? <h2>当前网络不稳定，稍后再试</h2> : <Child />}
      </div>
    );
  }
}

```

```jsx
import React, { Component } from "react";

export default class Child extends Component {
  state = {
    users: [
      { id: "001", name: "tom", age: 18 },
      { id: "002", name: "jack", age: 19 },
      { id: "003", name: "peiqi", age: 20 },
    ],
    // users:'abc'//这里模拟数据传输出了问题，子组件会报错
  };

  render() {
    return (
      <div>
        <h2>我是Child组件</h2>
        {this.state.users.map((userObj) => {
          return (
            <h4 key={userObj.id}>
              {userObj.name}----{userObj.age}
            </h4>
          );
        })}
      </div>
    );
  }
}

```

<a name="UWRv7"></a>

## 9. 组件通信方式总结

<a name="hmABm"></a>

### 组件间的关系：

- 父子组件
- 兄弟组件（非嵌套组件）
- 祖孙组件（跨级组件）
  <a name="KCQYt"></a>

### 几种通信方式：

1.props：<br />  (1).children props<br />  (2).render props<br />2.消息订阅-发布：<br /> 	 pubs-sub<br />3.集中式管理：<br /> 	 redux<br />4.conText:<br /> 	 生产者-消费者模式
<a name="QtG7c"></a>

### 比较好的搭配方式：

父子组件：props<br />兄弟组件：消息订阅-发布、集中式管理<br />祖孙组件(跨级组件)：消息订阅-发布、集中式管理、conText(开发用的少，封装插件用的多)
<a name="zJZeP"></a>

# 打包部署

npm run build  打包<br />npm i serve -g  安装serve<br />serve -s build 利用serve启动应用
<a name="En2M9"></a>

# React Router 6 快速上手

<a name="LH8dI"></a>

# ![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689254999069-800bace7-5d33-452b-91b7-c03cc0a5b2ca.png#averageHue=%23d9d9d9&clientId=uc25a49b6-3eac-4&from=paste&id=KpHhf&originHeight=152&originWidth=599&originalType=url&ratio=1.25&rotation=0&showTitle=false&size=11521&status=done&style=shadow&taskId=u4ce29864-edef-49e3-afa0-680728b2176&title=)

<a name="CwKLp"></a>

## 1.概述

1. React Router 以三个不同的包发布到 npm 上，它们分别为：
   1. react-router: 路由的核心库，提供了很多的：组件、钩子。
   2. **react-router-dom(我们学这个):包含react-router所有内容，并添加一些专门用于 DOM 的组件，例如 <BrowserRouter>等 **。
   3. react-router-native: 包括react-router所有内容，并添加一些专门用于ReactNative的API，例如:<NativeRouter>等。
2. 与React Router 5.x 版本相比，改变了什么？
   1. 内置组件的变化：移除<Switch/> ，新增 <Routes/>等。
   2. 语法的变化：component={About} 变为 element={<About/>}等。
   3. 新增多个hook：useParams、useNavigate、useMatch等。
   4. 新增多个hook说明！**官方明确推荐函数式组件了！！！**......

安装react-router<br />`npm i react-router-dom`
<a name="HRcY9"></a>

## 2.Component

<a name="orHPl"></a>

### 1. <BrowserRouter>

1. 说明：<BrowserRouter>用于包裹整个应用。
2. 示例代码：

```jsx
import React from "react";
import ReactDOM from "react-dom";
import { BrowserRouter } from "react-router-dom";

ReactDOM.render(
  <BrowserRouter>
    {/* 整体结构（通常为App组件） */}
  </BrowserRouter>,root
);
```

<a name="mMY7n"></a>

### 2. <HashRouter>

1. 说明：作用与<BrowserRouter>一样，但<HashRouter>修改的是地址栏的hash值。
2. 备注：6.x版本中<HashRouter>、<BrowserRouter> 的用法与 5.x 相同。
   <a name="WKtqy"></a>

### 3. <Routes/> 与 <Route/>

1. v6版本中移出了先前的<Switch>，引入了新的替代者：<Routes>。
2. <Routes> 和 <Route>要配合使用，且必须要用<Routes>包裹<Route>。
3. <Route> 相当于一个 if 语句，如果其路径与当前 URL 匹配，则呈现其对应的组件。
4. <Route caseSensitive> 属性用于指定：匹配时是否区分大小写（默认为 false）。
5. 当URL发生变化时，<Routes>都会查看其所有子<Route> 元素以找到最佳匹配并呈现组件 。
6. <Route> 也可以嵌套使用，且可配合useRoutes()配置 “路由表” ，但需要通过 <Outlet> 组件来渲染其子路由。
7. 示例代码：

```jsx
<Routes>
  /*path属性用于定义路径，element属性用于定义当前路径所对应的组件*/
  <Route path="/login" element={<Login />}></Route>

  /*用于定义嵌套路由，home是一级路由，对应的路径/home*/
  <Route path="home" element={<Home />}>
    /*test1 和 test2 是二级路由,对应的路径是/home/test1 或 /home/test2*/
    <Route path="test1" element={<Test/>}></Route>
    <Route path="test2" element={<Test2/>}></Route>
  </Route>

  //Route也可以不写element属性, 这时就是用于展示嵌套的路由 .所对应的路径是/users/xxx
  <Route path="users">
    <Route path="xxx" element={<Demo />} />
  </Route>
</Routes>
```

<a name="TLzfT"></a>

### 4. <Link>

1. 作用: 修改URL，且不发送网络请求（路由链接）。
2. 注意: 外侧需要用<BrowserRouter>或<HashRouter>包裹。
3. 示例代码：

```jsx
import { Link } from "react-router-dom";

function Test() {
  return (
    <div>
      <Link to="/路径">按钮</Link>
    </div>
  );
}
```

<a name="sdGYN"></a>

### 5. <NavLink>

1. 作用: 与<Link>组件类似，且可实现导航的“高亮”效果。
2. 示例代码：

```jsx
import React from "react";
import { NavLink, Routes, Route, Navigate } from "react-router-dom";
import About from "./pages/About";
import Home from "./pages/Home";

export default function App() {
  function computedClassName({ isActive }) {
    // list-group-item是bootstrap的样式  atguigu是点击激活的样式
    // 下面的意思是isActive如果是true，说明这个标签被激活了，所以我们得给他加上激活的样式，atguigu是点击激活的样式
    return isActive ? "list-group-item atguigu" : "list-group-item";
  }
  return (
    <div>
      <div className="row">
        <div className="col-xs-offset-2 col-xs-8">
          <div className="page-header">
            <h2>React Router Demo</h2>
          </div>
        </div>
      </div>
      <div className="row">
        <div className="col-xs-2 col-xs-offset-2">
          <div className="list-group">
            {/* 路由链接 */}
            {/* className的值可以为一个函数, 这个函数写在了上面*/}
            <NavLink className={computedClassName} to="/about">
              About
            </NavLink>
            <NavLink className={computedClassName} to="/home">
              Home
            </NavLink>
          </div>
        </div>
        <div className="col-xs-6">
          <div className="panel">
            <div className="panel-body">
              {/* 注册路由 */}
              <Routes>
                <Route path="/ABOUT" element={<About />} />
                <Route path="/home" element={<Home />} />
                <Route path="/" element={<Navigate to="/about" />} />
              </Routes>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}

```

<a name="XjhGu"></a>

### 6. <Navigate>

1. 作用：只要<Navigate>组件被渲染，就会修改路径，切换视图。
2. replace属性用于控制跳转模式（push 或 replace，默认是push）。
3. 示例代码：

```jsx
import React,{useState} from 'react'
import {Navigate} from 'react-router-dom'

export default function Home() {
  const [sum,setSum] = useState(1)
  return (
    <div>
      <h3>我是Home的内容</h3>
      {/* 根据sum的值决定是否切换视图,下面显示的是如果sum为1就显示h4标签，否则就跳转路由到about页面，并且为replace模式（与push相照应） */}
      {sum === 1 ? <h4>sum的值为{sum}</h4> : <Navigate to="/about" replace={true}/>}
      <button onClick={()=>setSum(2)}>点我将sum变为2</button>
    </div>
  )
}
```

还可以用来表示默认跳转到哪个页面

```jsx
import React from "react";
import { NavLink, Routes, Route, Navigate } from "react-router-dom";
import About from "./pages/About";
import Home from "./pages/Home";

export default function App() {
  return (
    <div>
      <div className="row">
        <div className="col-xs-offset-2 col-xs-8">
          <div className="page-header">
            <h2>React Router Demo</h2>
          </div>
        </div>
      </div>
      <div className="row">
        <div className="col-xs-2 col-xs-offset-2">
          <div className="list-group">
            {/* 路由链接 */}
            <NavLink className="list-group-item" to="/about">
              About
            </NavLink>
            <NavLink className="list-group-item" to="/home">
              Home
            </NavLink>
          </div>
        </div>
        <div className="col-xs-6">
          <div className="panel">
            <div className="panel-body">
              {/* 注册路由 */}
              {/* Routes和Switch的区别是Switch是为了提升效率，可写可不写。
              但是Routes必须得写，也能实现Switch的功能。 */}
              <Routes>
                <Route path="/about" element={<About />} />
                {/* 默认不区分大小写 */}
                <Route path="/HOME" element={<Home />} />
                {/* 这里表示如果路径是/，也就会默认重定向到about路径下，进行渲染About组件 */}
                <Route path="/" element={<Navigate to="/about" />} />
              </Routes>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}

```

<a name="vgxP8"></a>

### 7. <Outlet>

1. 当<Route>产生嵌套时，渲染其对应的后续子路由。
2. 示例代码：

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689417002442-f0a77363-d1e4-4087-bdf3-71e75854e8cb.png#averageHue=%23211f1d&clientId=ubf169963-208c-4&from=paste&height=192&id=u8287f5d1&originHeight=240&originWidth=255&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=11895&status=done&style=shadow&taskId=udf6c46cf-97d8-41ab-9ccc-11bc1cc7ed3&title=&width=204)

```jsx
import About from "../pages/About";
import Home from "../pages/Home";
import Message from "../pages/Message";
import News from "../pages/News";
import { Navigate } from "react-router-dom";

export default [
  {
    path: "/about",
    element: <About />,
  },
  {
    path: "/home",
    element: <Home />,
    children: [
      {
        // 子路由可以直接写路径，不需要再加父的路径了
        path: "news",
        element: <News />,
      },
      {
        path: "message",
        element: <Message />,
      },
    ],
  },
  {
    path: "/",
    element: <Navigate to="/about" />,
  },
];

```

```jsx
import React from "react";
import { NavLink, useRoutes } from "react-router-dom";
import routes from "./routes";

export default function App() {
  //根据路由表生成对应的路由规则
  const element = useRoutes(routes);
  return (
    <div>
      <div className="row">
        <div className="col-xs-offset-2 col-xs-8">
          <div className="page-header">
            <h2>React Router Demo</h2>
          </div>
        </div>
      </div>
      <div className="row">
        <div className="col-xs-2 col-xs-offset-2">
          <div className="list-group">
            {/* 路由链接 */}
            <NavLink className="list-group-item" to="/about">
              About
            </NavLink>
            {/*
            <NavLink className="list-group-item" end to="/home">Home</NavLink> 这里的意思是如果子路由被点击了，父路由的高亮就不显示了
            */}
            <NavLink className="list-group-item" to="/home">
              Home
            </NavLink>
          </div>
        </div>
        <div className="col-xs-6">
          <div className="panel">
            <div className="panel-body">
              {/* 
                  注册路由
                  并且路由匹配的界面就显示在这里了 
              */}
              {element}
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}

```

```jsx
import React from "react";
import { NavLink, Outlet } from "react-router-dom";

export default function Home() {
  return (
    <div>
      <h2>Home组件内容</h2>
      <div>
        <ul className="nav nav-tabs">
          <li>
            {/* 注意， */}
            <NavLink className="list-group-item" to="news">
              News
            </NavLink>
          </li>
          <li>
            <NavLink className="list-group-item" to="message">
              Message
            </NavLink>
          </li>
        </ul>
        {/* 指定路由组件呈现的位置 */}
        <Outlet />
      </div>
    </div>
  );
}

```

<a name="JuRzn"></a>

## 3.Hooks

<a name="WF9Zf"></a>

### 1. useRoutes()

1. 作用：根据路由表，动态创建<Routes>和<Route>。
2. 示例代码：

```jsx
//路由表配置：src/routes/index.js
import About from '../pages/About'
import Home from '../pages/Home'
import {Navigate} from 'react-router-dom'

export default [
  {
    path:'/about',
    element:<About/>
  },
  {
    path:'/home',
    element:<Home/>
  },
  {
    path:'/',
    element:<Navigate to="/about"/>
  }
]

//App.jsx
import React from 'react'
import {NavLink,useRoutes} from 'react-router-dom'
import routes from './routes'

export default function App() {
  //根据路由表生成对应的路由规则
  const element = useRoutes(routes)
  return (
    <div>
      ......
      {/* 注册路由 */}
      {element}
      ......
    </div>
  )
}
```

真实项目使用：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689416404028-5c29434f-3487-4081-8c0e-f17d67e31d5d.png#averageHue=%2324221f&clientId=ubf169963-208c-4&from=paste&height=146&id=u29e50bef&originHeight=182&originWidth=235&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=9000&status=done&style=shadow&taskId=ub0026d24-c1bd-42c3-9a7b-3b5a5de4425&title=&width=188)

```jsx
import About from '../pages/About'
import Home from '../pages/Home'
import {Navigate} from 'react-router-dom'

export default [
	{
		path:'/about',
		element:<About/>
	},
	{
		path:'/home',
		element:<Home/>
	},
	{
		path:'/',
		element:<Navigate to="/about"/>
	}
]
```

```jsx
import React from 'react'
import {NavLink,useRoutes} from 'react-router-dom'
import routes from './routes'

export default function App() {
	//根据路由表生成对应的路由规则
	const element = useRoutes(routes)
	return (
		<div>
			<div className="row">
				<div className="col-xs-offset-2 col-xs-8">
					<div className="page-header"><h2>React Router Demo</h2></div>
				</div>
			</div>
			<div className="row">
				<div className="col-xs-2 col-xs-offset-2">
					<div className="list-group">
						{/* 路由链接 */}
						<NavLink className="list-group-item" to="/about">About</NavLink>
						<NavLink className="list-group-item" to="/home">Home</NavLink>
					</div>
				</div>
				<div className="col-xs-6">
					<div className="panel">
						<div className="panel-body">
							{/* 注册路由 */}
							{element}
						</div>
					</div>
				</div>
			</div>
		</div>
	)
}

```

<a name="lkAdO"></a>

### 2. useNavigate()

1. 作用：返回一个函数用来实现编程式导航。
2. 示例代码：

```jsx
import React, { useState } from "react";
import { Link, Outlet, useNavigate } from "react-router-dom";

export default function Message() {
  const navigate = useNavigate();
  const [messages] = useState([
    { id: "001", title: "消息1", content: "锄禾日当午" },
    { id: "002", title: "消息2", content: "汗滴禾下土" },
    { id: "003", title: "消息3", content: "谁知盘中餐" },
    { id: "004", title: "消息4", content: "粒粒皆辛苦" },
  ]);

  function showDetail(m) {
    navigate("detail", {
      //replace和push相对应
      replace: false,
      // 这里只能传输state参数，如果要传search和params参数，直接拼接路径就可以了
      state: {
        id: m.id,
        title: m.title,
        content: m.content,
      },
    });
  }
  return (
    <div>
      <ul>
        {messages.map((m) => {
      return (
        // 路由链接
        <li key={m.id}>
          <Link
            to="detail"
            state={{
              id: m.id,
              title: m.title,
              content: m.content,
            }}
            >
            {m.title}
          </Link>
          <button onClick={() => showDetail(m)}>查看详情</button>
        </li>
      );
    })}
      </ul>
      <hr />
      {/* 指定路由组件的展示位置 */}
      <Outlet />
    </div>
  );
}

```

```jsx
import React from "react";
import { useNavigate } from "react-router-dom";

export default function Header() {
  const navigate = useNavigate();

  function back() {
    // 可以实现后退
    navigate(-1);
  }
  function forward() {
    // 可以实现前进
    navigate(1);
  }

  return (
    <div className="col-xs-offset-2 col-xs-8">
      <div className="page-header">
        <h2>React Router Demo</h2>
        <button onClick={back}>←后退</button>
        <button onClick={forward}>前进→</button>
      </div>
    </div>
  );
}

```

<a name="deAN2"></a>

### 3. useParams()

1. 作用：回当前匹配路由的params参数，类似于5.x中的match.params。
2. 示例代码：

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689417672765-f4a4aaef-0954-4d64-8e83-88f38b3592f4.png#averageHue=%2322201e&clientId=ubf169963-208c-4&from=paste&height=215&id=u138cbb14&originHeight=269&originWidth=250&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=13073&status=done&style=shadow&taskId=u86c5bb5a-3d40-4741-8ddc-a273f423b97&title=&width=200)

```jsx
import About from "../pages/About";
import Home from "../pages/Home";
import Message from "../pages/Message";
import News from "../pages/News";
import Detail from "../pages/Detail";
import { Navigate } from "react-router-dom";

export default [
  {
    path: "/about",
    element: <About />,
  },
  {
    path: "/home",
    element: <Home />,
    children: [
      {
        path: "news",
        element: <News />,
      },
      {
        path: "message",
        element: <Message />,
        children: [
          {
            // 在路由里面传递参数
            path: "detail/:id/:title/:content",
            element: <Detail />,
          },
        ],
      },
    ],
  },
  {
    path: "/",
    element: <Navigate to="/about" />,
  },
];

```

```jsx
import React, { useState } from "react";
import { Link, Outlet } from "react-router-dom";

export default function Message() {
  const [messages] = useState([
    { id: "001", title: "消息1", content: "锄禾日当午" },
    { id: "002", title: "消息2", content: "汗滴禾下土" },
    { id: "003", title: "消息3", content: "谁知盘中餐" },
    { id: "004", title: "消息4", content: "粒粒皆辛苦" },
  ]);
  return (
    <div>
      <ul>
        {messages.map((m) => {
          return (
            // 路由链接
            <li key={m.id}>
              {/* 在这里进行传输参数 */}
              <Link to={`detail/${m.id}/${m.title}/${m.content}`}>
                {m.title}
              </Link>
            </li>
          );
        })}
      </ul>
      <hr />
      {/* 指定路由组件的展示位置 */}
      <Outlet />
    </div>
  );
}

```

```jsx
import React from "react";
import { useParams, useMatch } from "react-router-dom";

export default function Detail() {
  // 在这里进行接收参数
  const { id, title, content } = useParams();
  return (
    <ul>
      <li>消息编号：{id}</li>
      <li>消息标题：{title}</li>
      <li>消息内容：{content}</li>
    </ul>
  );
}

```

<a name="w9Inv"></a>

### 4. useSearchParams()

1. 作用：用于读取和修改当前位置的 URL 中的查询字符串。
2. 返回一个包含两个值的数组，内容分别为：当前的seaech参数、更新search的函数。
3. 示例代码：

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689419946872-080d453c-7283-40e7-8f0e-b88e808ba8de.png#averageHue=%2322201e&clientId=ubf169963-208c-4&from=paste&height=215&id=u7619b107&originHeight=269&originWidth=267&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=13666&status=done&style=shadow&taskId=u52c2c8d8-bbb8-44f5-987e-382b2f5697d&title=&width=213.6)

```jsx
import About from "../pages/About";
import Home from "../pages/Home";
import Message from "../pages/Message";
import News from "../pages/News";
import Detail from "../pages/Detail";
import { Navigate } from "react-router-dom";

export default [
  {
    path: "/about",
    element: <About />,
  },
  {
    path: "/home",
    element: <Home />,
    children: [
      {
        path: "news",
        element: <News />,
      },
      {
        path: "message",
        element: <Message />,
        children: [
          {
            // 这里就不需要加参数了
            path: "detail",
            element: <Detail />,
          },
        ],
      },
    ],
  },
  {
    path: "/",
    element: <Navigate to="/about" />,
  },
];

```

```jsx
import React, { useState } from "react";
import { Link, Outlet } from "react-router-dom";

export default function Message() {
  const [messages] = useState([
    { id: "001", title: "消息1", content: "锄禾日当午" },
    { id: "002", title: "消息2", content: "汗滴禾下土" },
    { id: "003", title: "消息3", content: "谁知盘中餐" },
    { id: "004", title: "消息4", content: "粒粒皆辛苦" },
  ]);
  return (
    <div>
      <ul>
        {messages.map((m) => {
          return (
            // 路由链接
            <li key={m.id}>
              {/* 用search方式传输参数 */}
              <Link
                to={`detail?id=${m.id}&title=${m.title}&content=${m.content}`}
              >
                {m.title}
              </Link>
            </li>
          );
        })}
      </ul>
      <hr />
      {/* 指定路由组件的展示位置 */}
      <Outlet />
    </div>
  );
}

```

```jsx
import React from 'react'
import {useSearchParams} from 'react-router-dom'

export default function Detail() {
	const [search,setSearch] = useSearchParams()
	const id = search.get('id')
	const title = search.get('title')
	const content = search.get('content')
	return (
		<ul>
			<li>消息编号：{id}</li>
			<li>消息标题：{title}</li>
			<li>消息内容：{content}</li>
		</ul>
	)
}

```

<a name="pnRqu"></a>

### 5. useLocation()

1. 作用：获取当前 location 信息，对标5.x中的路由组件的location属性。并且这里面可以获取state参数
2. 示例代码：

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689420182986-8cd5c14a-bad2-49c1-9908-e01b6b98a95f.png#averageHue=%2321201e&clientId=ubf169963-208c-4&from=paste&height=216&id=ufa75365c&originHeight=270&originWidth=284&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=13764&status=done&style=shadow&taskId=ucaa28283-43c6-4750-8f2f-dd6cc7f892e&title=&width=227.2)

```jsx
import About from "../pages/About";
import Home from "../pages/Home";
import Message from "../pages/Message";
import News from "../pages/News";
import Detail from "../pages/Detail";
import { Navigate } from "react-router-dom";

export default [
  {
    path: "/about",
    element: <About />,
  },
  {
    path: "/home",
    element: <Home />,
    children: [
      {
        path: "news",
        element: <News />,
      },
      {
        path: "message",
        element: <Message />,
        children: [
          {
            // 这里仍然不需要参数
            path: "detail",
            element: <Detail />,
          },
        ],
      },
    ],
  },
  {
    path: "/",
    element: <Navigate to="/about" />,
  },
];

```

```jsx
import React, { useState } from "react";
import { Link, Outlet } from "react-router-dom";

export default function Message() {
  const [messages] = useState([
    { id: "001", title: "消息1", content: "锄禾日当午" },
    { id: "002", title: "消息2", content: "汗滴禾下土" },
    { id: "003", title: "消息3", content: "谁知盘中餐" },
    { id: "004", title: "消息4", content: "粒粒皆辛苦" },
  ]);
  return (
    <div>
      <ul>
        {messages.map((m) => {
          return (
            // 路由链接
            <li key={m.id}>
              {/* 这里进行传输数据 */}
              <Link
                to="detail"
                state={{
                  id: m.id,
                  title: m.title,
                  content: m.content,
                }}
              >
                {m.title}
              </Link>
            </li>
          );
        })}
      </ul>
      <hr />
      {/* 指定路由组件的展示位置 */}
      <Outlet />
    </div>
  );
}

```

```jsx
import React from "react";
import { useLocation } from "react-router-dom";

export default function Detail() {
  // 通过useLocation进行接收数据
  //下面用了结构赋值的嵌套写法
  const {
    state: { id, title, content },
  } = useLocation();
  return (
    <ul>
      <li>消息编号：{id}</li>
      <li>消息标题：{title}</li>
      <li>消息内容：{content}</li>
    </ul>
  );
}

```

<a name="Xrkdh"></a>

### 6. useMatch()

1. 作用：返回当前匹配信息，对标5.x中的路由组件的match属性。
2. 示例代码：

```
<Route path="/login/:page/:pageSize" element={<Login />}/>
<NavLink to="/login/1/10">登录</NavLink>

export default function Login() {
  const match = useMatch('/login/:x/:y')
  console.log(match) //输出match对象
  //match对象内容如下：
  /*
    {
      params: {x: '1', y: '10'}
      pathname: "/LoGin/1/10"  
      pathnameBase: "/LoGin/1/10"
      pattern: {
        path: '/login/:x/:y', 
        caseSensitive: false, 
        end: false
      }
    }
  */
  return (
    <div>
      <h1>Login</h1>
    </div>
  )
}
```

<a name="MtDxQ"></a>

### 7. useInRouterContext()

作用：如果组件在 <Router> 的上下文中呈现，则 useInRouterContext 钩子返回 true，否则返回 false。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689421647793-073cdd9c-91b8-4bfd-b8c1-073098e8fb4f.png#averageHue=%2321201f&clientId=ubf169963-208c-4&from=paste&height=493&id=ue9c2544c&originHeight=616&originWidth=672&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=56323&status=done&style=shadow&taskId=u72c7a8ed-5132-40e3-821c-17c6a3d9d4e&title=&width=537.6)App及其子组件都会处于上下文环境中

```jsx
import React from "react";
import { useInRouterContext } from "react-router-dom";

export default function Demo() {
  // 这里会打印为false，因为没处于上下文环境中
  console.log(useInRouterContext());
  return <div>Demo</div>;
}

```

<a name="OUeAC"></a>

### 8. useNavigationType()

1. 作用：返回当前的导航类型（用户是如何来到当前页面的）。
2. 返回值：POP、PUSH、REPLACE。
3. 备注：POP是指在浏览器中直接打开了这个路由组件（刷新页面）。
   <a name="ci1Bz"></a>

### 9. useOutlet()

1. 作用：用来呈现当前组件中渲染的嵌套路由。
2. 示例代码：

```jsx
const result = useOutlet()
console.log(result)
// 如果嵌套路由没有挂载,则result为null
// 如果嵌套路由已经挂载,则展示嵌套的路由对象
```

<a name="b5I6N"></a>

### 10.useResolvedPath()

用来解析路径的。<br />作用：给定一个 URL值，解析其中的：path、search、hash值。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689421967314-bd9821de-5560-4108-8808-4704b7a0e64f.png#averageHue=%23302f2e&clientId=ubf169963-208c-4&from=paste&height=282&id=u1d0249f7&originHeight=353&originWidth=755&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=121203&status=done&style=shadow&taskId=u48e37432-962d-41a8-9748-7738880503d&title=&width=604)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689421978602-1183aca9-0c64-45b2-be29-3e2b4ef05e99.png#averageHue=%23f6f4f3&clientId=ubf169963-208c-4&from=paste&height=28&id=u67601fd8&originHeight=35&originWidth=518&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=17747&status=done&style=shadow&taskId=uce938475-bfdd-41cb-b53a-354b772e444&title=&width=414.4)<br /> 
