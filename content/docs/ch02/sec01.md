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

> 注意：`<!-- 相当于给模板定义一个名字 define end 成对出现-->  `

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

模板语法都包含在{{和}}中间， 其中{{.}}中的点表示当前对象  

当传入一个结构体对象时， 可以根据.来访问结构体的对应字段  

1. {{.}} 输出数据
2. {{/* a comment */}}   注释
3. {{$obj := .title}}  {{$obj}}   变量
4. {{- .Name -}}  移除空格
5. 比较函数

| 函数 | 作用                       |
| ---- | -------------------------- |
| eq   | 如果 arg1 == arg2 则返回真 |
| ne   | 如果 arg1 != arg2 则返回真 |
| lt   | 如果 arg1 < arg2 则返回真  |
| le   | 如果 arg1 <= arg2 则返回真 |
| gt   | 如果 arg1 > arg2 则返回真  |
| ge   | 如果 arg1 >= arg2 则返回真 |

6. 条件判断

{{if pipeline}} T1 {{end}}

{{if pipeline}} T1 {{else}} T0 {{end}}

{{if pipeline}} T1 {{else if pipeline}} T0 {{end}}

{{if gt .score 60}}
及格
{{else}}
不及格  
{{end}}

{{if gt .score 90}}
优秀
{{else if gt .score 60}}
及格
{{else}}
不及格
{{end}}  

7. range 遍历（pipeline 的值必须是数组、 切片、 字典或者通道）

```go
{{range $key,$value := .obj}}
	{{$value}}
{{end}}
```

```go
{{$key,$value := .obj}}
	{{$value}}
{{else}}
	pipeline 的值其长度为 0
{{end}}
```

8. with

```go
user := UserInfo{
		Name: "张三",
		Gender: "男",
		Age: 18,
	} 
router.GET("/", func(c *gin.Context) {
	c.HTML(http.StatusOK, "default/index.html",map[string]interface{}{
		"user": user,
		})
	})

{{with .user}}
		<h4>姓名： {{.Name}}</h4>
		<h4>性别： {{.user.Gender}}</h4>
		<h4>年龄： {{.Age}}</h4>
{{end}}
```



## 2.1.6 静态文件服务

当我们渲染的 HTML 文件中引用了静态文件时,我们需要配置静态 web 服务  

r.Static("/static", "./static") 前面的/static 表示路由 后面的./static 表示路径  

```go
func main() {
	router := gin.Default()
	router.Static("/static", "./static")
	router.LoadHTMLGlob("templates/**/*")
	// ...
	router.Run(":9000") //改变默认启动的端口
}
```



## 2.1.7 路由详解

路由（Routing） 是由一个 URI（或者叫路径） 和一个特定的 HTTP 方法（GET、 POST 等）组成的， 涉及到应用如何响应客户端对某个网站节点的访问  

前面介绍了路由基础以及路由配置， 这里详细讲讲路由传值、 路由返回值  

### 2.1.7.1 GET/POST

1. Get请求传值

http://127.0.0.1:9000/user?uid=20&page=1

```go
package main

import "github.com/gin-gonic/gin"

func main() {
	// 创建一个默认的路由引擎
	r := gin.Default()
	// 配置路由
	r.GET("/user", func(c *gin.Context) {
		uid := c.Query("uid")
		page := c.DefaultQuery("page", "0")
		c.String(200, "uid=%v page=%v", uid, page)
	})
	r.Run(":9000") //改变默认启动的端口
}

```

2. 动态路由传值

http://127.0.0.1:9000/user/20

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
	r.Run(":9000") //改变默认启动的端口
}

```

3. Post 请求传值 获取 form 表单数据  



3. 获取 GET POST 传递的数据绑定到结构体  
4. 获取 Post Xml 数据  

### 2.1.7.2 简单的路由组

### 2.1.7.3 路由文件、分组



## 2.1.8 自定义控制器

### 2.1.8.1 控制器分组

当我们的项目比较大的时候有必要对我们的控制器进行分组  

新建 controller/admin/NewsController.go  



### 2.1.8.2 控制器继承



## 2.1.9 Gin中间件

Gin 框架允许开发者在处理请求的过程中， 加入用户自己的钩子（Hook） 函数。 这个钩子函数就叫中间件， 中间件适合处理一些公共的业务逻辑， 比如登录认证、 权限校验、 数据分页、记录日志、 耗时统计等。

> 通俗的讲： 中间件就是匹配路由前和匹配路由完成后执行的一系列操作  



### 2.1.9.1 路由中间件

1. 初始中间件

Gin 中的中间件必须是一个 gin.HandlerFunc 类型， 配置路由的时候可以传递多个 func 回调函数， 最后一个 func 回调函数前面触发的方法都可以称为中间件  

```go
package main

import (
	"fmt"
	"github.com/gin-gonic/gin"
)

