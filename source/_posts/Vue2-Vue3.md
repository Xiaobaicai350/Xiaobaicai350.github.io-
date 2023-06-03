---
title: Vue2+Vue3
date: 2023-06-03 15:56:37
tags: 前端
---

# 前端工程化与 webpack

## 前端工程化

### 1.小白眼中的前端开发 vs 实际的前端开发

小白眼中的前端开发：

- 会写HTML + CSS + JavaScript 就会前端开发
- 需要美化页面样式，就拽一个bootstrap 过来
- 需要操作 DOM 或发起 Ajax 请求，再拽一个 jQuery 过来
- 需要快速实现网页布局效果，就拽一个 Layui 过来



实际的前端开发：

- 模块化（js 的模块化、css 的模块化、资源的模块化）
- 组件化（复用现有的 UI 结构、样式、行为）
- 规范化（目录结构的划分、编码规范化、接口规范化、文档规范化、 Git 分支管理）
- 自动化（自动化构建、自动部署、自动化测试）

### 2.什么是前端工程化

前端工程化指的是：在企业级的前端项目开发中，把前端开发所需的工具、技术、流程、经验等进行规范化、标准化。

企业中的 Vue 项目和 React 项目，都是基于工程化的方式进行开发的。

好处：前端开发自成体系，有一套标准的开发方案和流程。

### 3.前端工程化的解决方案

早期的前端工程化解决方案：
grunt（ https://www.gruntjs.net/ ）
gulp（ https://www.gulpjs.com.cn/ ）



目前主流的前端工程化解决方案：
webpack（ https://www.webpackjs.com/ ）
parcel（ https://zh.parceljs.org/ ）

## webpack 的基本使用

### 1.什么是 webpack

概念：webpack 是前端项目工程化的具体解决方案。

主要功能：它提供了友好的前端模块化开发支持，以及代码压缩混淆、处理浏览器端 JavaScript 的兼容性、性能优化等强大的功能。



好处：让程序员把工作的重心放到具体功能的实现上，提高了前端开发效率和项目的可维护性。

注意：目前Vue，React 等前端项目，基本上都是基于webpack 进行工程化开发的。

### 2.创建列表隔行变色项目

① 新建项目空白目录，并运行npm init –y 命令(运行这个目录的前提，必须是纯英文目录)，初始化包管理配置文件package.json

② 新建 src 源代码目录

③ 新建 src -> index.html 首页和 src -> index.js 脚本文件![image-20230603213047875](../pic/image-20230603213047875.png)

④ 初始化首页基本的结构

⑤ 运行npm install jquery –S （这个参数的意思是把jquery添加到dependencies目录下，不过不加这个指令也会安装到这个目录下，我个人感觉没有必要加)命令，安装 jQuery

⑥ 通过 ES6 模块化的方式导入 jQuery，实现列表隔行变色效果

### 3.在项目中安装 webpack

在终端运行如下的命令，安装webpack 相关的两个包：

`npm install webpack@5.42.1 webpack-cli@4.7.2 -D`(这个D的意思是安装到devDependencies)

![image-20230603213132868](../pic/image-20230603213132868.png)

至于为什么这样安装，可以看npm的官方

![image-20230603213151718](../pic/image-20230603213151718.png)

注意：

- -S是 --save的简写
- -D是 --save-dev的简写

### 4.在项目中配置 webpack

①  在项目根目录中，创建名为 webpack.config.js 的 webpack 配置文件，并初始化如下的基本配置：

![image-20230603213224083](../pic/image-20230603213224083.png)

② 在 package.json 的 scripts 节点下，新增 dev 脚本如下：

![image-20230603213232797](../pic/image-20230603213232797.png)

上面后面这个webpack是固定的

③ 在终端中运行npm run dev 命令，启动webpack 进行项目的打包构建

#### mode 的可选值

mode 节点的可选值有两个，分别是：

①	development

- 开发环境
- 不会对打包生成的文件进行代码压缩和性能优化
- 打包速度快，适合在开发阶段使用

②	production

- 生产环境
- 会对打包生成的文件进行代码压缩和性能优化
- 打包速度很慢，仅适合在项目发布阶段使用

![image-20230603213327565](../pic/image-20230603213327565.png)

#### webpack.config.js 文件的作用

