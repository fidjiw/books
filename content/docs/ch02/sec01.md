---
title: 2.1 Gin框架
weight: 1
---

# 2.1 Gin框架

## 2.1.1 Gin介绍

![gin框架](https://gitee.com/fidjiw/images/raw/master/img/image-20220121092518744.png)



Gin 是一个 Go (Golang) 编写的轻量级 http web 框架， 运行速度非常快， 如果你是性能和高效的追求者， 可以使用 Gin 框架。

Gin 最擅长的就是  **Api接口的高并发** ， 如果项目的规模不大， 业务相对简单， 这个时候我们也推荐您使用 Gin。

当某个接口的性能遭到较大挑战的时候， 还是可以考虑使用 Gin 重写接口。

Gin 也是一个流行的 golang Web 框架， Github Strat 量已经超过了 50k。

Gin 的官网： https://gin-gonic.com/zh-cn/

Gin Github 地址： https://github.com/gin-gonic/gin  



## 2.1.2 Gin环境搭建

下载安装gin

```go
go get -u github.com/gin-gonic/gin
```

新建main.go配置路由

```go
package main

import "github.com/gin-gonic/gin"

func main() {
	// 创建一个默认的路由引擎
	r := gin.Default()
	// 配置路由
	r.GET("/", func(c *gin.Context) {
		c.JSON(200, gin.H{ // c.JSON： 返回 JSON 格式的数据
			"你好": "Hello world!",
		})
	})
	// 启动 HTTP 服务， 默认在 0.0.0.0:8080 启动服务
	r.Run(":9000") //改变默认启动的端口
}
```



## 2.1.3 Go程序的热加载

所谓热加载就是当我们对代码进行修改时， 程序能够自动重新加载并执行， 这在我们开发中是非常便利的， 可以快速进行代码测试， 省去了每次手动重新编译

beego 中我们可以使用官方给我们提供的 bee 工具来热加载项目， 但是 gin 中并没有官方提供的热加载工具， 这个时候我们要实现热加载就可以借助第三方的工具 

- 工具 1（推荐） ： https://github.com/gravityblast/fresh  
- 工具 2： https://github.com/codegangsta/gin  

```go
go get github.com/pilu/fresh
fresh
```

 ```go
 go get -u github.com/codegangsta/gin
 gin run main.go
 ```



## 2.1.4 Gin路由

### 2.1.4.1 路由概述

路由（Routing） 是由一个 URI（或者叫路径） 和一个特定的 HTTP 方法（GET、 POST 等）组成的， 涉及到应用如何响应客户端对某个网站节点的访问  

RESTful API 是目前比较成熟的一套互联网应用程序的 API 设计理论， 我们设计路由的时候建议参考 RESTful API 指南  

在 RESTful 架构中， 每个网址代表一种资源， 不同的请求方式表示执行不同的操作  

| GET（SELECT）    | 从服务器取出资源（一项或多项）                 |
| ---------------- | ---------------------------------------------- |
| POST（CREATE）   | 在服务器新建一个资源                           |
| PUT（UPDATE）    | 在服务器更新资源（客户端提供改变后的完整资源） |
| DELETE（DELETE） | 从服务器删除资源                               |

### 2.1.4.2 简单路由配置

1. get请求

```go
r.GET("/", func(c *gin.Context) {
		c.String(200, "Get")
	})
```

2. post请求

```go
r.POST("/", func(c *gin.Context) {
		c.String(200, "Get")
	})
```

3. PUT请求

```go
r.PUT("/", func(c *gin.Context) {
		c.String(200, "Get")
	})
```

4. DELETE请求

```go
r.DELETE("/", func(c *gin.Context) {
		c.String(200, "Get")
	})
```

5. 路由里面获取 Get 传值  

```go
package main

import "github.com/gin-gonic/gin"

func main() {
	// 创建一个默认的路由引擎
	r := gin.Default()
	// 配置路由
	r.GET("/hello", func(c *gin.Context) {
		goo := c.Query("goo")
		c.String(200, "goo=%s", goo)
	})
	// 启动 HTTP 服务， 默认在 0.0.0.0:8080 启动服务
	r.Run(":9000")
}

// http://127.0.0.1:9000/hello?goo=golang
// goo=golang
```

6. 动态路由

```go
package main

import "github.com/gin-gonic/gin"

func main() {
	// 创建一个默认的路由引擎
	r := gin.Default()
	// 配置路由
	r.GET("/user/:uid", func(c *gin.Context) {
		uid := c.Param("uid")
		c.String(200, "userID=%s", uid)
	})
	// 启动 HTTP 服务， 默认在 0.0.0.0:8080 启动服务
	r.Run(":9000")
}

// http://127.0.0.1:9000/user/20
// userID=20
```



### 2.1.4.3 各种返回数据格式

1. 返回字符串

```go
r.GET("/news", func(c *gin.Context) {
		aid := c.Query("aid")
		c.String(200, "aid=%s", aid)
	})
```



2. 返回JSON数据

```go
r.GET("/someJSON", func(c *gin.Context) {
		// 方式一： 自己拼接 JSON
    	//gin.H 是 map[string]interface{}的缩写
		c.JSON(http.StatusOK, gin.H{"message": "Hello world!"}) 
	})
	r.GET("/moreJSON", func(c *gin.Context) {
		// 方法二： 使用结构体
		var msg struct {
			Name    string `json:"user"`
			Message string
			Age     int
		}
		msg.Name = "golang"
		msg.Message = "Hello world!"
		msg.Age = 18
		c.JSON(http.StatusOK, msg)
	})
```



3. 返回JSONP格式数据（JSONP主要用于跨域请求）



```go
r.GET("/JSONP", func(c *gin.Context) {
		data := map[string]interface{}{
			"foo": "bar",
		}
		//JSONP?callback=x
		//与JSON的区别是可以传入回调函数
		// 将输出： x({\"foo\":\"bar\"})
		c.JSONP(http.StatusOK, data)
	})
```



4. 返回XML数据

```go
r.GET("/someXML", func(c *gin.Context) {
		// 方式一： 自己拼接 JSON
		c.XML(http.StatusOK, gin.H{"message": "Hello world!"})
	})
	r.GET("/moreXML", func(c *gin.Context) {
		// 方法二： 使用结构体
		type MessageRecord struct {
			Name string
			Message string
			Age int
		} 
		var msg MessageRecord
		msg.Name = "golang"
		msg.Message = "Hello world!"
		msg.Age = 18
		c.XML(http.StatusOK, msg)
	})
```



## 2.1.5 Gin HTML模板渲染

### 2.1.5.1 模板放在同一个目录

1. 首先在项目根目录新建 ` templates ` 文件夹（任意名），然后在文件夹中新建 `index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
<h1>这是HTML模板</h1>
<h3>{{.title}}</h3>
</body>
</html>
```

2. Gin中使用 `c.HTML` 渲染模板，渲染模板前需要使用 `LoadHTMLGlob()` 或 `LoadHTMLFiles()` 方法加载模板

```go
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)

func main() {
	router := gin.Default()
	router.LoadHTMLGlob("templates/*") //加载html模板
	//router.LoadHTMLFiles("templates/*", "templates/*")
	router.GET("/", func(c *gin.Context) {
		c.HTML(http.StatusOK, "index.html", gin.H{
			"title": "首页",
		})
	})
	router.Run(":9000") //改变默认启动的端口
}

```

![image-20220121123140615](https://gitee.com/fidjiw/images/raw/master/img/image-20220121123140615.png)





### 2.1.5.2 模板放在不同目录

情景：不同目录下有同名模板

这个时候就不能用 * 全部引入了

解决方法：定义模板的时候需要通过 `define`  来定义名称

> 注意：<!-- 相当于给模板定义一个名字 define end 成对出现-->  

下面的文件路径为：`templates/admin/index.html`  

```html
{{ define "admin/index.html" }}
<!--    <!DOCTYPE html>-->
    <html lang="en">
    <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Document</title>
    </head>
    <body>
      <h1>后台模板</h1>
      <h3>{{.title}}</h3>
    </body>
    </html>
{{ end }}
```

```go
package main

import (
	"github.com/gin-gonic/gin"
	"net/http"
)

func main() {
	router := gin.Default()
	router.LoadHTMLGlob("templates/**/*")

	router.GET("/admin", func(c *gin.Context) {
		c.HTML(http.StatusOK, "index.html", gin.H{
			"title": "后台管理",
		})
	})
	router.Run(":9000") //改变默认启动的端口
```



> 注意：如果模板在多级目录里面的话需要这样配置 r.LoadHTMLGlob("templates/**/**/*") /**表示目录  

### 2.1.5.3 Gin模板基本语法





## 2.1.6 静态文件服务

## 2.1.7 路由详解

## 2.1.8 自定义控制器

## 2.1.9 Gin中间件

## 2.1.10 Gin自定义Model

## 2.1.11 Gin文件上传

## 2.1.12 Gin Cookie

## 2.1.13 Gin Session

## 2.1.14 Gin GORM

