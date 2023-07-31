---
title: TypeScript
date: 2023-07-31 21:07:26
tags: TypeScript
---

<a name="ELD9x"></a>

# 1. TypeScript 介绍

<a name="NrigF"></a>

## 1.1 TypeScript 是什么

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689430579884-7e4c1040-97d0-4c60-ab29-4a16ed5209de.png#averageHue=%230c80cf&clientId=ud8812ea3-9cc4-4&from=paste&height=148&id=u5ed143ef&originHeight=295&originWidth=295&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=28543&status=done&style=shadow&taskId=uaf4733ae-00f4-4e1a-88aa-e65297143a1&title=&width=148)<br />TypeScript（简称：TS）是 JavaScript 的超集（JS 有的 TS 都有）。<br />TypeScript = Type + JavaScript（在 JS 基础之上，为 JS 添加了类型支持）。<br />TypeScript 是微软开发的开源编程语言，可以在任何运行 JavaScript 的地方运行。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689430598097-2e16a724-6bb4-4e30-a701-40e91c083118.png#averageHue=%23232222&clientId=ud8812ea3-9cc4-4&from=paste&height=165&id=ube1219fc&originHeight=330&originWidth=1264&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=162231&status=done&style=shadow&taskId=ue93da2e1-4514-441a-8f38-8dddf58fe4c&title=&width=632)![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689430614070-422a1683-94c5-4b75-94c0-5a6f49ce5558.png#averageHue=%232f84c0&clientId=ud8812ea3-9cc4-4&from=paste&height=253&id=uc91eb6e5&originHeight=506&originWidth=520&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=157843&status=done&style=shadow&taskId=u8fe5015d-fbfa-4f23-b8d8-c669f3c94e2&title=&width=260)
<a name="ePwFv"></a>

## 1.2 TypeScript 为什么要为 JS 添加类型支持？

背景：JS 的类型系统存在“先天缺陷”，JS 代码中绝大部分错误都是类型错误（Uncaught TypeError）。<br />问题：增加了找 Bug、改 Bug 的时间，严重影响开发效率。<br />从编程语言的动静来区分，TypeScript 属于静态类型的编程语言，JS 属于动态类型的编程语言。

- 静态类型：编译期做类型检查；（这一点和Java很像，Java也是编译期做类型检查）
- 动态类型：执行期做类型检查。

代码编译和代码执行的顺序：先编译，再执行。

- 对于 JS 来说：需要等到代码真正去执行的时候才能发现错误（晚）。
- 对于 TS 来说：在代码编译的时候（代码执行前）就可以发现错误（早）。

并且，配合 VSCode 等开发工具，TS 可以提前到在编写代码的同时就发现代码中的错误，减少找 Bug、改 Bug 时间。
<a name="vrlsn"></a>

## 1.3 TypeScript 相比 JS 的优势

1. 更早（写代码的同时）发现错误，减少找 Bug、改 Bug 时间，提升开发效率。
2. 程序中任何位置的代码都有代码提示，随时随地的安全感，增强了开发体验。
3. 强大的类型系统提升了代码的可维护性，使得重构代码更加容易。
4. 支持最新的 ECMAScript 语法，优先体验最新的语法，让你走在前端技术的最前沿。
5. TS 类型推断机制，不需要在代码中的每个地方都显示标注类型，让你在享受优势的同时，尽量降低了成本。

除此之外，Vue 3 源码使用 TS 重写、Angular 默认支持 TS、React 与 TS 完美配合，TypeScript 已成为大中型前端项目的首先编程语言。
<a name="YS2Kv"></a>

# 2. TypeScript 初体验

<a name="GBNwo"></a>

## 2.1 安装编译 TS 的工具包

问题：为什么要安装编译 TS 的工具包？<br />回答：Node.js/浏览器，只认识 JS 代码，不认识 TS 代码。需要先将 TS 代码转化为 JS 代码，然后才能运行。<br />安装命令：`npm i -g typescript`。<br />typescript 包：用来编译 TS 代码的包，提供了 tsc 命令，实现了 TS -> JS 的转化。<br />验证是否安装成功：`tsc –v`（查看 typescript 的版本）。
<a name="nn0x1"></a>

## 2.2 编译并运行 TS 代码

创建 hello.ts 文件（注意：TS 文件的后缀名为 .ts）。<br />将 TS 编译为 JS：在终端中输入命令，tsc hello.ts（此时，在同级目录中会出现一个同名的 JS 文件）。<br />执行 JS 代码：在终端中输入命令，node hello.js。

说明：所有合法的 JS 代码都是 TS 代码，有 JS 基础只需要学习 TS 的类型即可。<br />注意：由 TS 编译生成的 JS 文件，代码中就没有类型信息了。

> 但是有没有感觉这种方法有点麻烦，之后会解决

<a name="OcomX"></a>

## 2.3 简化运行 TS 的步骤

问题描述：每次修改代码后，都要重复执行两个命令，才能运行 TS 代码，太繁琐。<br />简化方式：使用 ts-node 包，直接在 Node.js 中执行 TS 代码。<br />安装命令：`npm i -g ts-node`（ts-node 包提供了 ts-node 命令）。<br />使用方式：`ts-node hello.ts`<br />解释：ts-node 命令在内部偷偷的将 TS -> JS，然后，再运行 JS 代码。（这个命令不会在目录里面生成js文件了）
<a name="Vg7sK"></a>

# 3. TypeScript 常用类型

<a name="JkXLJ"></a>

## 3.0 概述

TypeScript 是 JS 的超集，TS 提供了 JS 的所有功能，并且额外的增加了：类型系统。

- 所有的 JS 代码都是 TS 代码。
- JS 有类型（比如，number/string 等），但是 JS 不会检查变量的类型是否发生变化。而 TS 会检查。 TypeScript 类型系统的主要优势：可以显示标记出代码中的意外行为，从而降低了发生错误的可能性。
  <a name="ogp2p"></a>

## 3.1 类型注解

示例代码：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689431053230-473ba7e3-e512-43e1-ae85-387519c360f8.png#averageHue=%2352221b&clientId=ud8812ea3-9cc4-4&from=paste&height=124&id=u2cd53028&originHeight=124&originWidth=704&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=41440&status=done&style=shadow&taskId=u768ac8ee-2163-4278-aba1-d79f827130c&title=&width=704)<br />说明：代码中的 : number (注意这里带了冒号)就是类型注解。<br />作用：为变量添加类型约束。比如，上述代码中，约定变量 age 的类型为 number（数值类型）。<br />解释：约定了什么类型，就只能给变量赋值该类型的值，否则，就会报错。

或者是你声明的类型是number，但是你赋值为字符串，这也是不行的，就比如下面的<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689431107056-e8206765-8926-4298-bd60-db99b41bf030.png#averageHue=%232b3330&clientId=ud8812ea3-9cc4-4&from=paste&height=326&id=u3e47c9e3&originHeight=326&originWidth=1200&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=196224&status=done&style=shadow&taskId=u15fb3874-2f21-4713-9258-4526ed56507&title=&width=1200)
<a name="wr5Z6"></a>

## 3.2 常用基础类型概述

可以将 TS 中的常用基础类型细分为两类：

- JS 已有类型
  - 原始类型：number/string/boolean/null/undefined/symbol。
  - 对象类型：object（包括，数组、对象、函数等对象）。
- TS 新增类型
  - 联合类型、自定义类型（类型别名）、接口、元组、字面量类型、枚举、void、any 等。
    <a name="m77Hv"></a>

## 3.3 原始类型

原始类型：number/string/boolean/null/undefined/symbol。<br />特点：简单。这些类型，完全按照 JS 中类型的名称来书写。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689431172389-55ea80f3-395e-4649-89a3-08701edab4fb.png#averageHue=%2338352a&clientId=ud8812ea3-9cc4-4&from=paste&height=278&id=ue27a9093&originHeight=278&originWidth=358&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=79560&status=done&style=shadow&taskId=u5d957471-b03a-4273-b9f8-593e3d56ad1&title=&width=358)
<a name="R0o4s"></a>

## 3.4 数组类型

对象类型：object（包括，数组、对象、函数等对象）。<br />特点：对象类型，在 TS 中更加细化，每个具体的对象都有自己的类型语法。<br />数组类型的两种写法：（推荐使用 number[] 写法,也就是第一种）

```typescript
let numbers1: number[] = [1, 2, 3, 4];
let numbers2: Array<number> = [1, 2, 3, 4];
//上面这两种定义方法是一样的，但是推荐用最上面的一种
```

需求：数组中既有 number 类型，又有 string 类型，这个数组的类型应该如何写？

```typescript
//联合类型。
//首先这个是数组，其次这个数组里面能出现number或string类型的元素
let arr: (number | string)[] = [1, "a", 2, "b"];

//不添加小括号的话，表示下面的数组可以是string数组，或者是单个的数字
let arr1: number | string[] = ["a", "b"];
let arr2: number | string[] = 1;

```

解释：| （竖线）在 TS 中叫做联合类型（由两个或多个其他类型组成的类型，表示可以是这些类型中的任意一种）<br />注意：这是 TS 中联合类型的语法，只有一根竖线，不要与 JS 中的或（||）混淆了。
<a name="q4Hoo"></a>

## 3.5 类型别名

类型别名（自定义类型）：为任意类型起别名。<br />使用场景：当同一类型（复杂）被多次使用时，可以通过类型别名，简化该类型的使用。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689431579702-7de3ad56-4cd3-4e33-a861-be4f750636d8.png#averageHue=%23314124&clientId=ud8812ea3-9cc4-4&from=paste&height=222&id=u054a39da&originHeight=222&originWidth=1142&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=202040&status=done&style=shadow&taskId=uc016eeef-4f03-49ff-87e7-db20f003c06&title=&width=1142)<br />解释：

1. 使用 type 关键字来创建类型别名。
2. 类型别名（比如，此处的 CustomArray，名称可以自定义），可以是任意合法的变量名称。
3. 创建类型别名后，直接使用该类型别名作为变量的类型注解即可。
   <a name="eA8Fp"></a>

## 3.6 函数类型

函数的类型实际上指的是：函数参数和返回值的类型。<br />为函数指定类型的两种方式：

1. 单独指定参数、返回值的类型 
2. 同时指定参数、返回值的类型。

**单独指定参数、返回值的类型：**<br />普通函数<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689431666536-c98fcce5-bbc6-4f55-849d-1844c5f7e893.png#averageHue=%235a2d20&clientId=ud8812ea3-9cc4-4&from=paste&height=208&id=ud9423943&originHeight=208&originWidth=1236&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=125298&status=done&style=shadow&taskId=uc0eff055-46a4-4ed6-8c91-8a1169e1c93&title=&width=1236)<br />箭头函数<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689431675581-52cd693f-3908-4161-8caf-dedd9cb20f5a.png#averageHue=%235e2e20&clientId=ud8812ea3-9cc4-4&from=paste&height=202&id=ue9b6775b&originHeight=202&originWidth=1274&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=128590&status=done&style=shadow&taskId=u3f908a28-2ee2-4163-ace9-d71116b2134&title=&width=1274)<br />**同时指定参数、返回值的类型：**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689431720736-14120a05-e698-4339-9ace-90a2d81dae66.png#averageHue=%23272322&clientId=ud8812ea3-9cc4-4&from=paste&height=206&id=u023aa027&originHeight=206&originWidth=1642&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=152252&status=done&style=shadow&taskId=ucdd35851-bd20-4afe-91a0-507fbc9c3a9&title=&width=1642)<br />解释：当函数作为表达式时，可以通过类似箭头函数形式的语法来为函数添加类型。<br />注意：这种形式只适用于函数表达式（箭头函数）。

