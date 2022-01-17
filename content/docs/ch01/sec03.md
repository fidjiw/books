---
title: 1.3 数组
weight: 3
---

## 1.3 数组

数组是一系列同一类型数据的集合

数组中包含的数据被称为数组元素，数组元素可以是任意的原始类型，如：int、string，也可以是用户自定义的类型

一个数组包含的元素个数被称为数组的长度

Golang中数组是一个长度固定的数据类型，**数组的长度是类型的一部分** ，[5]int 和 [8]int 是两个不同的类型，数组长度一旦定义，就不能再改变了

Golang中数组占用内存是连续的，数组元素被分配到连续的内存地址，所以索引数组元素的速度非常快

> - 数组是值类型，赋值和传参会复制整个数组，所以改变副本的值，不会改变本身的值
> - 数组支持 “==“、 ”!=” 操作符， 因为内存总是被初始化过的  
> - [n]*T 表示指针数组， *[n]T 表示数组指针  

### 1.31 基本语法：

```
package main

func main() {
	// 定义一个长度为 3 元素类型为 string 的数组 a
	var a [3]string
	a[0] = "hello"
	// 定义一个长度为 3 元素类型为 int 的数组 b 并赋值
	var b [3]int
	b[0] = 80
	b[1] = 100
	b[2] = 96

}
```

### 1.32 定义数组

var 数组变量名 [元素数量]元素类型

比如：var a [5]int  



### 1.33 初始化数组

方法1

```go
package main

import "fmt"

func main() {
	var stringArray [3]string                         //数组会初始化为 string 类型的空值 []
	var numArray = [3]int{1, 2}                 //定义并赋值进行初始化
	var cityArray = [3]string{"北京", "上海", "深圳"} //定义并赋值进行初始化
	fmt.Println(stringArray)                      //[   ]
	fmt.Println(numArray)                       //[1 2 0]
	fmt.Println(cityArray) //[北京 上海 深圳]
}

```

方法2

```go
package main

import "fmt"

func main() {
	var testArray [3]int
	var numArray = [...]int{1, 2} //让编译器根据初始化的个数来自行推断数组的长度
	var cityArray = [...]string{"北京", "上海", "深圳"}
	fmt.Println(testArray) //[0 0 0]
	fmt.Println(numArray) //[1 2]
	fmt.Printf("type of numArray:%T\n", numArray) //type of numArray:[2]int
	fmt.Println(cityArray) //[北京 上海 深圳]
	fmt.Printf("type of cityArray:%T\n", cityArray) //type of cityArray:[3]string
}

```

方法3

```go
package main

import "fmt"

func main() {
	a := [...]int{1: 1, 3: 5} //使用指定索引值来初始化数组
	fmt.Println(a) // [0 1 0 5]
	fmt.Printf("type of a:%T\n", a) //type of a:[4]int
}
```



### 1.34 遍历数组

方法1

```go
package main

import "fmt"

func main() {
	var a = [...]string{"北京", "上海", "深圳"}
	for i := 0; i < len(a); i++ { //使用for循环遍历数组
		fmt.Println(a[i])
	}
}
```

方法2

```go
package main

import "fmt"

func main() {
	var a = [...]string{"北京", "上海", "深圳"}
	for index, value := range a { //使用for range遍历数组
		fmt.Println(index, value)
	}
}
```



### 1.35 多维数组

以二维数组为例

定义：var 数组变量名 [元素数量] [元素数量]元素类型

二维数组的定义

```go
package main

import "fmt"

func main() {
	a := [3][2]string{
		{"北京", "上海"},
		{"广州", "深圳"},
		{"成都", "重庆"},
	}
	fmt.Println(a) //[[北京 上海] [广州 深圳] [成都 重庆]]
	fmt.Println(a[2][1]) //支持索引取值:重庆
}
```

二维数组的遍历

```go
package main

import "fmt"

func main() {
	a := [3][2]string{
		{"北京", "上海"},
		{"广州", "深圳"},
		{"成都", "重庆"},
	}
	for _, v1 := range a {
		for _, v2 := range v1 {
		fmt.Printf("%s\t", v2)
	}
	fmt.Println()
	}
}
```

> 多维数组只有第一层可以使用 ... 来让编译器推导数组的长度
>
> 值拷贝行为会造成性能问题，通常会建议使⽤用 slice，或数组指针  
>
> 内置函数 len 和 cap 都返回数组的长度



















