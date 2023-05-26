---
title: Gin
date: 2023-04-20 23:18:00
tags:
---

Go是一门正在快速增长的编程语言，专为构建简单、快速且可靠的软件而设计。golang提供的nethttp库已经很好了，对于http的协议的实现非常好，基于此再造框架，也不会是难事，因此生态中出现了很多框架。

- Gin: Go 语言编写的Web框架，以更好的性能实现类似Martini框架的API。
  - Gin是一个golang的微框架，封装比较优雅，API友好，源码注释比较明确。具有快速灵活，容错方便等特点。Beego:开源的高性能Go语言Web框架。

- beego是一个快速开发Go应用的http框架，go语言方面技术大牛。
  - beego可以用来快速开发API、Web、后端服务等各种应用，是一个RESTFul的框架，主要设计灵感来源于tornado、sinatra、flask这三个框架，但是结合了Go本身的一些特性(interface、struct继承等)而设计的一个框架。lris:全宇宙最快的Go语言Web框架。完备MVC支持，未来尽在掌握。

- lris是一个快速,简单但功能齐全的和非常有效的web框架。
  - 提供了一个优美的表现力和容易使用你的下一个网站或API的基础。

# Gin

## 安装Gin

Gin是一个golang的微框架，封装比较优雅，API友好，源代码比较明确。具有快速灵活，容错方便等特点。其实对于golang而言，**web框架的依赖远比Python，Java之类的要小**。自身的net/http足够简单，性能也非常不错。框架更像是一个常用函数或者工具的集合。

借助框架开发不仅可以省去很多常用的封装带来的时间，也有助于团队的编码风格和形成规范。

Gin官方文档地址: https://gin-gonic.com/zh-cn/docs/



安装命令：

```shell
go get -u github.com/gin-gonic/gin
```

## HelloWorld

```go
package main

import "github.com/gin-gonic/gin"

func main() {
	//创建服务
	ginServer := gin.Default()
	//访问地址，处理请求
	ginServer.GET("/hello", func(context *gin.Context) {
		context.JSON(200, gin.H{"msg": "hello,world"})
	})
	//服务器端口
	ginServer.Run(":8082")
}
```

目录结构

![image-20230420233551613](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230420233551613.png)

访问[localhost:8082/hello](http://localhost:8082/hello)

![image-20230420233515574](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230420233515574.png)

## 添加icon

效果

![image-20230420234624121](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230420234624121.png)

实现代码

![image-20230420234700711](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230420234700711.png)

```go
package main

import "github.com/gin-gonic/gin"
import "github.com/thinkerou/favicon"

func main() {
	//创建服务
	ginServer := gin.Default()
	ginServer.Use(favicon.New("./彩虹.png"))
	//访问地址，处理请求
	ginServer.GET("/hello", func(context *gin.Context) {
		context.JSON(200, gin.H{"msg": "hello,world"})
	})
	//服务器端口
	ginServer.Run(":8082")
}
```

## 利用restful形式开发

```go
package main

import "github.com/gin-gonic/gin"
import "github.com/thinkerou/favicon"

func main() {
	//1.创建服务
	ginServer := gin.Default()
	ginServer.Use(favicon.New("./彩虹.png"))
	//2.访问地址，处理请求
	//2.1 利用restful形式开发接口，并以json的形式返回
	ginServer.GET("/hello", func(context *gin.Context) {
		context.JSON(200, gin.H{"msg": "hello,get"})
	})
	ginServer.POST("/hello", func(context *gin.Context) {
		context.JSON(200, gin.H{"msg": "hello,post"})
	})
	ginServer.DELETE("/hello", func(context *gin.Context) {
		context.JSON(200, gin.H{"msg": "hello,DELETE"})
	})
	ginServer.PUT("/hello", func(context *gin.Context) {
		context.JSON(200, gin.H{"msg": "hello,PUT"})
	})
	//2.2 返回页面
	//3.配置服务器端口
	ginServer.Run(":8082")
}
```

测试都是成功的

![image-20230420235703771](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230420235703771.png)

## 返回页面

返回了页面并且后端向前端传输了数据





**目录结构：**

![image-20230421001105623](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421001105623.png)

**代码实现：**

index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>这是Gin的页面</title>
</head>
<body>
    <h1>hello Gin</h1>
<!--    下面这个代码可以访问key-value类型的数据-->
        {{.msg}}
</body>
</html>
```

main.go

```go
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)
import "github.com/thinkerou/favicon"

