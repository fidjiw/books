---
title: 1.11 反射
weight: 11
---

# 1.11 反射

![image-20220124095450928](https://gitee.com/fidjiw/images/raw/master/img/image-20220124095450928.png)

## 1.11.1反射的引子  

有时我们需要写一个函数， 这个函数有能力 **统一处理各种值类型** ， 而这些类型可能无法共享同一个接口， 也可能布局未知， 也有可能这个类型在我们设计函数时还不存在， 这个时候我们就可以用到反射  

- 空接口可以存储任意类型的变量， 我们如何知道这个空接口保存数据的类型是什么？值是什么？
  - 可以使用类型断言
  - 可以使用反射实现， 也就是在程序运行时动态的获取一个变量的类型信息和值信息  
- 把结构体序列化成 json 字符串， 自定义结构体 Tag 标签的时候就用到了反射  

```go
package main

import (
	"encoding/json"
	"fmt"
)

type Student struct {
	ID     int    `json:"id"`
	Gender string `json:"gender"`
	Name   string `json:"name"`
	Sno    string `json:"sno"`
}

func main() {
	var s1 = Student{
		ID:     1,
		Gender: "男",
		Name:   "李四",
		Sno:    "s0001",
	}
	var s, _ = json.Marshal(s1)
	jsonStr := string(s)
	fmt.Println(jsonStr)
}
```

- ORM 框架用到了反射技术  

ORM:对象关系映射（ Object Relational Mapping， 简称 ORM） 是通过使用描述对象和数据库之间映射的元数据， 将面向对象语言程序中的对象自动持久化到关系数据库中  

## 1.11.2 反射的基本介绍



## 1.11.3 reflect.TypeOf()  

## 1.11.4 reflect.ValueOf()  

## 1.11.5 结构体反射  

## 1.11.6 反射三大定律