<a name="C3Qds"></a>

### void

如果函数没有返回值，那么，函数返回值类型为：void。（这个类型是ts新增的，js里面没有）<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689431760283-e2ac593a-ade4-4baf-aa7a-3ed12850422e.png#averageHue=%23473b26&clientId=ud8812ea3-9cc4-4&from=paste&height=194&id=u3ab41e51&originHeight=194&originWidth=894&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=120400&status=done&style=shadow&taskId=u96f96d65-d0dd-4ff2-a829-5e281c33cc6&title=&width=894)
<a name="nAn7A"></a>

### 可选参数

使用函数实现某个功能时，参数可以传也可以不传。这种情况下，在给函数参数指定类型时，就用到可选参数了。<br />比如，数组的 slice 方法，可以 slice() 也可以 slice(1) 还可以 slice(1, 3)。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689431823964-82a19ce9-3993-4bfb-ac84-7bb78166d165.png#averageHue=%235d4a29&clientId=ud8812ea3-9cc4-4&from=paste&height=208&id=u3be12b14&originHeight=208&originWidth=1310&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=194685&status=done&style=shadow&taskId=u5d21ef25-8652-4385-86e8-99ed20cea15&title=&width=1310)可选参数：在可传可不传的参数名称后面添加 ?（问号）。<br />注意：可选参数只能出现在参数列表的最后，也就是说可选参数后面不能再出现必选参数。
<a name="TEHFP"></a>

## 3.7 对象类型

JS 中的对象是由属性和方法构成的，而 TS 中对象的类型就是在描述对象的结构（有什么类型的属性和方法）。<br />对象类型的写法：

```typescript
let person: {
  name: string;
  age: number;
  sayHi(): void;
  sayHello(name: string): void;
} = {
  name: "胥天昊",
  age: 18,
  sayHi() {
    console.log("Hi");
  },
  sayHello(name) {
    console.log("Hello" + name);
  },
};
```

解释：

- 直接使用 {} 来描述对象结构。属性采用属性名: 类型的形式；方法采用方法名(): 返回值类型的形式。
- 如果方法有参数，就在方法名后面的小括号中指定参数类型（比如：greet(name: string): void。这里跟之前的函数类型是一致的）。
- 在一行代码中指定对象的多个属性类型时，使用 ;（分号）来分隔。
  - 如果一行代码只指定一个属性类型（通过换行来分隔多个属性类型），可以去掉 ;（分号）。
  - 方法的类型也可以使用箭头函数形式（比如：{ sayHi: () => void }）。

对象的属性或方法，也可以是可选的，此时就用到可选属性了。<br />比如，我们在使用 axios({ … })  （里面是个配置对象，但是这个对象里面有的属性是可以省略的）时，如果发送 GET 请求，method 属性就可以省略。

```typescript
function myAxios(config: { url: string; method?: string }) {}
myAxios({
  url: "",
});
```

可选属性的语法与函数可选参数的语法一致，都使用 ?（问号）来表示。
<a name="U4toq"></a>

## 3.8 接口

当一个对象类型被多次使用时，一般会使用接口（interface）来描述对象的类型，达到复用的目的。解释：

1. 使用 interface 关键字来声明接口。
2. 接口名称（比如，此处的 IPerson），可以是任意合法的变量名称。
3. 声明接口后，直接使用接口名称作为变量的类型。
4. 因为每一行只有一个属性类型，因此，属性类型后没有 ;（分号）。

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689432272787-0af9978d-3073-4d6f-b7f5-aaba57a392b8.png#averageHue=%23382c20&clientId=ud8812ea3-9cc4-4&from=paste&height=646&id=u5ee36bd1&originHeight=646&originWidth=562&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=203416&status=done&style=shadow&taskId=udcb3249a-5c97-4efe-b05b-1ff28d05958&title=&width=562)<br />interface（接口）和 type（类型别名）的对比：

- 相同点：都可以给对象指定类型。
- 不同点：
  - 接口，只能为对象指定类型。
  - 类型别名，不仅可以为对象指定类型，实际上可以为任意类型指定别名

总的来说type要比interface功能要多，更NB。interface能干的type也能干。

如果两个接口之间有相同的属性或方法，可以将公共的属性或方法抽离出来，通过继承来实现复用。比如，这两个接口都有 x、y 两个属性，重复写两次，可以，但很繁琐。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689432347332-c8642a4c-9723-4db8-87cb-5b02f0db3eb0.png#averageHue=%2321221d&clientId=ud8812ea3-9cc4-4&from=paste&height=148&id=u49c8eade&originHeight=148&originWidth=1298&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=168935&status=done&style=shadow&taskId=u438f83f0-ba85-4996-a5df-d9366b053ca&title=&width=1298)<br />下面是更好的方式：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689432359779-419502bf-1081-4b68-ab16-c07eaacbbf99.png#averageHue=%2321231d&clientId=ud8812ea3-9cc4-4&from=paste&height=160&id=u9c12d288&originHeight=160&originWidth=1178&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=175638&status=done&style=shadow&taskId=u67b24495-da16-4b5f-8167-c75065cfd7c&title=&width=1178)<br />解释：

1. 使用 extends（继承）关键字实现了接口 Point3D 继承 Point2D。
2. 继承后，Point3D 就有了 Point2D 的所有属性和方法（此时，Point3D 同时有 x、y、z 三个属性）。
   <a name="iSqOf"></a>

## 3.9 元组

场景：在地图中，使用经纬度坐标来标记位置信息。<br />可以使用数组来记录坐标，那么，该数组中只有两个元素，并且这两个元素都是数值类型。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689432382302-074e2b47-bc2c-404f-a496-0f018ccda0d8.png#averageHue=%2322221e&clientId=ud8812ea3-9cc4-4&from=paste&height=110&id=u765746da&originHeight=110&originWidth=1064&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=74505&status=done&style=shadow&taskId=ue13f7109-f894-495e-a904-005cdcb90f0&title=&width=1064)<br />使用 number[] 的缺点：不严谨，因为该类型的数组中可以出现任意多个数字。<br />更好的方式：元组（Tuple）。<br />元组类型是另一种类型的数组，它确切地知道包含多少个元素，以及特定索引对应的类型。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689432391630-39080ad8-89fe-4c17-bf70-583848e95f3e.png#averageHue=%2321221e&clientId=ud8812ea3-9cc4-4&from=paste&height=106&id=u99deaebe&originHeight=106&originWidth=1252&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=99427&status=done&style=shadow&taskId=ubba04a19-3f37-4ba9-b0dc-6299d5b22d9&title=&width=1252)<br />解释：

1. 元组类型可以确切地标记出有多少个元素，以及每个元素的类型。
2. 该示例中，元素有两个元素，每个元素的类型都是 number（也可以换类型）。
   <a name="dCTna"></a>

## 3.10 类型推论

在 TS 中，某些没有明确指出类型的地方，TS 的类型推论机制会帮助提供类型。<br />换句话说：由于类型推论的存在，这些地方，类型注解可以省略不写！<br />发生**类型推论**的 2 种常见场景：

1.  声明变量并初始化时（注意这里如果声明变量没有立即初始化的话，仍然需要手动添加类型注解）	
2.  决定函数返回值时。

**声明变量并初始化时**<br />下面的变量会自动推断出变量的类型是number<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689432459356-ddf3303a-c031-4992-b65d-9d0a046218c4.png#averageHue=%232a241e&clientId=ud8812ea3-9cc4-4&from=paste&height=220&id=u68e70273&originHeight=220&originWidth=1296&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=187847&status=done&style=shadow&taskId=u308867f8-d2cd-4409-ac10-73487436168&title=&width=1296)<br />**决定函数返回值时**<br />下面的函数会自动推断出函数返回值是number<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689432500441-e4161669-0b18-44ae-9bd3-6fbcfa8a6d39.png#averageHue=%232c4234&clientId=ud8812ea3-9cc4-4&from=paste&height=154&id=u09085799&originHeight=154&originWidth=1466&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=193697&status=done&style=shadow&taskId=u87b5a830-b158-45c3-9c6f-399ed21de03&title=&width=1466)

注意：这两种情况下，类型注解可以省略不写！<br />推荐：能省略类型注解的地方就省略（偷懒，充分利用TS类型推论的能力，提升开发效率）。<br />技巧：如果不知道类型，可以通过鼠标放在变量名称上，利用 VSCode 的提示来查看类型。
<a name="XqLsL"></a>

## 3.11 类型断言

有时候你会比 TS 更加明确一个值的类型，此时，可以使用类型断言来指定更具体的类型（其实就是强制类型转换）。<br />比如，下面是一个超链接<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689463013960-2a3e6297-178c-4a35-8a9a-07313b936ab3.png#averageHue=%2323261d&clientId=ufd44ac5e-30bf-4&from=paste&height=64&id=u715c5437&originHeight=64&originWidth=1260&originalType=binary&ratio=1&rotation=0&showTitle=false&size=100289&status=done&style=shadow&taskId=uf95f06cc-59a1-463d-8ddf-6c02584d9ac&title=&width=1260)<br />默认的返回值是HTMLElement![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689463068851-e6212e3f-4f34-4e35-9743-626b880aaa65.png#averageHue=%23313a2a&clientId=ufd44ac5e-30bf-4&from=paste&height=158&id=u53e577cb&originHeight=158&originWidth=1084&originalType=binary&ratio=1&rotation=0&showTitle=false&size=141087&status=done&style=shadow&taskId=u99d59a0f-d2dc-44c9-8710-ad168e92ad2&title=&width=1084)<br />注意：getElementById 方法返回值的类型是 HTMLElement，该类型只包含所有标签公共的属性或方法，不包含 a标签特有的 href 等属性。<br />因此，这个类型太宽泛（不具体），无法操作 href 等 a 标签特有的属性或方法。<br />解决方式：这种情况下就需要使用类型断言指定更加具体的类型。<br />使用类型断言：<br />下面的HTMLAnchorElement是a标签的具体的类型，里面有a标签的属性，比如说href属性。。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689463137905-153e1c40-e10a-4670-9591-2a5dfc2c59fb.png#averageHue=%23314b2d&clientId=ufd44ac5e-30bf-4&from=paste&height=160&id=u95b3f554&originHeight=160&originWidth=1582&originalType=binary&ratio=1&rotation=0&showTitle=false&size=204879&status=done&style=shadow&taskId=u44cf625d-fd76-4e60-bd03-d63e5f6f00e&title=&width=1582)<br />解释：

