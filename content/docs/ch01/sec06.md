---
title: 1.6 函数
weight: 6
---

# 1.6 函数

![hs](https://gitee.com/fidjiw/images/raw/master/img/hs.png)

## 1.6.1 函数定义

函数是组织好的、 可重复使用的、 用于执行指定任务的代码块  

Go 语言中支持： 函数、 匿名函数和闭包  

Go 语言中定义函数使用 func 关键字  

```go
func 函数名(参数)(返回值){
	函数体
}
```

- 函数名： 由字母、 数字、 下划线组成。 但函数名的第一个字母不能是数字。在同一个包内， 函数名也称不能重名（包的概念详见后文） 。
-  参数： 参数由参数变量和参数变量的类型组成， 多个参数之间使用,分隔。
-  返回值： 返回值由返回值变量和其变量类型组成， 也可以只写返回值的类型， 多个返回值必须用()包裹， 并用,分隔。
-  函数体： 实现指定功能的代码块。

```go
func intSum(x int, y int) int {
	return x + y
}
```



函数的参数和返回值都是可选的，可以实现一个既不需要参数也没有返回值的函数

```go
func sayHello() {
	fmt.Println("Hello Golang")
}
```

## 1.6.2 函数调用

定义了函数之后， 我们可以通过函数名()的方式调用函数  

> 注意：调用有返回值的函数时， 可以不接收其返回值





## 1.6.3 函数参数

### 1.6.3.1 类型简写

函数的参数中如果相邻变量的类型相同， 则可以省略类型  

```go
func intSum(x, y int) int {
	return x + y
}
```



### 1.6.3.2 可变参数

可变参数是指函数的参数数量不固定

Go 语言中的可变参数通过在参数名后加...来标识  

> 注意： 可变参数通常要作为函数的最后一个参数  

```go
func intSum(x ...int) int {
    fmt.Println(x) //x 是一个切片
    sum := 0
    for _, v := range x {
    	sum = sum + v
    } 
    return sum
}

ret1 := intSum()
ret2 := intSum(10)
ret3 := intSum(10, 20)
ret4 := intSum(10, 20, 30)
fmt.Println(ret1, ret2, ret3, ret4) //0 10 30 60  
```

固定参数搭配可变参数使用时， 可变参数要放在固定参数的后面  

```go
func intSum2(x int, y ...int) int {
    fmt.Println(x, y)
    sum := x
    for _, v := range y {
    	sum = sum + v
    } 
        return sum
} 

ret5 := intSum2(100)
ret6 := intSum2(100, 10)
ret7 := intSum2(100, 10, 20)
ret8 := intSum2(100, 10, 20, 30)
fmt.Println(ret5, ret6, ret7, ret8) //100 110 130 160
```

本质上， 函数的可变参数是通过切片来实现的  



## 1.6.4 函数返回值

### 1.6.4.1 函数多返回值



### 1.6.4.2 返回值命名



## 1.6.5 函数变量作用域

## 1.6.6 函数类型与变量

## 1.6.7 高阶函数

## 1.6.8 匿名函数和闭包

## 1.6.9 defer

## 1.6.10 内置函数 panic/recover

