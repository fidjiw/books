---
title: 1.2 流程控制
weight: 2
---

# 1.2 流程控制

![image-20220122151047485](https://gitee.com/fidjiw/images/raw/master/img/image-20220122151047485.png)



流程控制在编程语言中充当着重要的角色，它控制着程序的逻辑走向和执行次序。

这一小节介绍Go语言中常用的流程控制语句 **if** 和 **for** 、遍历语句 **for range** 以及为了简化代码而生的 **switch** 和 **goto** ，最后介绍两个中断循环的语句 **break** 和 **continue** 。

> 注意：Go语言是没有while的

## 1.2.1 if else

直接看例子

```go
package main
import (
	"fmt"
)
func main() {
	ifDemo()
}

func ifDemo() {
	score := 65
    if score >= 90 {   //if语句中的左{必须与if同行
		fmt.Println("A")
    } else if score > 75 {   //else语句必须与上一个if语句的右括号}以及else的左括号{同行
		fmt.Println("B")
	} else {
		fmt.Println("C")
	}
}
```

> 注意：Go语言中规定与if匹配的左括号必须与if和表达式放在同一行，否则会出现编译错误，与else匹配的{也必须与else放在同一行，而且else还要与if的右边括号}同行。

### 1.2.1.1 if语句特殊写法

```go
package main
import (
	"fmt"
)
func main() {
	ifDemo()
}

func ifDemo() {
	if score := 56; score >= 90 {
		fmt.Println("A")
	} else if score > 75 {
		fmt.Println("B")
	} else {
		fmt.Println("C")
	}
}
```

在if表达式之前添加一个执行语句，在根据语句的变量值进行判断执行。

## 1.2.2 for

Go语言中几乎所有的循环都可以用for语句来实现（可用for语句的变形来实现其他编程语言中的while循环）。

for语句的基本格式：

for 初始语句；条件表达式；结束语句{

​			循环体

}

```go
package main
import (
	"fmt"
)
func main() {
	forDemo()
}

func forDemo() {
	for i := 0; i < 10; i++ {
		fmt.Println(i)
	}
}
```

for语句的变形：

- 省略初始语句（初始语句后面的分号不能省略）

```go
i := 0
	for ; i < 10; i++ {
		fmt.Println(i)
	}
```

- 省略初始语句和结束语句（类似其他编程语言中的while）

```go
i := 0
	for i < 10 {
		fmt.Println(i)
		i++
	}
```

## 1.2.3 for range

Go语言中使用for range来遍历数组、切片、字符串、map、channel。

- 数组、切片、字符串返回索引和值
- map返回键和值
- channel返回通道内的值

```go
package main
import (
	"fmt"
)
func main() {
	Demo()
}

func Demo() {
	str1 := "hello golang"
	for index, value1 := range str1 {
		fmt.Printf("index=%d, value=%c \n", index, value1)
	}
	str2 := "hello golang"
	for _, value2 := range str2 {
		fmt.Printf("val=%c \n", value2)
	}
}

```

## 1.2.4 switch case

使用switch语句对大量的值进行条件判断，Go语言中规定每一个switch中只能有一个default分支。

```go
package main

import "fmt"

func main() {
	Demo()
}

func Demo() {
	extension_name := ".go"
	switch extension_name {
	case ".html":
		fmt.Println("text/html")
		break   //每个case语句可以不写break，不会出现穿透
	case ".css":
		fmt.Println("text/css")
		break
	case ".js":
		fmt.Println("text/javascript")
		break
	default:
		fmt.Println("格式错误")
		break
	}
}
```

一个case可以有多个值，多个case值用逗号分开

```go
package main

import "fmt"

func main() {
	Demo()
}

func Demo() {
	n := 2
	switch n {
	case 1, 3, 5, 7, 9:
		fmt.Println("奇数")
	case 2, 4, 6, 8:
		fmt.Println("偶数")
	default:
		fmt.Println(n)
	}

	//switch n := 7; n {
	//case 1, 3, 5, 7, 9:
	//	fmt.Println("奇数")
	//case 2, 4, 6, 8:
	//	fmt.Println("偶数")
	//default:
	//	fmt.Println(n)
	//}

	age := 56
	switch {
	case age < 25:
		fmt.Println("好好学习吧！ ")
	case age > 25 && age <= 60:
		fmt.Println("好好工作吧！ ")
	case age > 60:
		fmt.Println("好好享受吧！ ")
	default:
		fmt.Println("活着真好！ ")
	}
}
```

### 1.2.4.1 fallthrought

fallthrought可以执行满足条件的case的下一个case，可以理解为穿透到下一个case，是为了兼容C语言中case的设计。

```go
package main

import "fmt"

func main() {
	Demo()
}

func Demo() {
	s := "a"
	switch {
	case s == "a":
		fmt.Println("hello")
		fallthrough   //穿透到下一个case，默认只能穿透一层
	case s == "b":
		fmt.Println("golang")
	case s == "c":
		fmt.Println("hello_world")
	default:
		fmt.Println("...")
	}
}
```

## 1.2.5 goto 

goto语句通过标签进行代码之间的**无条件跳转** ，可以快速地跳出循环，但是滥用goto会使程序可读性变差。

```go
package main
import "fmt"
func main() {
	for i := 0; i < 10; i++ {
		for j := 0; j < 10; j++ {
			if i == 6 {
				// 设置退出标签
				goto breakTag
			}
			fmt.Printf("%v%v\n", i, j)
		}
	}
	return
	// 退出标签
breakTag:
	fmt.Println("结束 for 循环")
}
```



## 1.2.6 break

在循环程序中需要中断退出的时候可以用到break，一般来说，break用于以下几个方面：

- 跳出循环，并执行循环之后的语句
- 跳出case
- 在多重循环中，用label标出想要break的循环



## 1.2.7 continue

continue语句是跳出当前循环，开始下一次的循环，仅限于在for循环中使用。

在continue语句后面添加标签，表示开始标签对应的循环。

```go
package main
import "fmt"
func main() {
	here:
		for i := 0; i < 10; i++ {
			for j := 0; j < 10; j++ {
				if j == 6 {
					continue here
				}
				fmt.Println("i j 的值", i, j)
			}
		}
}
```



