1. 使用 as 关键字实现类型断言。
2. 关键字 as 后面的类型是一个更加具体的类型（HTMLAnchorElement 是 HTMLElement 的子类型）。
3. 通过类型断言，aLink 的类型变得更加具体，这样就可以访问 a 标签特有的属性或方法了。

另一种语法，使用 <> 语法，这种语法形式不常用，知道即可（好像那个强制类型转换）<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689463187326-4c0f82f5-a6fe-42be-aec6-908f02ba7bb3.png#averageHue=%232e3021&clientId=ufd44ac5e-30bf-4&from=paste&height=82&id=ud125181b&originHeight=82&originWidth=1528&originalType=binary&ratio=1&rotation=0&showTitle=false&size=132404&status=done&style=shadow&taskId=u030ff569-42af-4f10-bf45-fe79c6aeade&title=&width=1528)<br />那么我们如何知道这个标签的子类型是什么呢？？？技巧：用浏览器的查看选中你要查看的元素，在浏览器控制台，通过 `console.dir($0)` 打印 DOM 元素，打印出来的一个对象往下面拉，在属性列表的最后面的`prototype`里面，即可看到该元素的类型。
<a name="IP70D"></a>

## 3.12 字面量类型

思考以下代码，两个变量的类型分别是什么？<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689472579203-1f88e89a-b46d-451c-8791-0efae8e55718.png#averageHue=%23554f2b&clientId=ufd44ac5e-30bf-4&from=paste&height=94&id=u685515a0&originHeight=94&originWidth=456&originalType=binary&ratio=1&rotation=0&showTitle=false&size=42228&status=done&style=shadow&taskId=uf7ad42cf-204c-428e-b8f6-96e22b3125b&title=&width=456)<br />通过 TS 类型推论机制，可以得到答案：

1. 变量 str1 的类型为：string。
2. 变量 str2 的类型为：'Hello TS'。(这个就很奇怪了)

解释：

1. str1 是一个变量（let），它的值可以是任意字符串，所以类型为：string。
2. str2 是一个常量（const），它的值不能变化，只能是 'Hello TS'，所以，它的类型为：'Hello TS'。

注意：**此处的 'Hello TS'，就是一个字面量类型**。也就是说某个特定的字符串也可以作为 TS 中的类型。 除字符串外，任意的 JS 字面量（比如，对象、数字等）都可以作为类型使用。

```typescript
const str1 = "helloTs";
const str2: "helloTs" = "helloTs";

//上面这两个类型是一样的
console.log(typeof str1 === typeof str2); //true
```

还可以用数字当类型<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689472850618-778e3524-9055-414c-97f7-5117370d1cbd.png#averageHue=%232c2222&clientId=ufd44ac5e-30bf-4&from=paste&height=76&id=ub7d0c2e8&originHeight=76&originWidth=600&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10481&status=done&style=shadow&taskId=ud83821ea-e3ff-495c-88e3-04552b2bf44&title=&width=600)。类型为18，值也一定得是18

使用模式：字面量类型配合联合类型一起使用。 <br />使用场景：用来表示一组明确的可选值列表。<br />比如，在贪吃蛇游戏中，游戏的方向的可选值只能是上、下、左、右中的任意一个。

```typescript
// 这里的意思是dir的类型是后面的四个的其中一种，要不然就会报错，这就意味着值也必须是后面的其中一种
function changeDir(dir: "up" | "down" | "left" | "right") {}

changeDir("left");
```

解释：参数 direction 的值只能是 up/down/left/right 中的任意一个。<br />优势：相比于 string 类型，使用字面量类型更加精确、严谨。
<a name="MxVC8"></a>

## 3.13 枚举

枚举的功能类似于字面量类型+联合类型组合的功能，也可以表示一组明确的可选值。<br />枚举：定义一组命名常量。它描述一个值，该值可以是这些命名常量中的一个。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689473158067-5c9f0cc6-d099-4e39-a5e5-4b445d30ab4e.png#averageHue=%23322a22&clientId=ufd44ac5e-30bf-4&from=paste&height=203&id=waD6L&originHeight=203&originWidth=783&originalType=binary&ratio=1&rotation=0&showTitle=false&size=145029&status=done&style=shadow&taskId=uf1f79f5a-4a6e-4022-8303-371de94881d&title=&width=783)<br />解释：

1. 使用 enum 关键字定义枚举。
2. 约定枚举名称、枚举中的值以大写字母开头。
3. 枚举中的多个值之间通过 ,（逗号）分隔。
4. 定义好枚举后，直接使用枚举名称作为类型注解。

注意：形参 direction 的类型为枚举 Direction，那么，实参的值就应该是枚举 Direction 成员的任意一个。<br />访问枚举类型的值是通过点语法直接进行访问

枚举类型的值和Java中的一样，默认都是从0开始，之后依次递增<br />当然也可以进行手动初始化<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689473257822-e488421b-1f0c-45c2-8234-d4bf861aee84.png#averageHue=%2321221f&clientId=ufd44ac5e-30bf-4&from=paste&height=87&id=u25baf30e&originHeight=87&originWidth=738&originalType=binary&ratio=1&rotation=0&showTitle=false&size=58134&status=done&style=shadow&taskId=u8eed5ee8-b963-4e06-ae79-4b879e1ff2c&title=&width=738)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689473260739-bbad262f-9bf4-4882-a913-b10a4d447330.png#averageHue=%2321221f&clientId=ufd44ac5e-30bf-4&from=paste&height=49&id=u3adcc2cd&originHeight=49&originWidth=909&originalType=binary&ratio=1&rotation=0&showTitle=false&size=46713&status=done&style=shadow&taskId=uea523bd4-d30b-4072-952b-322aa72a4ba&title=&width=909)

也可以进行 字符串枚举：枚举成员的值是字符串。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689473294557-2d049066-2e0a-46ce-b91d-2e9b06b82a1b.png#averageHue=%23524d2a&clientId=ufd44ac5e-30bf-4&from=paste&height=235&id=uc07dca0b&originHeight=235&originWidth=374&originalType=binary&ratio=1&rotation=0&showTitle=false&size=66930&status=done&style=shadow&taskId=ud0414150-5e33-4e4c-b80b-fb0daf01752&title=&width=374)<br />注意：字符串枚举没有自增长行为，**只有数字才有自增长行为**，因此，字符串枚举的每个成员必须有**初始值。**