webpack.config.js 是 webpack 的配置文件。webpack 在真正开始打包构建之前，会先读取这个配置文件，从而基于给定的配置，对项目进行打包。（也就是说，当我们运行npm run dev 的时候，先会读取webpack.config.js中的配置选项，之后会根据模式运行webpack命令）

注意：由于 webpack 是基于 node.js 开发出来的打包工具，因此在它的配置文件中，支持使用 node.js 相关的语法和模块进行webpack 的个性化配置。

#### webpack 中的默认约定

在 webpack 4.x 和 5.x 的版本中，有如下的默认约定：

① 默认的打包入口文件为 src -> index.js(src下面的index.js文件)

② 默认的输出文件路径为dist -> main.js（dist下面的main.js文件）



注意：可以在webpack.config.js 中修改打包的默认约定

#### 自定义打包的入口与出口

在 webpack.config.js 配置文件中，通过entry 节点指定打包的入口。通过output 节点指定打包的出口。示例代码如下：

![image-20230603213450195](../pic/image-20230603213450195.png)

## webpack 中的插件

### webpack 插件的作用

通过安装和配置第三方的插件，可以拓展webpack 的能力，从而让webpack 用起来更方便。最常用的webpack 插件有如下两个：

①	webpack-dev-server

- 类似于node.js 阶段用到的 nodemon 工具
- 每当修改了源代码，然后ctrl+s，webpack 会自动进行项目的打包和构建,这样我们就可以实时看到我们修改的效果咯

②	html-webpack-plugin

- webpack 中的HTML 插件（类似于一个模板引擎插件）
- 可以通过此插件自定制 index.html 页面的内容

### webpack-dev-server

webpack-dev-server 可以让webpack 监听项目源代码的变化，从而进行自动打包构建。

#### 安装 webpack-dev-server

运行如下的命令，即可在项目中安装此插件：

`npm install webpack-dev-server@3.11.2 -D`

#### 配置 webpack-dev-server

① 修改package.json -> scripts 中的dev 命令如下：（之前是dev:webpack,现在加了一个serve）

![image-20230603213647963](../pic/image-20230603213647963.png)

② 再次运行npm run dev 命令，重新进行项目的打包

③  在浏览器中访问http://localhost:8080 地址，查看自动打包效果（这个是项目的根目录）

![image-20230603213659746](../pic/image-20230603213659746.png)

注意：webpack-dev-server 会启动一个实时打包的http 服务器

#### 打包生成的文件哪儿去了？

① 不配置webpack-dev-server 的情况下，webpack 打包生成的文件，会存放到实际的物理磁盘上

- 严格遵守开发者在webpack.config.js 中指定配置
- 根据output 节点指定路径进行存放

② 配置了 webpack-dev-server 之后，打包生成的文件存放到了内存中

- 不再根据output 节点指定的路径，存放到实际的物理磁盘上
- 提高了实时打包输出的性能，因为内存比物理磁盘速度快很多

#### 生成到内存中的文件该如何访问？

webpack-dev-server 生成到内存中的文件，默认放到了项目的根目录中，而且是虚拟的、不可见的。

- 可以直接用/ 表示项目根目录，后面跟上要访问的文件名称，即可访问内存中的文件
- 例如/bundle.js 就表示要访问webpack-dev-server 生成到内存中的bundle.js 文件

### html-webpack-plugin

html-webpack-plugin 是 webpack 中的HTML 插件，可以通过此插件自定制 index.html 页面的内容。

需求：通过html-webpack-plugin 插件，将 src 目录下的 index.html 首页，复制到项目根目录中一份！

#### 安装 html-webpack-plugin

运行如下的命令，即可在项目中安装此插件：

`npm install html-webpack-plugin@5.3.2 -D`

#### 配置 html-webpack-plugin

下面的配置是在webpack.config.js配置文件中进行配置的

![image-20230603213913500](../pic/image-20230603213913500.png)

插件是在webpack指令执行期间会加载并调用这些插件

#### 解惑 html-webpack-plugin

① 通过HTML 插件复制到项目根目录中的 index.html 页面，也被放到了内存中

② HTML 插件在生成的 index.html 页面，自动注入了打包的bundle.js （不一定一定是这个文件，这个文件是我们自己配置的名字）文件

### devServer 节点

在 webpack.config.js 配置文件中，可以通过devServer 节点对webpack-dev-server 插件进行更多的配置，示例代码如下：

