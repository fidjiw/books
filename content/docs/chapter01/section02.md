---
title: 1.2 流程控制
weight: 2
bookToc: false
---

# Golang流程控制介绍

流程控制在编程语言中充当着重要的角色，它控制着程序的逻辑走向和执行次序。

这一小节介绍Go语言中常用的流程控制语句 **if** 和 **for** 、遍历语句 **for range** 以及为了简化代码而生的 **switch** 和 **goto** ，最后两个中断循环的语句 **break** 和 **continue** 。

> 注意：Go语言是没有while的

## 1. if else

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

### if语句特殊写法

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

### 练习

## 2. for

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

### 练习

## 3. for range



## 4.  switch case



## 5.  goto 



## 6. break



## 7. continue



