枚举是 TS 为数不多的非 JavaScript 类型级扩展（不仅仅是类型）的特性之一。<br />因为：其他类型仅仅被当做类型，而枚举不仅用作类型，还提供值（枚举成员都是有值的）。 也就是说，其他的类型会在编译为 JS 代码时自动移除。但是，枚举类型会被编译为 JS 代码！<br />![左边的是TS代码，右边是转化成的JS代码（Direction是一个对象    Up为属性 字符串UP为属性值）](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689473357226-1f509169-ee28-46d8-8ea7-0e4b82d4a74e.png#averageHue=%234f4729&clientId=ufd44ac5e-30bf-4&from=paste&height=274&id=u352521c2&originHeight=274&originWidth=1075&originalType=binary&ratio=1&rotation=0&showTitle=true&size=238305&status=done&style=shadow&taskId=u7fd299fd-a0bd-4187-bc98-54188d37e3f&title=%E5%B7%A6%E8%BE%B9%E7%9A%84%E6%98%AFTS%E4%BB%A3%E7%A0%81%EF%BC%8C%E5%8F%B3%E8%BE%B9%E6%98%AF%E8%BD%AC%E5%8C%96%E6%88%90%E7%9A%84JS%E4%BB%A3%E7%A0%81%EF%BC%88Direction%E6%98%AF%E4%B8%80%E4%B8%AA%E5%AF%B9%E8%B1%A1%C2%A0%20%C2%A0%20Up%E4%B8%BA%E5%B1%9E%E6%80%A7%20%E5%AD%97%E7%AC%A6%E4%B8%B2UP%E4%B8%BA%E5%B1%9E%E6%80%A7%E5%80%BC%EF%BC%89&width=1075 "左边的是TS代码，右边是转化成的JS代码（Direction是一个对象    Up为属性 字符串UP为属性值）")<br />说明：枚举与前面讲到的字面量类型+联合类型组合的功能类似，都用来表示一组明确的可选值列表。<br />一般情况下，推荐使用字面量类型+联合类型组合的方式，因为相比枚举，这种方式更加直观、简洁、高效。
<a name="g30Tj"></a>

## 3.14 any 类型

原则：不推荐使用 any！这会让 TypeScript 变为 “AnyScript”（失去 TS 类型保护的优势）。<br />因为当值的类型为 any 时，可以对该值进行任意操作，并且不会有代码提示。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689473485108-32110af6-33b0-4550-90db-5608bed33ed6.png#averageHue=%2323221e&clientId=ufd44ac5e-30bf-4&from=paste&height=209&id=u80ecb5fc&originHeight=209&originWidth=404&originalType=binary&ratio=1&rotation=0&showTitle=false&size=59834&status=done&style=shadow&taskId=u928cb2ca-50fe-4b90-aa0d-7a320c7d96e&title=&width=404)<br />解释：以上操作都不会有任何类型错误提示，即使可能存在错误！<br />尽可能的避免使用 any 类型，除非临时使用 any 来“避免”书写很长、很复杂的类型！<br />其他隐式具有 any 类型的情况：

1. 声明变量不提供类型也不提供默认值 ![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689473601339-067e3d77-fdf9-41b1-99e1-a372883619c4.png#averageHue=%23242424&clientId=ufd44ac5e-30bf-4&from=paste&height=141&id=u62b6c08a&originHeight=141&originWidth=808&originalType=binary&ratio=1&rotation=0&showTitle=false&size=17702&status=done&style=shadow&taskId=uc363fffe-c780-49ec-adab-662c73e4470&title=&width=808)
2. 函数参数不加类型。 ![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689473613908-2e3344da-2fe2-49c3-a9a3-b1d76f91d37d.png#averageHue=%23252525&clientId=ufd44ac5e-30bf-4&from=paste&height=172&id=uf6508654&originHeight=172&originWidth=934&originalType=binary&ratio=1&rotation=0&showTitle=false&size=28612&status=done&style=shadow&taskId=u00e98f73-8c36-4348-9bb3-533b4395693&title=&width=934)

注意：因为不推荐使用 any，所以，这两种情况下都应该提供类型！
<a name="N7oYa"></a>

## 3.15 typeof

typeof 操作符：可以在类型上下文中引用变量或属性的类型（类型查询）。<br />使用场景：根据已有变量的值，获取该值的类型，来简化类型书写。

```typescript
let p = { x: 1, y: 2 };
function funcPoint1(point: { x: number; y: number }) {}
function funcPoint2(point: typeof p) {}
//上面这两种定义方法是等价的
```

解释：

1. 使用 typeof 操作符来获取变量 p 的类型，结果与第一种（对象字面量形式的类型）相同。
2. typeof 出现在类型注解的位置（参数名称的冒号后面）所处的环境就在类型上下文（区别于 JS 代码）。
3. 注意：typeof 只能用来查询变量或属性的类型，无法查询其他形式的类型（比如，函数调用的类型 `typeof funcPoint1`）。
   <a name="LlU71"></a>

# 4. TypeScript 高级类型

<a name="l1hDT"></a>

## 4.1 class 类

TypeScript 全面支持 ES2015 中引入的 class 关键字，并为其添加了类型注解和其他语法（比如，可见性修饰符等）![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689473912265-7f19575f-f028-489c-9c7b-fb8452e82e0d.png#averageHue=%23232621&clientId=ufd44ac5e-30bf-4&from=paste&height=159&id=u7bfac549&originHeight=159&originWidth=422&originalType=binary&ratio=1&rotation=0&showTitle=false&size=57078&status=done&style=shadow&taskId=u2bca8a01-aca7-4fee-9ace-62038307121&title=&width=422)<br />解释：

1. 根据 TS 中的类型推论，可以知道 Person 类的实例对象 p 的类型是 Person。
2. TS 中的 class，不仅提供了 class 的语法功能，也作为一种类型存在。


实例属性初始化：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689473943142-d1cea1fa-2cb8-420c-ba78-a70901c51a48.png#averageHue=%23593122&clientId=ufd44ac5e-30bf-4&from=paste&height=199&id=ufc1e1d51&originHeight=199&originWidth=428&originalType=binary&ratio=1&rotation=0&showTitle=false&size=58599&status=done&style=shadow&taskId=u969da2a9-850b-4a95-8235-141fb296733&title=&width=428)<br />解释：

1. 声明成员 age，类型为 number（没有初始值）。
2. 声明成员 gender，并设置初始值，此时，可省略类型注解（TS 会根据**类型推论**为 string 类型）。

构造函数：

```typescript
class Person {
  name: string;
  age: number;
  // 构造函数
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}
// 创建对象
const p = new Person("胥天昊", 18);
// 访问属性
console.log(p.name);
console.log(p.age);

```

解释：

1. 成员初始化（比如，age: number）后，才可以通过 this.age 来访问实例成员。
2. 需要为构造函数指定类型注解，否则会被隐式推断为 any；构造函数不需要返回值类型。

实例方法：

```typescript
class Person {
  sayHello(name: string) {
    console.log("Hello" + name);
  }
}
// 创建对象
const p = new Person();
p.sayHello("胥天昊");

```

解释：方法的类型注解（参数和返回值）与函数用法相同。

类继承的两种方式：

1.  extends（继承父类） 
2.  implements（实现接口）。

 说明：JS 中只有 extends，而 implements 是 TS 提供的。

extends<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689474303390-fdc6868f-fefb-4fa3-8771-f6d4e772fc9a.png#averageHue=%233f3c26&clientId=ufd44ac5e-30bf-4&from=paste&height=277&id=ue8144aad&originHeight=277&originWidth=651&originalType=binary&ratio=1&rotation=0&showTitle=false&size=138344&status=done&style=shadow&taskId=u06583ce1-7edb-46e7-af4c-369bc5e840c&title=&width=651)<br />解释：

1. 通过 extends 关键字实现继承。
2. 子类 Dog 继承父类 Animal，则 Dog 的实例对象 dog 就同时具有了父类 Animal 和 子类 Dog 的所有属性和方法。

implements

```typescript
interface Say {
  sayHello(name: string): void;
}
// 实现接口
class Person implements Say {
  sayHello(name: string) {
    console.log("Hello" + name);
  }
}
// 创建对象
const p = new Person();
p.sayHello("胥天昊");

```

解释：

1. 通过 implements 关键字让 class 实现接口。
2. Person 类实现接口 Say 意味着，Person 类中必须提供 Say 接口中指定的所有方法和属性。


类成员可见性：可以使用 TS 来控制 class 的方法或属性对于 class 外的代码是否可见。<br />可见性修饰符包括：

1.  public（公有的） 
2.  protected（受保护的） 
3.  private（私有的）。

public：表示公有的、公开的，公有成员可以被任何地方访问，**默认可见性**。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689474513786-907ac987-7e91-4cae-a9c2-e82e1fb5c9b8.png#averageHue=%23252422&clientId=ufd44ac5e-30bf-4&from=paste&height=202&id=uc6749852&originHeight=202&originWidth=526&originalType=binary&ratio=1&rotation=0&showTitle=false&size=65770&status=done&style=shadow&taskId=u95ae22b0-60fc-4fb0-b251-bfae5d092af&title=&width=526)<br />解释：

1. 在类属性或方法前面添加 public 关键字，来修饰该属性或方法是共有的。
2. 因为 public 是默认可见性，所以，可以直接省略。

protected：表示受保护的，仅对其声明所在类和子类中（非实例对象）可见。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689474778923-d561e8d2-3665-4db3-bb16-95948dfae324.png#averageHue=%2322201f&clientId=ufd44ac5e-30bf-4&from=paste&height=462&id=u7be28654&originHeight=462&originWidth=869&originalType=binary&ratio=1&rotation=0&showTitle=false&size=49535&status=done&style=shadow&taskId=u18ee1248-fafc-483b-b675-c71277c12d6&title=&width=869)<br />解释：

1. 在类属性或方法前面添加 protected 关键字，来修饰该属性或方法是受保护的。
2. 在子类的方法内部可以通过 this 来访问父类中受保护的成员，但是，对实例不可见！


private：表示私有的，只在当前类中可见，对实例对象以及子类也是不可见的。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689474798415-7cfbad7b-7c4c-4651-b083-f984d018fdc7.png#averageHue=%23242322&clientId=ufd44ac5e-30bf-4&from=paste&height=235&id=uf469da81&originHeight=235&originWidth=770&originalType=binary&ratio=1&rotation=0&showTitle=false&size=88464&status=done&style=shadow&taskId=ube68ed42-2a91-4803-b983-6e05868eceb&title=&width=770)<br />解释：

1. 在类属性或方法前面添加 private 关键字，来修饰该属性或方法是私有的。
2. 私有的属性或方法只在当前类中可见，对子类和实例对象也都是不可见的！


除了可见性修饰符之外，还有一个常见修饰符就是：readonly（只读修饰符）。  <br />readonly：表示只读，用来**防止在构造函数之外对属性进行赋值**（不过这个属性可以赋默认值）。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689474841370-ec7d98f0-16e1-4680-8a4f-cb7eea3b65db.png#averageHue=%23322b22&clientId=ufd44ac5e-30bf-4&from=paste&height=234&id=u2b6e16e1&originHeight=234&originWidth=459&originalType=binary&ratio=1&rotation=0&showTitle=false&size=86907&status=done&style=shadow&taskId=u0f23d37f-a8a9-4f35-95ae-a565e39de9b&title=&width=459)<br />解释：

1. 使用 readonly 关键字修饰该属性是只读的，注意只能修饰属性不能修饰方法。
2. 注意：属性 age 后面的类型注解（比如，此处的 number）如果不加，则 age 的类型为 18 （字面量类型，和const定义的变量差不多）。
3. 接口或者 {} 表示的对象类型，也可以使用 readonly。
4. 只要是readonly修饰的属性，必须手动提供明确的类型，要不然类型就是any了
   <a name="pjJnV"></a>

## 4.2 类型兼容性

两种类型系统：

1.  Structural Type System（结构化类型系统） 
2.  Nominal Type System（标明类型系统）。 

TS 采用的是结构化类型系统，也叫做 duck typing（鸭子类型），类型检查关注的是**值所具有的形状**。<br />也就是说，在结构类型系统中，如果两个对象具有相同的形状，则认为它们属于同一类型。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689474959235-1b46eed1-9d3a-415e-a080-f75cfe09fa21.png#averageHue=%232d2a20&clientId=ufd44ac5e-30bf-4&from=paste&height=162&id=u84d19371&originHeight=162&originWidth=606&originalType=binary&ratio=1&rotation=0&showTitle=false&size=105099&status=done&style=shadow&taskId=u91dd170f-50a9-4f59-8919-2b08ff64483&title=&width=606)<br />解释：

1. Point 和 Point2D 是两个名称不同的类。
2. 变量 p 的类型被显示标注为 Point 类型，但是，它的值却是 Point2D 的实例，并且没有类型错误。
3. 因为 TS 是结构化类型系统，只检查 Point 和 Point2D 的结构是否相同（结构是指**所包含的属性名称和属性类型**）。
4. 但是，如果在 Nominal Type System 中（比如，C#、Java 等），它们是不同的类，类型无法兼容。

更准确的说法：对于对象类型来说，y 的成员至少与 x 相同，则 x 兼容 y（成员多的可以赋值给少的，提供的比要求的要多，肯定可以呀）。
<a name="R8ail"></a>

### class之间兼容性

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689475076806-f3a6e981-61a1-4084-8441-94b3372d8bbf.png#averageHue=%2320241f&clientId=ufd44ac5e-30bf-4&from=paste&height=119&id=u9523a38a&originHeight=119&originWidth=789&originalType=binary&ratio=1&rotation=0&showTitle=false&size=116812&status=done&style=shadow&taskId=u31ea02a8-6901-4a9d-87c9-596c504f313&title=&width=789)<br />解释：

1. Point3D 的成员至少与 Point 相同，则 Point 兼容 Point3D。
2. 所以，成员多的 Point3D 可以赋值给成员少的 Point。

除了 class 之外，TS 中的其他类型也存在相互兼容的情况，包括：

1.  接口兼容性 
2.  函数兼容性 
    <a name="zsDGt"></a>

### 接口之间兼容性

接口之间的兼容性，类似于 class。并且，class 和 interface 之间也可以兼容。

```typescript
interface Point2D {
  x: number;
  y: number;
}
class Point3D {
  x: number;
  y: number;
  z: number;
}
let p1: Point2D;
let p2 = new Point3D();

// 把一个属性多的赋给属性少的
p1 = p2;

```

<a name="gNTBI"></a>

### 函数之间兼容性

函数之间兼容性比较复杂，需要考虑：

1. 参数个数 
2. 参数类型 
3. 返回值类型。

**参数个数**，参数多的兼容参数少的（或者说，参数少的可以赋值给多的，和对象刚好相反）。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689475515820-461e2ff7-5381-467a-abc8-ebc8d3c3f3c9.png#averageHue=%23342e22&clientId=ufd44ac5e-30bf-4&from=paste&height=158&id=u0b5619c1&originHeight=158&originWidth=1090&originalType=binary&ratio=1&rotation=0&showTitle=false&size=163137&status=done&style=shadow&taskId=u108b0a7b-5258-47d4-8f2f-80166dfb581&title=&width=1090)<br />解释：

1. 参数少的可以赋值给参数多的，所以，f1 可以赋值给 f2。f2不能赋值给f1
2. 数组 forEach 方法的第一个参数是回调函数，该示例中类型为：(value: string, index: number, array: string[]) => void。
3. 在 JS 中省略用不到的函数参数实际上是很常见的，这样的使用方式，促成了 TS 中函数类型之间的兼容性。
4. 并且因为回调函数是有类型的，所以，TS 会自动推导出参数 item、index、array 的类型。

**参数类型，**相同位置的参数类型要相同（原始类型）或兼容（对象类型）。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689475855337-96411e25-e7dd-447a-a2cb-a5100a09eb18.png#averageHue=%2321221e&clientId=ufd44ac5e-30bf-4&from=paste&height=165&id=u4ce1c157&originHeight=165&originWidth=507&originalType=binary&ratio=1&rotation=0&showTitle=false&size=91238&status=done&style=shadow&taskId=ue9dadfea-d6e7-4025-a620-ed2dd7dbbae&title=&width=507)<br />解释：函数类型 F2 兼容函数类型 F1，因为 F1 和 F2 的第一个参数类型相同。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689475876257-98c46320-65a1-4ddc-a5e6-aec2aec10c77.png#averageHue=%2328251f&clientId=ufd44ac5e-30bf-4&from=paste&height=302&id=u42a947a5&originHeight=302&originWidth=831&originalType=binary&ratio=1&rotation=0&showTitle=false&size=198378&status=done&style=shadow&taskId=u52d97187-6b01-4348-aa06-3af37862f90&title=&width=831)<br />解释：

1. 注意，此处与前面讲到的接口兼容性冲突。
2. 技巧：将对象拆开，把每个属性看做一个个参数，则，参数少的（f2）可以赋值给参数多的（f3）。

**返回值类型**，只关注返回值类型本身即可：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689475930596-9dd9f435-8650-4f8c-a852-e74f9844e0a2.png#averageHue=%2322221e&clientId=ufd44ac5e-30bf-4&from=paste&height=194&id=u82d60e8a&originHeight=194&originWidth=1102&originalType=binary&ratio=1&rotation=0&showTitle=false&size=175125&status=done&style=shadow&taskId=u087d03d1-0768-425c-be28-ff954b151e3&title=&width=1102)<br />解释：

1. 如果返回值类型是**原始类型**，此时两个类型要**相同**，比如，左侧类型 F5 和 F6。
2. 如果返回值类型是**对象类型**，此时**成员多的可以赋值给成员少的**，比如，右侧类型 F7 和 F8。
   <a name="ReW1i"></a>

## 4.3 交叉类型

交叉类型（&）：功能类似于接口继承（extends），用于组合多个类型为一个类型（常用于对象类型）。<br />比如<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689494186944-0effdf69-c133-4fed-b40f-c8edbf0fbc5b.png#averageHue=%23333122&clientId=u22b399c1-6df0-4&from=paste&height=273&id=ufc64a876&originHeight=273&originWidth=603&originalType=binary&ratio=1&rotation=0&showTitle=false&size=164390&status=done&style=shadow&taskId=ubdff51c8-4967-47fb-8b1f-0a0a7564d1f&title=&width=603)<br />解释：使用交叉类型后，新的类型 PersonDetail 就同时具备了 Person 和 Contact 的所有属性类型。<br />相当于<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689494207079-7cc9e907-f1c1-4c43-b372-5de635a54957.png#averageHue=%2320231e&clientId=u22b399c1-6df0-4&from=paste&height=46&id=ua27b49aa&originHeight=46&originWidth=803&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47381&status=done&style=shadow&taskId=ub0897057-b10d-46bc-87cb-a2cef52567b&title=&width=803)


交叉类型（&）和接口继承（extends）的对比：

- 相同点：都可以实现对象类型的组合。
- 不同点：两种方式实现类型组合时，对于同名属性之间，处理类型冲突的方式不同。

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689494250896-4c18798d-8091-4aa4-8a85-eb7dbe1d9a05.png#averageHue=%233f2f23&clientId=u22b399c1-6df0-4&from=paste&height=271&id=ub34bcd61&originHeight=271&originWidth=1072&originalType=binary&ratio=1&rotation=0&showTitle=false&size=228996&status=done&style=shadow&taskId=ueac3d5f5-8d33-44b1-97d0-a14dbc82975&title=&width=1072)<br />说明：以上代码，接口继承会报错（类型不兼容）；交叉类型没有错误，可以简单的理解为：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689494268014-534174ed-4d9f-4064-aae2-b28cc064e134.png#averageHue=%2320231e&clientId=u22b399c1-6df0-4&from=paste&height=50&id=uebda4248&originHeight=50&originWidth=604&originalType=binary&ratio=1&rotation=0&showTitle=false&size=38011&status=done&style=shadow&taskId=uf8c61c1d-6f4d-4ef6-a6b5-d6dcae2444f&title=&width=604)这个函数参数里面传这两种数据类型都可以
<a name="xk2T9"></a>

## 4.4 泛型

泛型是可以在保证类型安全前提下，让函数等与多种类型一起工作，从而实现复用，常用于：函数、接口、class 中。 需求：创建一个 id 函数，传入什么数据就返回该数据本身（也就是说，参数和返回值类型相同）。<br />比如，id(10) 调用一个函数就会直接返回 10 本身。为了能让函数能够接受任意类型，可以将参数类型修改为 any。但是，这样就失去了 TS 的类型保护，类型不安全。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689494355147-eefe9ca5-605f-4bde-956c-4e53a7f7329d.png#averageHue=%23212620&clientId=u22b399c1-6df0-4&from=paste&height=46&id=u75ff4fa6&originHeight=46&originWidth=714&originalType=binary&ratio=1&rotation=0&showTitle=false&size=44322&status=done&style=shadow&taskId=u1245ea17-115e-43da-a433-87437cf8fdb&title=&width=714)<br />所以就要有了**泛型**，泛型在保证类型安全（不丢失类型信息）的同时，可以让函数等与多种不同的类型一起工作，灵活可复用。

**创建泛型函数：**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689494389548-66745395-f8d5-4483-8ec6-f29150983de4.png#averageHue=%23242d25&clientId=u22b399c1-6df0-4&from=paste&height=49&id=u2ec5ab41&originHeight=49&originWidth=833&originalType=binary&ratio=1&rotation=0&showTitle=false&size=54846&status=done&style=shadow&taskId=u1ccb4cc0-275e-4ee5-8cdb-a0b5c6c9096&title=&width=833)<br />解释：

1. 语法：在函数名称的后面添加 <>（尖括号），尖括号中添加类型变量，比如此处的 Type。
2. 类型变量 Type，是一种特殊类型的变量，它处理**类型**而不是值。
3. 该类型变量相当于一个类型容器，能够捕获用户提供的类型（具体是什么类型由用户调用该函数时指定）。
4. 因为 Type 是类型，因此可以将其作为函数参数和返回值的类型，表示参数和返回值具有相同的类型。
5. 类型变量 Type，可以是任意合法的变量名称。

**调用泛型函数：**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689494481019-704ece60-fc91-46c9-b626-c1c4871e5d4b.png#averageHue=%2327382e&clientId=u22b399c1-6df0-4&from=paste&height=100&id=uba4a90c7&originHeight=100&originWidth=862&originalType=binary&ratio=1&rotation=0&showTitle=false&size=89857&status=done&style=shadow&taskId=ubc69b3dc-e189-4e1f-a6dd-49b0e991dc8&title=&width=862)<br />解释：

1. 语法：在函数名称的后面添加 <>（尖括号），尖括号中指定具体的类型，比如，此处的 number。
2. 当传入类型 number 后，这个类型就会被函数声明时指定的类型变量 Type 捕获到。
3. 此时，Type 的类型就是 number，所以，函数 id 参数和返回值的类型也都是 number。

同样，如果传入类型 string，函数 id 参数和返回值的类型就都是 string。<br />这样，通过泛型就做到了让 id 函数与多种不同的类型一起工作，实现了复用的同时保证了类型安全。

**简化调用泛型函数**<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689494550601-61475188-ad32-43f7-b217-ef8c40fa1fb3.png#averageHue=%23293229&clientId=u22b399c1-6df0-4&from=paste&height=105&id=ue6ca8f3e&originHeight=105&originWidth=903&originalType=binary&ratio=1&rotation=0&showTitle=false&size=76489&status=done&style=shadow&taskId=u3b897976-e0a9-4b27-9585-e57b9c8cde6&title=&width=903)<br />解释：

1. 在调用泛型函数时，可以省略 <类型> 来简化泛型函数的调用。
2. 此时，TS 内部会采用一种叫做类型参数推断的机制，来根据传入的实参自动推断出类型变量 Type 的类型。
3. 比如，传入实参 10，TS 会自动推断出变量 num 的类型 number，并作为 Type 的类型。

推荐：使用这种简化的方式调用泛型函数，使代码更短，更易于阅读。<br />说明：当编译器无法推断类型或者推断的类型不准确时，就需要显式地传入类型参数。


<a name="Xwqaf"></a>

### 泛型约束

**泛型约束**：默认情况下，泛型函数的类型变量 Type 可以代表多个类型，这导致无法访问任何属性。<br />比如，id('a') 调用函数时获取参数的长度：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689494618030-ea7f5bb6-4214-4686-9a11-4858d799514c.png#averageHue=%232f2921&clientId=u22b399c1-6df0-4&from=paste&height=161&id=ucfa4647d&originHeight=161&originWidth=607&originalType=binary&ratio=1&rotation=0&showTitle=false&size=87924&status=done&style=shadow&taskId=uebc470ec-5e43-40bb-8afa-1b555246d82&title=&width=607)<br />解释：Type 可以代表任意类型，无法保证一定存在 length 属性，比如 number 类型就没有 length。<br />此时，就需要为泛型添加约束来收缩类型（缩窄类型取值范围）。

添加泛型约束收缩类型，主要有以下两种方式：

1.  指定更加具体的类型 
2.  添加约束。
    <a name="vDwOx"></a>

#### 指定更加具体的类型

比如，将类型修改为 Type[]（Type 类型的数组），因为只要是数组就一定存在 length 属性，因此就可以访问了。

```typescript
function id<Type>(value: Type[]): Type[] {
  console.log(value.length);
  return value;
}

// 调用,这里传入的一定得是个数组
id<string>(["a"]); //1

```

<a name="icau3"></a>

#### 添加约束

```typescript
interface MyLength {
  length: number;
}
function id<Type extends MyLength>(value: Type): Type {
  console.log(value.length);
  return value;
}

// 调用
id<string>("abc"); //3

```

解释：

1. 创建描述约束的接口 MyLength，该接口要求提供 length 属性。
2. 通过 extends 关键字使用该接口，为泛型（类型变量）添加约束。
3. 该约束表示：传入的类型必须具有 length 属性。

注意：传入的实参（比如，数组）只要有 length 属性即可，这也符合前面讲到的接口的类型兼容性。就可以直接传入数组或者字符串了


泛型的类型变量可以有多个，并且类型变量之间还可以约束（比如，第二个类型变量受第一个类型变量约束）。比如，创建一个函数来获取对象中属性的值：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689495146026-0c556e13-6650-4dc2-9792-02a20ec6bbe6.png#averageHue=%233f3625&clientId=u22b399c1-6df0-4&from=paste&height=203&id=u9016724d&originHeight=203&originWidth=1077&originalType=binary&ratio=1&rotation=0&showTitle=false&size=154558&status=done&style=shadow&taskId=ueaba268d-378d-4cec-9718-dcf7d444e46&title=&width=1077)<br />解释：

1. 添加了第二个类型变量 Key，两个类型变量之间使用（,）逗号分隔。
2. keyof 关键字接收一个对象类型，生成其键名称（可能是字符串或数字）的联合类型。(这里生成的是Type的联合，传入过来的是'name' | 'age')
3. 本示例中 keyof Type 实际上获取的是 person 对象所有键的联合类型，也就是：'name' | 'age'。
4. 类型变量 Key 受 Type 约束，可以理解为：Key 只能是 Type 所有键中的任意一个，或者说只能访问对象中存在的属性。


**泛型接口**：接口也可以配合泛型来使用，以增加其灵活性，增强其复用性。<br />![左边定义泛型接口，右边使用这个接口](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689495274593-5fbb0418-cc05-4119-b161-6ea9f2707627.png#averageHue=%2322221e&clientId=u22b399c1-6df0-4&from=paste&height=159&id=u8b70ba8c&originHeight=159&originWidth=986&originalType=binary&ratio=1&rotation=0&showTitle=true&size=149244&status=done&style=shadow&taskId=u76acfbed-8b1a-4d03-9336-f68e504e820&title=%E5%B7%A6%E8%BE%B9%E5%AE%9A%E4%B9%89%E6%B3%9B%E5%9E%8B%E6%8E%A5%E5%8F%A3%EF%BC%8C%E5%8F%B3%E8%BE%B9%E4%BD%BF%E7%94%A8%E8%BF%99%E4%B8%AA%E6%8E%A5%E5%8F%A3&width=986 "左边定义泛型接口，右边使用这个接口")<br />解释：

1. 在接口名称的后面添加 <类型变量>，那么，这个接口就变成了泛型接口。
2. 接口的类型变量，对接口中所有其他成员可见，也就是**接口中**所有成员都可以使用类型变量。
3. 使用泛型接口时，需要显式指定具体的类型（比如，此处的 IdFunc<nunber>）。
4. 此时，id 方法的参数和返回值类型都是 number；ids 方法的返回值类型是 number[]。


实际上，JS 中的数组在 TS 中就是一个泛型接口。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689495374567-1f2beb52-846c-4e17-8adc-c41fb094a5da.png#averageHue=%23344a3b&clientId=u22b399c1-6df0-4&from=paste&height=334&id=uafbbe5f6&originHeight=334&originWidth=1107&originalType=binary&ratio=1&rotation=0&showTitle=false&size=300852&status=done&style=shadow&taskId=u1c53ed6c-db6c-4209-99e1-374b36881de&title=&width=1107)<br />当我们在使用数组时，TS 会根据数组的不同类型，来自动将类型变量设置为相应的类型。


**泛型类**：class 也可以配合泛型来使用。<br />比如，React 的 class 组件的基类 Component 就是泛型类，不同的组件有不同的 props 和 state。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689495466599-4bc54295-c70f-488c-a849-700c2d34b1a8.png#averageHue=%232f241f&clientId=u22b399c1-6df0-4&from=paste&height=387&id=ufcc4b6d0&originHeight=387&originWidth=903&originalType=binary&ratio=1&rotation=0&showTitle=false&size=217413&status=done&style=shadow&taskId=ue3311d3f-d6d7-4ef6-b561-8680ff9d67f&title=&width=903)<br />解释：React.Component 泛型类两个类型变量，分别指定 props 和 state 类型。

创建泛型类：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689495520725-a11940fd-5c6f-4a8f-af92-2e6180954e12.png#averageHue=%2324231e&clientId=u22b399c1-6df0-4&from=paste&height=157&id=uded3ce60&originHeight=157&originWidth=666&originalType=binary&ratio=1&rotation=0&showTitle=false&size=99642&status=done&style=shadow&taskId=uaadf96ad-8d59-43ba-b359-66e467bfb5c&title=&width=666)<br />解释：

1. 类似于泛型接口，在 class 名称后面添加 <类型变量>，这个类就变成了泛型类。
2. 此处的 add 方法，采用的是箭头函数形式的类型书写方式。

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689495493042-93112b44-524f-481a-899f-e350ca3d533b.png#averageHue=%23212520&clientId=u22b399c1-6df0-4&from=paste&height=82&id=u1c122941&originHeight=82&originWidth=655&originalType=binary&ratio=1&rotation=0&showTitle=false&size=62938&status=done&style=shadow&taskId=u9f0f36a2-d78f-4ceb-9ea3-786921a85df&title=&width=655)<br />类似于泛型接口，在创建 class 实例时，在类名后面通过 <类型> 来指定明确的类型。



**泛型工具类型**：TS 内置了一些常用的工具类型，来简化 TS 中的一些常见操作。<br />说明：它们都是基于泛型实现的（泛型适用于多种类型，更加通用），并且是内置的，可以直接在代码中使用。<br />这些工具类型有很多，主要学习以下几个：

1. Partial<Type>
2. Readonly<Type>
3. Pick<Type, Keys>
4. Record<Keys, Type>

泛型工具类型 - **Partial<Type>** 用来构造（创建）一个类型，将 Type 的所有属性设置为可选。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689495568104-0c35ac63-d48d-430f-bd88-6b445250711c.png#averageHue=%2322221e&clientId=u22b399c1-6df0-4&from=paste&height=201&id=uc588dae3&originHeight=201&originWidth=544&originalType=binary&ratio=1&rotation=0&showTitle=false&size=90904&status=done&style=shadow&taskId=u5a6679b9-6bf4-4ad3-af5b-cd15e357c68&title=&width=544)<br />解释：构造出来的新类型 PartialProps 结构和 Props 相同，但所有属性都变为可选的（可选的意思之前介绍过）。

泛型工具类型 -** Readonly<Type>** 用来构造一个类型，将 Type 的所有属性都设置为 readonly（只读）。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689495613527-88c12ee5-6db1-4758-9149-0a26341fb44e.png#averageHue=%2322221e&clientId=u22b399c1-6df0-4&from=paste&height=197&id=u8928ee68&originHeight=197&originWidth=576&originalType=binary&ratio=1&rotation=0&showTitle=false&size=90502&status=done&style=shadow&taskId=u4bfc962c-d5a6-46cb-9fea-f7e6eef2909&title=&width=576)<br />解释：构造出来的新类型 ReadonlyProps 结构和 Props 相同，但所有属性都变为只读的。<br />![这里的id报错了](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689495620846-8b2637d1-cca2-4f61-8a7e-d93b0da4fc20.png#averageHue=%232e2c22&clientId=u22b399c1-6df0-4&from=paste&height=101&id=u4ce331f1&originHeight=101&originWidth=818&originalType=binary&ratio=1&rotation=0&showTitle=true&size=59257&status=done&style=shadow&taskId=ua9f61c41-54a0-41db-b31a-c24f5aad14b&title=%E8%BF%99%E9%87%8C%E7%9A%84id%E6%8A%A5%E9%94%99%E4%BA%86&width=818 "这里的id报错了")<br />当我们想重新给 id 属性赋值时，就会报错：无法分配到 "id" ，因为它是只读属性。

泛型工具类型 - **Pick<Type, Keys>** 从 Type 中选择一组属性来构造新类型。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689495648553-3113e58c-403f-417c-8494-3f773e83dedf.png#averageHue=%23423121&clientId=u22b399c1-6df0-4&from=paste&height=238&id=ue6e6f2f3&originHeight=238&originWidth=699&originalType=binary&ratio=1&rotation=0&showTitle=false&size=111436&status=done&style=shadow&taskId=ud88e78b2-0eb5-47fe-bbeb-70f6fa751d1&title=&width=699)<br />解释：

1. Pick 工具类型有两个类型变量：
   1. 表示选择谁的属性 
   2. 表示选择哪几个属性。
2. 其中第二个类型变量，如果只选择一个则只传入该属性名即可。
3. 第二个类型变量传入的属性只能是第一个类型变量中存在的属性。
4. 构造出来的新类型 PickProps，只有 id 和 title 两个属性类型。

泛型工具类型 - **Record<Keys,Type>** 构造一个对象类型，属性键为 Keys，属性类型为 Type。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689495721814-09bdbc28-2599-46b9-a2b1-43dca2f2fad8.png#averageHue=%23242222&clientId=u22b399c1-6df0-4&from=paste&height=235&id=ufea440fc&originHeight=235&originWidth=785&originalType=binary&ratio=1&rotation=0&showTitle=false&size=93657&status=done&style=shadow&taskId=u98cc1e9b-a89a-4c2b-81be-ac4a88f2895&title=&width=785)<br />解释：

1. Record 工具类型有两个类型变量：
   1. 表示对象有哪些属性 
   2. 表示对象属性的类型。
2. 构建的新对象类型 RecordObj 表示：这个对象有三个属性分别为a/b/c，属性值的类型都是 string[]。（这就意味着如果是混合的类型就用不了这个工具类了）
   <a name="Zx3Bq"></a>

## 4.5 索引签名类型

绝大多数情况下，我们都可以在使用对象前就确定对象的结构，并为对象添加准确的类型。<br />使用场景：当无法确定对象中有哪些属性（或者说对象中可以出现任意多个属性），此时，就用到索引签名类型了。<br />![这个图片里面可以出现任意个属性类型为number的变量](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689495797085-1f1a5800-63fd-4157-8ea6-50f22fa1ddc1.png#averageHue=%231b1919&clientId=u22b399c1-6df0-4&from=paste&height=161&id=ub988a2c0&originHeight=161&originWidth=795&originalType=binary&ratio=1&rotation=0&showTitle=true&size=84386&status=done&style=shadow&taskId=u08f745cd-3cc0-4786-9a0c-d825e9fa0d8&title=%E8%BF%99%E4%B8%AA%E5%9B%BE%E7%89%87%E9%87%8C%E9%9D%A2%E5%8F%AF%E4%BB%A5%E5%87%BA%E7%8E%B0%E4%BB%BB%E6%84%8F%E4%B8%AA%E5%B1%9E%E6%80%A7%E7%B1%BB%E5%9E%8B%E4%B8%BAnumber%E7%9A%84%E5%8F%98%E9%87%8F&width=795 "这个图片里面可以出现任意个属性类型为number的变量")<br />解释：

1. 使用 [key: string] 来约束该接口中允许出现的属性名称。表示只要是 string 类型的属性名称，都可以出现在对象中。
2. 这样，对象 obj 中就可以出现任意多个属性（比如，a、b 等）。
3. key 只是一个占位符，可以换成任意合法的变量名称。
4. 隐藏的前置知识：JS 中对象（{}）的键是 string 类型的。


**模拟数组**<br />在 JS 中数组是一类特殊的对象，特殊在数组的**键（索引key）**是数值类型。<br />并且，数组也可以出现任意多个元素。所以，在数组对应的泛型接口中，也用到了索引签名类型<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689495918679-1cb988e0-379d-49c0-bf90-a2c52eebd26e.png#averageHue=%2327241f&clientId=u22b399c1-6df0-4&from=paste&height=163&id=ubfcbd3b0&originHeight=163&originWidth=571&originalType=binary&ratio=1&rotation=0&showTitle=false&size=74898&status=done&style=shadow&taskId=ud22aeb39-fd97-4d1c-9238-28559456204&title=&width=571)<br />解释：

1. MyArray 接口模拟原生的数组接口，并使用 [n: number] 来作为索引签名类型。
2. 该索引签名类型表示：只要是 number 类型的键（索引）都可以出现在数组中，或者说数组中可以有任意多个元素。
3. 同时也符合数组索引是 number 类型这一前提。
   <a name="iBH7Q"></a>

## 4.6 映射类型

映射类型：基于旧类型创建新类型（对象类型），减少重复、提升开发效率。<br />比如，类型 PropKeys 有 x/y/z，另一个类型 Type1 中也有 x/y/z，并且 Type1 中 x/y/z 的类型相同：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689496039314-52d10cb6-c4d1-467b-b5c6-f7f9f0d6f244.png#averageHue=%232e2e21&clientId=u22b399c1-6df0-4&from=paste&height=95&id=uf7cdb0fb&originHeight=95&originWidth=758&originalType=binary&ratio=1&rotation=0&showTitle=false&size=76778&status=done&style=shadow&taskId=u18f4037d-b8a4-40b7-8321-7f2652abf7d&title=&width=758)<br />这样书写没错，但 x/y/z 重复书写了两次。像这种情况，就可以使用**映射类型**来进行简化。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689496047707-5233151f-6b88-4a45-831c-3a63b964805a.png#averageHue=%23262820&clientId=u22b399c1-6df0-4&from=paste&height=85&id=u2111d2d7&originHeight=85&originWidth=657&originalType=binary&ratio=1&rotation=0&showTitle=false&size=72096&status=done&style=shadow&taskId=u953250fd-3f60-410c-920d-b47b7f7d5ee&title=&width=657)<br />解释：

1. 映射类型是基于索引签名类型的，所以，该语法类似于索引签名类型，也使用了 []。
2. Key in PropKeys 表示 Key 可以是 PropKeys 联合类型中的任意一个。
3. 使用映射类型创建的新对象类型 Type2 和类型 Type1 结构完全相同。
4. 注意：映射类型只能在类型别名中使用，不能在接口中使用。

映射类型除了根据联合类型创建新类型外，还可以根据对象类型来创建：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689496126702-c7e6cb81-64e3-4b09-b064-dc050e758422.png#averageHue=%231e231f&clientId=u22b399c1-6df0-4&from=paste&height=85&id=ua8723059&originHeight=85&originWidth=775&originalType=binary&ratio=1&rotation=0&showTitle=false&size=95059&status=done&style=shadow&taskId=u1e34c5f8-af2a-4972-8fcc-a0b80c3e317&title=&width=775)<br />解释：

1. 首先，先执行 keyof Props 获取到对象类型 Props 中所有键的联合类型即，'a' | 'b' | 'c'。
2. 然后，Key in ... 就表示 Key 可以是 Props 中所有的键名称中的任意一个。

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689496143276-3e10f9c2-abb1-4487-84e8-89bfc144e49d.png#averageHue=%23242722&clientId=u22b399c1-6df0-4&from=paste&height=247&id=ue7cc47d0&originHeight=247&originWidth=765&originalType=binary&ratio=1&rotation=0&showTitle=false&size=137978&status=done&style=shadow&taskId=u77c766a3-28a1-457b-aaf7-a3ccca48812&title=&width=765)

实际上，前面讲到的泛型工具类型（比如，Partial<Type>）都是基于映射类型实现的。 比如，Partial<Type> 的实现：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689496161512-0233f8fb-1060-4ccc-bce1-41592d5bd1a7.png#averageHue=%2322221e&clientId=u22b399c1-6df0-4&from=paste&height=208&id=ucb3cfb35&originHeight=208&originWidth=916&originalType=binary&ratio=1&rotation=0&showTitle=false&size=149946&status=done&style=shadow&taskId=uc2a5e973-6729-4014-b062-5721253ca81&title=&width=916)<br />解释：

1. keyof T 即 keyof Props 表示获取 Props 的所有键，也就是：'a' | 'b' | 'c'。
2. 在 [] 后面添加 ?（问号），表示将这些属性变为可选的，以此来实现 Partial 的功能。
3. 冒号后面的 T[P] 表示获取 T 中每个键对应的类型。比如，如果是 'a' 则类型是 number；如果是 'b' 则类型是 string。
4. 最终，新类型 PartialProps 和旧类型 Props 结构完全相同，只是让所有类型都变为可选了。

刚刚用到的 T[P] 语法，在 TS 中叫做索引查询（访问）类型。<br />作用：用来查询属性的类型。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689496215956-d00a7c51-d9dd-4b29-9411-e6a7bd5b9bd8.png#averageHue=%232a2923&clientId=u22b399c1-6df0-4&from=paste&height=174&id=u4893b770&originHeight=174&originWidth=839&originalType=binary&ratio=1&rotation=0&showTitle=false&size=81577&status=done&style=shadow&taskId=u47f78570-6ee5-4efa-ad3f-a1de326cd0d&title=&width=839)<br />解释：Props['a'] 表示查询类型 Props 中属性 'a' 对应的类型 number。所以，TypeA 的类型为 number。<br />注意：[] 中的属性必须存在于被查询类型中，否则就会报错。<br />索引查询类型的其他使用方式：同时查询多个索引的类型<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689496234902-9444b494-9bae-4851-ba15-52a3e35af0be.png#averageHue=%232b2b21&clientId=u22b399c1-6df0-4&from=paste&height=113&id=u828d2d1f&originHeight=113&originWidth=767&originalType=binary&ratio=1&rotation=0&showTitle=false&size=88901&status=done&style=shadow&taskId=ucc5c39d9-d24f-4db5-88e3-41b02eefca7&title=&width=767)<br />解释：使用字符串字面量的联合类型，获取属性 a 和 b 对应的类型，结果为： string | number。![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689496244841-8ae724a4-61ae-47b7-8c41-387e6040dc1f.png#averageHue=%2321231f&clientId=u22b399c1-6df0-4&from=paste&height=54&id=ua0ce0f4a&originHeight=54&originWidth=937&originalType=binary&ratio=1&rotation=0&showTitle=false&size=50673&status=done&style=shadow&taskId=uc7ab8820-b268-468b-8c1d-baa59064dfc&title=&width=937)<br />解释：使用 keyof 操作符获取 Props 中所有键对应的类型，结果为： string | number | boolean。<br />并且上面的会做一个去重的操作

<a name="Ug2Bl"></a>

# 5. TypeScript 类型声明文件

<a name="wFalq"></a>

## 5.0 概述

今天几乎所有的 JavaScript 应用都会引入许多第三方库来完成任务需求。<br />这些第三方库不管是否是用 TS 编写的，最终都要编译成 JS 代码，才能发布给开发者使用。<br />我们知道是 TS 提供了类型，才有了代码提示和类型保护等机制。<br />但在项目开发中使用第三方库时，你会发现它们几乎都有相应的 TS 类型，这些类型是怎么来的呢？从类型声明文件来  <br />**类型声明文件：用来为已存在的 JS 库提供类型信息。**<br />这样在 TS 项目中使用这些库时，就像用 TS 一样，都会有代码提示、类型保护等机制了。
<a name="r9epW"></a>

## 5.1 TS 中的两种文件类型

TS 中有两种文件类型：

1.  .ts 文件 
2.  .d.ts 文件。

- .ts 文件：
  - 既包含类型信息又可执行代码。
  - 可以被编译为 .js 文件，然后，执行代码。
  - 用途：编写程序代码的地方。
- .d.ts 文件：
  - 只包含类型信息的类型声明文件。
  - 不会生成 .js 文件，**仅用于提供类型信息**。
  - 用途：为 JS 提供类型信息。

总结：.ts 是 implementation（代码实现文件）；.d.ts 是 declaration（类型声明文件）。<br />如果要为 JS 库提供类型信息，要使用 .d.ts 文件。
<a name="eD3Zr"></a>

## 5.2 类型声明文件的使用说明

在使用 TS 开发项目时，类型声明文件的使用包括以下两种方式：

1. 使用已有的类型声明文件
2. 创建自己的类型声明文件

学习顺序：先会用（别人的）再会写（自己的）。

<a name="Uc7zK"></a>

### 使用已有的类型声明文件

1 内置类型声明文件 <br />2 第三方库的类型声明文件。<br />**内置类型声明文件**：TS 为 JS 运行时可用的所有标准化内置 API 都提供了声明文件。<br />比如，在使用数组时，数组所有方法都会有相应的代码提示以及类型信息：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689509817555-03909193-88d9-4849-917a-f126d2410edc.png#averageHue=%23333d40&clientId=u3d13f150-1e55-4&from=paste&height=89&id=u3e371888&originHeight=89&originWidth=1027&originalType=binary&ratio=1&rotation=0&showTitle=false&size=121906&status=done&style=shadow&taskId=uf2bdfee0-535f-44a4-9ad5-167819fe211&title=&width=1027)<br />**第三方库的类型声明文件**：目前，几乎所有常用的第三方库都有相应的类型声明文件。第三方库的类型声明文件有两种存在形式：

1.  库自带类型声明文件 
2.  由 DefinitelyTyped 提供。

**库自带类型声明文件**：比如，axios。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689509927470-555c4c9c-9c89-44ce-8498-01e7c3b41f10.png#averageHue=%233f411f&clientId=u3d13f150-1e55-4&from=paste&height=251&id=u8042054c&originHeight=251&originWidth=290&originalType=binary&ratio=1&rotation=0&showTitle=false&size=53085&status=done&style=shadow&taskId=udf7f3d5f-a507-45c2-b749-2219ad89a3b&title=&width=290)<br />解释：这种情况下，正常导入该库，TS 就会自动加载库自己的类型声明文件，以提供该库的类型声明。

**由 DefinitelyTyped 提供**<br />DefinitelyTyped 是一个 github 仓库，用来提供高质量 TypeScript 类型声明。<br />可以通过 npm/yarn 来下载该仓库提供的 TS 类型声明包，这些包的名称格式为：@types/*。<br />比如，@types/react、@types/lodash 等。<br />说明：在实际项目开发时，如果你使用的第三方库没有自带的声明文件，VSCode 会给出明确的提示<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689509976205-44c2ed87-e7e8-4155-9559-40d1859395d2.png#averageHue=%23252827&clientId=u3d13f150-1e55-4&from=paste&height=149&id=ub674b157&originHeight=149&originWidth=1033&originalType=binary&ratio=1&rotation=0&showTitle=false&size=164967&status=done&style=shadow&taskId=uca522ea1-547d-49f2-9c05-2ead40533cf&title=&width=1033)<br />解释：当安装 @types/* 类型声明包后，TS 也会自动加载该类声明包，以提供该库的类型声明。<br />补充：TS 官方文档提供了一个页面，可以来查询 @types/* 库。
<a name="tlt7O"></a>

### 自己的类型声明文件

创建自己的类型声明文件：

1.  项目内共享类型 
2.  为已有 JS 文件提供类型声明。

项目内共享类型：如果多个 .ts 文件中都用到同一个类型，此时可以创建 .d.ts 文件提供该类型，实现类型共享。<br />操作步骤：

1. 创建 index.d.ts 类型声明文件。
2. 创建需要共享的类型，并使用 export 导出（TS 中的类型也可以使用 import/export 实现模块化功能）。
3. 在需要使用共享类型的 .ts 文件中，通过 import 导入即可（.d.ts 后缀导入时，直接省略）。

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689510124468-db29316e-81bf-48ff-963d-26884e29485b.png#averageHue=%231e1e1e&clientId=u3d13f150-1e55-4&from=paste&id=uc8e4c9b4&originHeight=481&originWidth=1313&originalType=binary&ratio=1&rotation=0&showTitle=false&size=148452&status=done&style=shadow&taskId=u7c92d7d4-3f70-43f6-a0ac-89b9148120a&title=)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689510128532-096f6f56-d261-42d8-92bb-c8e46c3e02cc.png#averageHue=%231e1e1e&clientId=u3d13f150-1e55-4&from=paste&height=333&id=ub8e44df0&originHeight=333&originWidth=1064&originalType=binary&ratio=1&rotation=0&showTitle=false&size=124091&status=done&style=shadow&taskId=u18f9c400-0767-4186-a17c-4217c776598&title=&width=1064)

**为已有 JS 文件提供类型声明**：

1. 在将 JS 项目迁移到 TS 项目时，为了让已有的 .js 文件有类型声明。
2. 成为库作者，创建库给其他人使用。

注意：类型声明文件的编写与模块化方式相关，不同的模块化方式有不同的写法。但由于历史原因，JS 模块化的发展<br />经历过多种变化（AMD、CommonJS、UMD、ESModule 等），而 TS 支持各种模块化形式的类型声明。这就导致<br />，类型声明文件相关内容又多又杂。

说明：TS 项目中也可以使用 .js 文件。<br />说明：在导入 .js 文件时，TS 会自动加载与 .js 同名的 .d.ts 文件，以提供类型声明。<br />declare 关键字：用于类型声明，为其他地方（比如，.js 文件）已存在的变量声明类型，而不是创建一个新的变量。

1. 对于 type、interface 等这些明确就是 TS 类型的（只能在 TS 中使用的），可以省略 declare 关键字。
2. 对于 let、function 等具有双重含义（在 JS、TS 中都能用），应该使用 declare 关键字，明确指定此处用于类型声明。

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689510266790-853d96b3-97a2-4e32-bfb7-e77de417aa75.png#averageHue=%2341392d&clientId=u3d13f150-1e55-4&from=paste&height=760&id=u50ce6a89&originHeight=760&originWidth=1608&originalType=binary&ratio=1&rotation=0&showTitle=false&size=536487&status=done&style=shadow&taskId=ube42a48f-53c8-4822-aeb1-6894cb49a2e&title=&width=1608)<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689510276397-822eacbb-56d1-4e04-851d-f9f8f8e2c931.png#averageHue=%23252826&clientId=u3d13f150-1e55-4&from=paste&height=441&id=u5a31173f&originHeight=441&originWidth=1790&originalType=binary&ratio=1&rotation=0&showTitle=false&size=125877&status=done&style=shadow&taskId=u726de90a-82d0-4319-85ab-1751ba2a235&title=&width=1790)
<a name="piwqB"></a>

# 6. 在 React 中使用 TypeScript

<a name="nmUBV"></a>

## 6.1 使用 CRA 创建支持 TS 的项目

React 脚手架工具 create-react-app（简称：CRA）默认支持 TypeScript。<br />**创建支持 TS 的项目命令：**`npx create-react-app 项目名称 --template typescript`

相对于非 TS 项目，目录结构主要由以下三个变化：<br />项目根目录中增加了 tsconfig.json 配置文件：指定 TS 的编译选项（比如，编译时是否移除注释）。<br />React 组件的文件扩展名变为：*.tsx。<br />src 目录中增加了 react-app-env.d.ts：React 项目默认的类型声明文件。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689510400437-db90c8dc-e433-4947-a37a-ffe628146ca2.png#averageHue=%23222317&clientId=u3d13f150-1e55-4&from=paste&height=283&id=u3bbe6e4f&originHeight=283&originWidth=231&originalType=binary&ratio=1&rotation=0&showTitle=false&size=58158&status=done&style=shadow&taskId=u6c2b2a40-86db-402d-8cb9-2562c3b5859&title=&width=231)![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689510402498-270a2962-ff20-4699-a5d3-3eb18e043bd4.png#averageHue=%23232016&clientId=u3d13f150-1e55-4&from=paste&height=353&id=u5148a25d&originHeight=353&originWidth=272&originalType=binary&ratio=1&rotation=0&showTitle=false&size=79956&status=done&style=shadow&taskId=u1d30ef57-4701-457b-8c86-57d2040ac7c&title=&width=272)<br />react-app-env.d.ts：React 项目默认的类型声明文件。<br />三斜线指令：指定依赖的其他类型声明文件，types 表示依赖的类型声明文件包的名称。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689510454004-31cb4b55-713c-48f0-bdce-4dba7c8cabc4.png#averageHue=%23201f1f&clientId=u3d13f150-1e55-4&from=paste&height=227&id=u4f7dda5e&originHeight=227&originWidth=647&originalType=binary&ratio=1&rotation=0&showTitle=false&size=13095&status=done&style=shadow&taskId=u25648753-7b80-49f4-843d-09fd8d26fbe&title=&width=647)<br />解释：告诉 TS 帮我们加载 react-scripts 这个包提供的类型声明。<br />react-scripts 的类型声明文件包含了两部分类型：

1. react、react-dom、node 的类型
2. 图片、样式等模块的类型，以允许在代码中导入图片、SVG 等文件。

TS 会自动加载该 .d.ts 文件，以提供类型声明（通过修改 tsconfig.json 中的 include 配置来验证）。
<a name="DbOk8"></a>

## 6.2 TS 配置文件 tsconfig.json

tsconfig.json 指定：项目文件和项目编译所需的配置项。<br />注意：TS 的配置项非常多（100+），其他的配置项用到时查文档即可。

1. tsconfig.json 文件所在目录为项目根目录（与 package.json 同级）。
2. tsconfig.json 可以自动生成，命令：tsc --init。
   <a name="pYa1L"></a>

## 6.3 React 中的常用类型

<a name="SnhRx"></a>

### 函数式组件

函数组件的类型以及组件的属性<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689510603086-172d92d3-6587-472b-89d0-4316ad8d2c01.png#averageHue=%23443625&clientId=u3d13f150-1e55-4&from=paste&height=247&id=u0ced07b2&originHeight=247&originWidth=700&originalType=binary&ratio=1&rotation=0&showTitle=false&size=148596&status=done&style=shadow&taskId=u46a40fbc-02cb-4324-8581-a9f16c5de12&title=&width=700)<br />实际上，还可以直接简化为（完全按照函数在 TS 中的写法）：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689510631835-6bd36343-58ba-49a6-9397-0af1b80bb686.png#averageHue=%2322221e&clientId=u3d13f150-1e55-4&from=paste&height=117&id=u53251a0b&originHeight=117&originWidth=668&originalType=binary&ratio=1&rotation=0&showTitle=false&size=81023&status=done&style=shadow&taskId=uedf93ac4-863a-4c83-a690-e7da8af0749&title=&width=668)<br />具体代码：下面的FC是一个react提供的东西<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689510586682-ea25b3cf-5184-48c4-ba16-0e90620d4640.png#averageHue=%23626161&clientId=u3d13f150-1e55-4&from=paste&height=770&id=u32c297a8&originHeight=770&originWidth=1288&originalType=binary&ratio=1&rotation=0&showTitle=false&size=386367&status=done&style=shadow&taskId=udf094f4d-7b5d-49bf-a780-e92d0d1ad9d&title=&width=1288)<br />函数组件属性的默认值（defaultProps）<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689510659078-e1950339-9423-4cf2-8f75-ac8b2eabc346.png#averageHue=%233f3024&clientId=u3d13f150-1e55-4&from=paste&height=226&id=u4549fa76&originHeight=226&originWidth=711&originalType=binary&ratio=1&rotation=0&showTitle=false&size=113388&status=done&style=shadow&taskId=ue737fe95-a7a9-41c9-be7d-700ed944cf6&title=&width=711)<br />实际上，还可以直接简化为（完全按照函数在 TS 中的写法）：<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689510671295-ccc432bb-45ac-4126-86e8-17a22361773e.png#averageHue=%2321221e&clientId=u3d13f150-1e55-4&from=paste&height=115&id=u3148b270&originHeight=115&originWidth=716&originalType=binary&ratio=1&rotation=0&showTitle=false&size=83494&status=done&style=shadow&taskId=udeb47d7d-c657-452d-8b79-c79bea9de14&title=&width=716)<br />事件绑定和事件对象<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689510726252-21441936-6bd5-4bae-a4fe-8b1907c19e04.png#averageHue=%23292620&clientId=u3d13f150-1e55-4&from=paste&height=104&id=u4a40da54&originHeight=104&originWidth=968&originalType=binary&ratio=1&rotation=0&showTitle=false&size=95149&status=done&style=shadow&taskId=ue82306b0-7fdc-40ff-92fb-e26bf716fa3&title=&width=968)<br />技巧：在 JSX 中写事件处理程序（e => {}），然后，把鼠标放在 e 上，利用 TS 的类型推论来查看事件对象类型。<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689510732434-e9220984-625c-429c-be3e-19fb0334a826.png#averageHue=%23242826&clientId=u3d13f150-1e55-4&from=paste&height=85&id=ua7cea2e2&originHeight=85&originWidth=1072&originalType=binary&ratio=1&rotation=0&showTitle=false&size=75462&status=done&style=shadow&taskId=u30d1d062-51cf-4a6b-bc85-0e871206671&title=&width=1072)
<a name="Kl5Ho"></a>

### class组件

class 组件的类型<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689510784640-adb91630-b258-4c49-8257-3fb4773c1dfd.png#averageHue=%2321221e&clientId=u3d13f150-1e55-4&from=paste&height=269&id=u1bd596e0&originHeight=269&originWidth=1090&originalType=binary&ratio=1&rotation=0&showTitle=false&size=294662&status=done&style=shadow&taskId=u6c019a9a-976b-44e8-891e-f277b9d9330&title=&width=1090)class 组件的属性和属性默认值<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689510829768-99d452a3-4dc7-4185-ba22-7f731ea83769.png#averageHue=%23292520&clientId=u3d13f150-1e55-4&from=paste&height=497&id=u6585566c&originHeight=497&originWidth=845&originalType=binary&ratio=1&rotation=0&showTitle=false&size=240832&status=done&style=shadow&taskId=u17966e0b-f619-4375-a531-812b1d7c923&title=&width=845)class 组件状态（state）和事件<br />![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1689510839813-11978644-a44c-4624-ac57-e3fe8f2a6045.png#averageHue=%2326231f&clientId=u3d13f150-1e55-4&from=paste&height=468&id=u04d661f1&originHeight=468&originWidth=780&originalType=binary&ratio=1&rotation=0&showTitle=false&size=225798&status=done&style=shadow&taskId=u9f68f695-1dee-4fe4-87e4-67de7abdce9&title=&width=780)
