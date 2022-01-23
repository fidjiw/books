---
title: 1.9 接口
weight: 9
---

# 1.9 接口

![image-20220123162730896](https://gitee.com/fidjiw/images/raw/master/img/image-20220123162730896.png)

## 1.9.1 Go接口的定义

在 Go 语言中接口（interface） 是一种类型， 一种抽象的类型。 接口（interface） 是一组函数 method 的集合， Go语言中的接口不能包含任何变量。

在 Go语言 中接口中的所有方法都没有方法体， 接口定义了一个对象的行为规范， **只定义规范不实现 **。 接口体现了程序设计的 **多态和高内聚低耦合** 的思想。

在 Go语言 中的接口也是一种数据类型， 不需要显示实现。 只需要一个 **变量含有接口类型中的所有方法** ， 那么这个变量就实现了这个接口

Go语言中每个接口由数个方法组成：

```go
type 接口名 interface{
	方法名 1( 参数列表 1 ) 返回值列表 1
    方法名 2( 参数列表 2 ) 返回值列表 2
    …
}
```

- 接口名： 使用 type 将接口定义为自定义的类型名。 Go 语言的接口在命名时， 一般会在单词后面添加 er， 如有写操作的接口叫 Writer， 有字符串功能的接口叫 Stringer 等。 接口名最好要能突出该接口的类型含义。
- 方法名： 当方法名首字母是大写且这个接口类型名首字母也是大写时， 这个方法可以被接口所在的包（package） 之外的代码访问。
- 参数列表、 返回值列表： 参数列表和返回值列表中的参数变量名可以省略。  

例子：定义一个 Usber 接口让 Phone 和 Camera 结构体实现这个接口  

```go
package main

import "fmt"

type Usber interface {
	Start()
	Stop()
}
type Phone struct {
	Name string
}

func (p Phone) Start() {
	fmt.Println(p.Name, "开始工作")
}
func (p Phone) Stop() {
	fmt.Println("phone 停止")
}

type Camera struct {
}

func (c Camera) Start() {
	fmt.Println("相机 开始工作")
}
func (c Camera) Stop() {
	fmt.Println("相机 停止工作")
}
func main() {
	phone := Phone{
		Name: "小米手机",
	}
	var p Usber = phone //phone 实现了 Usb 接口
	p.Start()
	camera := Camera{}
	var c Usber = camera //camera 实现了 Usb 接口
	c.Start()
}
```

> 注意：如果接口里面有方法，必须要通过 **结构体或者自定义类型** 来实现这个接口



## 1.9.2 空接口

Go语言中的接口可以不定义任何方法， 没有定义任何方法的接口就是空接口

**空接口表示没有任何约束， 因此任何类型变量都可以实现空接口  **  

空接口在实际项目中用的是非常多的， 用空接口可以表示任意数据类型

例子：

```go
package main

import "fmt"

func main() {
	// 定义一个空接口 x, x 变量可以接收任意的数据类型
	var x interface{}
	s := "你好 Golang"
	x = s // 让字符串s去实现x这个接口
	fmt.Printf("type:%T value:%v\n", x, x)
	i := 100
	x = i
	fmt.Printf("type:%T value:%v\n", x, x)
	b := true
	x = b
	fmt.Printf("type:%T value:%v\n", x, x)
}
```

### 1.9.2.1 空接口作为函数的参数  

### 1.9.2.2 map的值实现空接口  

### 1.9.2.3 切片实现空接口  



## 1.9.3 类型断言

## 1.9.4 结构体值接收者和指针接收者实现接口的区别  

## 1.9.5 一个结构体实现多个接口  

## 1.9.6 接口嵌套  