![image-20230603214011791](../pic/image-20230603214011791.png)

修改端口号也是在这修改的

注意：凡是修改了 webpack.config.js 配置文件，或修改了 package.json 配置文件，必须重启实时打包的服务器，否则最新的配置文件无法生效！

## webpack 中的 loader

### loader 概述

在实际开发过程中，webpack 默认只能打包处理以.js 后缀名结尾的模块。其他非.js 后缀名结尾的模块，webpack 默认处理不了，需要调用 loader 加载器才可以正常打包，否则会报错！



loader 加载器的作用：协助 webpack 打包处理特定的文件模块。比如：

- css-loader 可以打包处理.css 相关的文件
- less-loader 可以打包处理.less 相关的文件
- babel-loader 可以打包处理webpack 无法处理的高级 JS 语法

###  loader 的调用过程

![image-20230603214105413](../pic/image-20230603214105413.png)

### 打包处理 css 文件

①  运行npm i style-loader@3.0.0 css-loader@5.2.6 -D 命令，安装处理css 文件的 loader

② 在 webpack.config.js 的module -> rules 数组中，添加 loader 规则如下：

![image-20230603214124192](../pic/image-20230603214124192.png)

其中，test 表示匹配的文件类型， use 表示对应要调用的 loader

注意：

- use 数组中指定的 loader 顺序是固定的
- 多个 loader 的调用顺序是：从后往前调用

![image-20230603214141351](../pic/image-20230603214141351.png)

### 打包处理 less 文件

① 运行`npm i less-loader@10.0.1 less@4.1.1 -D`命令

② 在 webpack.config.js 的module -> rules 数组中，添加 loader 规则如下：

![image-20230603214214096](../pic/image-20230603214214096.png)

### 打包处理样式表中与 url 路径相关的文件

首先学一下base64怎么加载图片的：![image-20230603214256862](../pic/image-20230603214256862.png)



打包处理样式表中与 url 路径相关的文件：

①  运行`npm i url-loader@4.1.1 file-loader@6.2.0 -D `命令

② 在 webpack.config.js 的module -> rules 数组中，添加 loader 规则如下：

![image-20230603214236444](../pic/image-20230603214236444.png)

其中? 之后的是 loader 的参数项：

- limit 用来指定图片的大小，单位是字节（byte）
- 只有≤ limit 大小的图片，才会被转为base64 格式的图片，如果大于这个值，不会转成bese64格式，还是图片的格式

![image-20230603214329813](../pic/image-20230603214329813.png)

### 打包处理 js 文件中的高级语法

webpack 只能打包处理一部分高级的 JavaScript 语法。对于那些webpack 无法处理的高级 js 语法，需要借助于babel-loader 进行打包处理。例如webpack 无法处理下面的 JavaScript 代码：

![image-20230603214356933](../pic/image-20230603214356933.png)

#### 安装 babel-loader 相关的包

运行如下的命令安装对应的依赖包：

`npm i babel-loader@8.2.2 @babel/core@7.14.6 @babel/plugin-proposal-decorators@7.14.5 -D`



在 webpack.config.js 的 module -> rules 数组中，添加 loader 规则如下：

![image-20230603214422888](../pic/image-20230603214422888.png)

至于为什么要指定排除项：因为第三方包中的JS兼容性，不需要我们关心，人家已经处理好了...

#### 配置 babel-loader

> 这里其实是配置插件的插件

在项目根目录下，创建名为 babel.config.js 的配置文件，定义 Babel 的配置项如下：

![image-20230603214531382](../pic/image-20230603214531382.png)

详情请参考Babel 的官网https://babeljs.io/docs/en/babel-plugin-proposal-decorators

## 打包发布

### 为什么要打包发布

项目开发完成之后，需要使用webpack 对项目进行打包发布，主要原因有以下两点：

①  开发环境下，打包生成的文件存放于内存中，无法获取到最终打包生成的文件

② 开发环境下，打包生成的文件不会进行代码压缩和性能优化



为了让项目能够在生产环境中高性能的运行，因此需要对项目进行打包发布。

### 配置 webpack 的打包发布

在 package.json 文件的 scripts 节点下，新增build 命令如下：

![image-20230603214605967](../pic/image-20230603214605967.png)