func main() {
	//1.创建服务
	ginServer := gin.Default()
	ginServer.Use(favicon.New("./彩虹.png"))
	//加载静态页面到web服务器中
	//下面这行代码的意思是加载./templates/*目录下的全部文件
	ginServer.LoadHTMLGlob("templates/*")
	//2.访问地址，处理请求
	// 返回页面
	ginServer.GET("/index", func(context *gin.Context) {
		//第一个参数是响应码，这是个常量200
		//第二个参数是templates下返回的页面
		//第三个参数是后端传递给前端的数据
		context.HTML(http.StatusOK, "index.html", gin.H{"msg": "这是后端传送的数据"})
	})
	//3.配置服务器端口
	ginServer.Run(":8082")
}
```

![image-20230421001044000](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421001044000.png)

## 引入样式和脚本文件

**目录结构**

![image-20230421002757026](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421002757026.png)

**代码实现**

index.html

![image-20230421002825274](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421002825274.png)

main.go

![image-20230421002915406](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421002915406.png)

main.css

![image-20230421002951695](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421002951695.png)

index.js

![image-20230421003002436](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421003002436.png)

## 前后端传参

### 前端通过url方式传参给后端

前端通过url方式传参**并且**后端接收到参数回显到前端

代码实现:

![image-20230421115636103](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421115636103.png)

运行效果：

![image-20230421115557480](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421115557480.png)

### 前端通过restful方式传参给后端

代码实现：

![image-20230421120221601](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421120221601.png)

运行效果：

![image-20230421120301494](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421120301494.png)



### 前端通过发送json的方式传参给后端

接收代码

![image-20230421163632521](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421163632521.png)

传参：

![image-20230421163818257](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421163818257.png)





### 前端通过form表单的形式给后端传输数据

**注意这里使用了我们之前写的index接口，才可以访问到index.html页面的**

代码实现：

后端

![image-20230421165141813](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421165141813.png)

前端

![image-20230421165213566](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421165213566.png)

实际效果：

![image-20230421165006005](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421165006005.png)

![image-20230421165024010](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421165024010.png)

## 路由

### 重定向

后端代码实现：

```go
//路由
	ginServer.GET("/test", func(context *gin.Context) {
		//重定向
		//context.Redirect(301, "https://xiaobaicai350.github.io/")
        //需要注意的是第一个参数也就是状态码，必须和业务处理的状态一样
		context.Redirect(http.StatusMovedPermanently, "https://xiaobaicai350.github.io/")
	})
```



![image-20230421165718465](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421165718465.png)

重定向到了我的博客

![image-20230421165743854](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421165743854.png)

### 404

目录结构

![image-20230421170557775](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421170557775.png)

main.go

![image-20230421170421992](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421170421992.png)

404.html

![image-20230421170536251](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421170536251.png)



访问一个**随便的后缀**，后端会帮我们拦截到，然后转发到404页面

![image-20230421170510813](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421170510813.png)

### 路由组

说白了就是加个前缀

```go
	//路由组
	userGroup := ginServer.Group("/user")
	{
		//localhost:8082/user/add
		userGroup.GET("/add", func(context *gin.Context) {
			context.HTML()//这里可以写具体的处理，之前已经写过很多了，这里就不再写了
		})
		//localhost:8082/user/logout
		userGroup.GET("/logout")
		//localhost:8082/user/login
		userGroup.GET("/login")
	}
```

## 中间件

就是Java里面的**拦截器**

定义我们自己的拦截器：

![image-20230421173344197](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421173344197.png)

使用拦截器：

![image-20230421173722877](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421173722877.png)





验证结果：

访问http://localhost:8082/testHandler

![image-20230421173307744](https://raw.githubusercontent.com/Xiaobaicai350/picBed/master/xiaobaicai/image-20230421173307744.png)