func initMiddleware(ctx *gin.Context) {
	fmt.Println("我是一个中间件")
}
func main() {
	r := gin.Default()
	//访问路由的时候，首先会走中间件
	//func 回调函数
	r.GET("/", initMiddleware, func(ctx *gin.Context) {
		ctx.String(200, "首页--中间件演示")
	})
	r.GET("/news", initMiddleware, func(ctx *gin.Context) {
		ctx.String(200, "新闻页面--中间件演示")
	})
	r.Run(":8080")
}

```

2. ctx.Next()调用该请求的剩余处理程序  

中间件里面加上 ctx.Next()可以让我们在路由匹配完成后执行一些操作。  

比如我们统计一个请求的执行时间  

```go
package main

import (
	"fmt"
	"github.com/gin-gonic/gin"
	"time"
)

func initMiddleware(ctx *gin.Context) {
	fmt.Println("1-执行中中间件")
	start := time.Now().UnixNano()
	// 调用该请求的剩余处理程序
    // 执行调用initMiddleware方法的方法的剩余代码，再执行ctx.next下面的内容
	ctx.Next()
	fmt.Println("3-程序执行完成 计算时间")
	// 计算耗时 Go 语言中的 Since()函数保留时间值， 并用于评估与实际时间的差异
	end := time.Now().UnixNano()
	fmt.Println(end - start)
}
func main() {
	r := gin.Default()
	r.GET("/", initMiddleware, func(ctx *gin.Context) {
		fmt.Println("2-执行首页返回数据")
		ctx.String(200, "首页--中间件演示")
	})
	r.GET("/news", initMiddleware, func(ctx *gin.Context) {
		ctx.String(200, "新闻页面--中间件演示")
	})
	r.Run(":8080")
}
```

3. 一个路由配置多个中间件的执行顺序  

4. c.Abort()  

Abort 是终止的意思， c.Abort() 表示终止调用该请求的剩余处理程序  

### 2.1.9.2 全局中间件

```go
package main

import (
	"fmt"
	"github.com/gin-gonic/gin"
)

func initMiddleware(ctx *gin.Context) {
	fmt.Println("全局中间件 通过 r.Use 配置")
	// 调用该请求的剩余处理程序
	ctx.Next()
}
func main() {
	r := gin.Default()
	r.Use(initMiddleware)
	r.GET("/", func(ctx *gin.Context) {
		ctx.String(200, "首页--中间件演示")
	})
	r.GET("/news", func(ctx *gin.Context) {
		ctx.String(200, "新闻页面--中间件演示")
	})
	r.Run(":8080")
}
```



### 2.1.9.3 在路由分组中配置中间件



### 2.1.9.4 中间件和对应控制器之间共享数据  

### 2.1.9.5 中间件注意事项

1. gin 默认中间件

gin.Default()默认使用了 Logger 和 Recovery 中间件， 其中：

- Logger 中间件将日志写入 gin.DefaultWriter， 即使配置了 GIN_MODE=release。

- Recovery 中间件会 recover 任何 panic。 如果有 panic 的话， 会写入 500 响应码。

如果不想使用上面两个默认的中间件， 可以使用 gin.New()新建一个没有任何默认中间件的路由。

2. gin 中间件中使用 goroutine

当在中间件或 handler 中启动新的 goroutine 时， 不能使用原始的上下文（c *gin.Context） ，必须使用其只读副本（c.Copy()）

  

## 2.1.10 Gin自定义Model

1. 关于 Model

如果我们的应用非常简单的话， 我们可以在 Controller 里面处理常见的业务逻辑。 

但是如果我们有一个功能想在多个控制器、 或者多个模板里面复用的话， 那么我们就可以把公共的功能单独抽取出来作为一个模块（Model） 。 

Model 是逐步抽象的过程， 一般我们会在 Model里面封装一些公共的方法让不同 Controller 使用， 也可以在 Model 中实现和数据库打交道。

2. Model 里面封装公共的方法  

新建 models/ tools.go  

```go
package models

import (
	"crypto/md5"
	"fmt"
	"time"
) //时间戳间戳转换成日期

func UnixToDate(timestamp int) string {
	t := time.Unix(int64(timestamp), 0)
	return t.Format("2006-01-02 15:04:05")
}

//日期转换成时间戳 2020-05-02 15:04:05
func DateToUnix(str string) int64 {
	template := "2006-01-02 15:04:05"
	t, err := time.ParseInLocation(template, str, time.Local)
	if err != nil {
		return 0
	}
	return t.Unix()
}
func GetUnix() int64 {
	return time.Now().Unix()
}
func GetDate() string {
	template := "2006-01-02 15:04:05"
	return time.Now().Format(template)
}
func GetDay() string {
	template := "20060102"
	return time.Now().Format(template)
}
func Md5(str string) string {
	data := []byte(str)
	return fmt.Sprintf("%x\n", md5.Sum(data))
}
```

3. 控制器中调用 Model  

```go
package controllers

import (
"gin_demo/models"
) 

day := models.GetDay()
```



## 2.1.11 Gin文件上传

## 2.1.12 Gin Cookie

## 2.1.13 Gin Session

## 2.1.14 Gin GORM