--model 是一个参数项，用来指定webpack 的运行模式。production 代表生产环境，会对打包生成的文件进行代码压缩和性能优化（记不记得之前我们在webpack.config.js中配置模式为development，这里的--model就是覆盖那里的配置文件，改成了生产环境）

注意：通过 --model 指定的参数项，会覆盖webpack.config.js 中的model 选项。

### 把 JavaScript 文件统一生成到 js 目录中

在 webpack.config.js 配置文件的output 节点中，进行如下的配置：

![image-20230603214629006](../pic/image-20230603214629006.png)

### 把图片文件统一生成到 image 目录中

修改webpack.config.js 中的 url-loader 配置项，新增outputPath 选项即可指定图片文件的输出路径：

![image-20230603214647104](../pic/image-20230603214647104.png)

### 自动清理 dist 目录下的旧文件

为了在每次打包发布时自动清理掉dist 目录中的旧文件，可以安装并配置clean-webpack-plugin 插件：

![image-20230603214658639](../pic/image-20230603214658639.png)

## Source Map

### 生产环境遇到的问题

前端项目在投入生产环境之前，都需要对 JavaScript 源代码进行压缩混淆，从而减小文件的体积，提高文件的加载效率。此时就不可避免的产生了另一个问题：

对压缩混淆之后的代码除错（debug）是一件极其困难的事情，下面是压缩过的代码：

![image-20230603214716406](../pic/image-20230603214716406.png)

有如下的问题：

- 变量被替换成没有任何语义的名称
- 空行和注释被剔除

### 什么是 Source Map

Source Map 就是一个信息文件，里面储存着位置信息。也就是说，Source Map 文件中存储着压缩混淆后的代码，所对应的转换前的位置。

有了它，出错的时候，除错工具将直接显示原始代码，而不是转换后的代码，能够极大的方便后期的调试。

### webpack 开发环境下的 Source Map

在开发环境下，webpack 默认启用了 Source Map 功能。当程序运行出错时，可以直接在控制台提示错误行的位置，并定位到具体的源代码：

![image-20230603214811210](../pic/image-20230603214811210.png)

![image-20230603214814831](../pic/image-20230603214814831.png)

### 解决默认 Source Map 的问题

开发环境下，推荐在webpack.config.js 中添加如下的配置，即可保证运行时报错的行数与源代码的行数保持一致：

![image-20230603214827464](../pic/image-20230603214827464.png)

### webpack 生产环境下的 Source Map

在生产环境下，如果省略了devtool 选项，则最终生成的文件中不包含 Source Map。这能够防止原始代码通过 Source Map 的形式暴露给别有所图之人。

![image-20230603214839785](../pic/image-20230603214839785.png)

![image-20230603214848462](../pic/image-20230603214848462.png)

### 只定位行数不暴露源码

在生产环境下，如果只想定位报错的具体行数，且不想暴露源码。此时可以将 devtool 的值设置为
nosources-source-map。实际效果如图所示：

![image-20230603214902859](../pic/image-20230603214902859.png)

![image-20230603214906685](../pic/image-20230603214906685.png)

### 定位行数且暴露源码

在生产环境下，如果想在定位报错行数的同时，展示具体报错的源码。此时可以将devtool 的值设置为
source-map。实际效果如图所示：

![image-20230603214917448](../pic/image-20230603214917448.png)

![image-20230603214920680](../pic/image-20230603214920680.png)

采用此选项后：你应该将你的服务器配置为，不允许普通用户访问 source map 文件！

### Source Map 的最佳实践

① 开发环境下：

- 建议把devtool 的值设置为eval-source-map
- 好处：可以精准定位到具体的错误行

② 生产环境下：

- 建议关闭 Source Map 或将devtool 的值设置为nosources-source-map
- 好处：防止源码泄露，提高网站的安全性

### 实际开发中需要自己配置 webpack 吗？

答案：不需要！

- 实际开发中会使命令行工具（俗称 CLI）一键生成带有 webpack 的项目

- 开箱即用，所有 webpack 配置项都是现成的！

- 我们只需要知道 webpack 中的基本概念即可！

# @符号的作用

![image-20230603215025553](../pic/image-20230603215025553.png)

配置@指向的位置，但是在学了vue后他会默认给我们配置了

![image-20230603215044781](../pic/image-20230603215044781.png)
