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

## 2.1.5 GinHTML模板渲染

## 2.1.6 静态文件服务

## 2.1.7 路由详解

## 2.1.8 自定义控制器

## 2.1.9 Gin中间件

## 2.1.10 Gin自定义Model

## 2.1.11 Gin文件上传

## 2.1.12 Gin Cookie

## 2.1.13 Gin Session

## 2.1.14 Gin GORM

