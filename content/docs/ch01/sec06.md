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

Go 语言中通过 return 关键字向外输出返回值  

### 1.6.4.1 函数多返回值

Go 语言中函数支持多返回值

函数如果有多个返回值时必须用()将所有返回值包裹起来  

```go
func calc(x, y int) (int, int) {
    sum := x + y
    sub := x - y
    return sum, sub
}
```



### 1.6.4.2 返回值命名

函数定义时可以给返回值命名，并在函数体中直接使用这些变量， 最后通过 return 关键字返回

  ```go
  func calc(x, y int) (sum, sub int) {
      sum = x + y
      sub = x - y
      return
  }
  ```



## 1.6.5 函数变量作用域

### 1.6.5.1 全局变量

全局变量是定义在函数外部的变量， 它在程序整个运行周期内都有效。 在函数中可以访问到全局变量  



### 1.6.5.2 局部变量

局部变量是函数内部定义的变量， 函数内定义的变量无法在该函数外使用  

如果局部变量和全局变量重名， 优先访问局部变量  



## 1.6.6 函数类型与变量

### 1.6.6.1 定义函数类型

可以使用 type 关键字来定义一个函数类型  

```go
type calculation func(int, int) int
```

上面语句定义了一个 calculation 类型，它是一种函数类型，这种函数接收两个 int 类型的参数并且返回一个 int 类型的返回值  

凡是满足这个条件的函数都是 calculation 类型的函数  

```go
func add(x, y int) int {
	return x + y
} 
func sub(x, y int) int {
	return x - y
}
```

add 和 sub 都能赋值给 calculation 类型的变量  

```go
var c calculation
c = add
```



### 1.6.6.2 函数类型变量

可以声明函数类型的变量并且为该变量赋值  

```go
func main() {
    var c calculation // 声明一个 calculation 类型的变量 c
    c = add // 把 add 赋值给 c
    fmt.Printf("type of c:%T\n", c) // type of c:main.calculation
    fmt.Println(c(1, 2)) // 像调用 add 一样调用 c
    f := add // 将函数 add 赋值给变量 f
    fmt.Printf("type of f:%T\n", f) // type of f:func(int, int) int
    fmt.Println(f(10, 20)) // 像调用 add 一样调用 f
}
```



## 1.6.7 高阶函数

高阶函数分为：

- 函数作为参数
- 函数作为返回值



函数可以作为参数（回调函数）

```go
func add(x, y int) int {
	return x + y
} 
func calc(x, y int, op func(int, int) int) int {
	return op(x, y)
} 
func main() {
	ret2 := calc(10, 20, add)
	fmt.Println(ret2) //30
}
```

函数也可以作为返回值  

```go
func add(x, y int) int {
	return x + y
} 
func sub(x, y int) int {
	return x - y
} 
func do(s string) func(int, int) int {
    switch s {
    case "+":
    	return add
    case "-":
    	return sub
    default:
    	return nil
    }
} 
func main() {
    var a = do("+")
    fmt.Println(a(10, 20))
}
```



## 1.6.8 匿名函数和闭包

### 1.6.8.1 匿名函数

匿名函数就是没有函数名的函数 

```go
func(参数)(返回值){
	函数体
}
```

匿名函数因为没有函数名， 所以没办法像普通函数那样调用， 所以匿名函数需要保存到某个变量或者作为立即执行函数

```go
func main() {
    // 将匿名函数保存到变量
    add := func(x, y int) {
        fmt.Println(x + y)
	} 
    add(10, 20) // 通过变量调用匿名函数
    
    //自执行函数： 匿名函数定义完加()直接执行
    func(x, y int) {
    fmt.Println(x + y)
    }(10, 20)
}
```

匿名函数多用于实现回调函数和闭包  

### 1.6.8.2 闭包

闭包可以理解成 **定义在一个函数内部的函数** 

在本质上，闭包是将函数内部和函数外部连接起来的桥梁，或者说是函数和其引用环境的组合体  

```go
func adder() func(int) int {
    var x int
    return func(y int) int {
		x += y
		return x
	}
} 
func main() {
    var f = adder()
    fmt.Println(f(10)) //10
    fmt.Println(f(20)) //30
    fmt.Println(f(30)) //60
    
    f1 := adder()
    fmt.Println(f1(40)) //40
    fmt.Println(f1(50)) //90
}
```

变量 f 是一个函数并且它引用了其外部作用域中的 x 变量， 此时 f 就是一个闭包 

**在 f 的生命周期内， 变量 x 也一直有效**  

闭包进阶示例 1：  

```go
func adder2(x int) func(int) int {
	return func(y int) int {
		x += y
		return x
	}
} 
func main() {
    var f = adder2(10)
    fmt.Println(f(10)) //20
    fmt.Println(f(20)) //40
    fmt.Println(f(30)) //70
    
    f1 := adder2(20)
    fmt.Println(f1(40)) //60
    fmt.Println(f1(50)) //110
}
```

闭包进阶示例 2：  

```go
func makeSuffixFunc(suffix string) func(string) string {
	return func(name string) string {
		if !strings.HasSuffix(name, suffix) {
			return name + suffix
		} 
        return name
	}
} 

func main() {
    jpgFunc := makeSuffixFunc(".jpg")
    txtFunc := makeSuffixFunc(".txt")
    fmt.Println(jpgFunc("test")) //test.jpg
    fmt.Println(txtFunc("test")) //test.txt
}
```

闭包进阶示例 3：  

```go
func calc(base int) (func(int) int, func(int) int) {
	add := func(i int) int {
		base += i
		return base
	} 
    sub := func(i int) int {
		base -= i
		return base
} 
    return add, sub
} 
func main() {
    f1, f2 := calc(10)
    fmt.Println(f1(1), f2(2)) //11 9
    fmt.Println(f1(3), f2(4)) //12 8
    fmt.Println(f1(5), f2(6)) //13 7
}
```

> 提示：闭包其实并不复杂， 只要牢记闭包=函数+引用环境



## 1.6.9 defer

Go 语言中的 defer 语句会将其后面跟随的语句进行 **延迟处理**  

在 defer **归属的函数** 即将返回时， 将延迟处理的语句按 defer 定义的 **逆序** 进行执行，也就是说，先被 defer 的语句最后被执行，最后被 defer 的语句，最先被执行  

```go
func main() {
    fmt.Println("start")
    defer fmt.Println(1)
    defer fmt.Println(2)
    defer fmt.Println(3)
    fmt.Println("end")
} 

输出结果：
start
end
3 
2 
1
```

由于 defer 语句延迟调用的特性， 所以 defer 语句能非常方便的处理资源释放问题。 比如：资源清理、 文件关闭、 解锁及记录时间等。

### 1.6.9.1 defer执行时机

在 Go 语言的函数中 return 语句在底层并不是 [原子操作](https://www.cnblogs.com/gqymy/p/11470643.html)（不可中断的一个或者一系列操作），它分为给返回值赋值和 RET 指令两步。

而 defer 语句执行的时机就在 **返回值赋值操作后， RET 指令执行前** 。 

具体如下图所示  

![image-20220122160755580](https://gitee.com/fidjiw/images/raw/master/img/image-20220122160755580.png)



## 1.6.10 内置函数 panic/recover

