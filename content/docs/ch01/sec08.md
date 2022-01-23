---
title: 1.8 channel
weight: 8
---

# 1.8 channel

![image-20220122221731658](https://gitee.com/fidjiw/images/raw/master/img/image-20220122221731658.png)

在这个小节可以对照该博文 -> [Golang为并发而生](https://xfeng.fun/golang%E4%B8%BA%E5%B9%B6%E5%8F%91%E8%80%8C%E7%94%9F/)



## 1.8.1 为什么要使用 goroutine 

需求： 要统计 1-10000000 的数字中那些是素数， 并打印这些素数？

素数： 就是除了 1 和它本身不能被其他数整除的数  

实现方法：

1. 传统方法， 通过一个 for 循环判断各个数是不是素数
2. 使用并发或者并行的方式， 将统计素数的任务分配给多个 goroutine 去完成， 这个时候就用到了 goroutine
3. goroutine 结合 channel   

## 1.8.2 进程、线程与并行、并发  

### 1.8.2.1 进程和线程  

**进程**（ Process） 就是程序在操作系统中的一次执行过程， 是系统进行资源分配和调度的基本单位， 进程是一个动态概念， 是程序在执行过程中分配和管理资源的基本单位， 每一个进程都有一个自己的地址空间  

一个进程至少有 5 种基本状态， 它们是： 创建状态， 就绪状态，执行状态， 阻塞状态， 终止状态  

![image-20220123104754882](https://gitee.com/fidjiw/images/raw/master/img/image-20220123104754882.png)



**线程** 是进程的一个执行实例， 是程序执行的最小单元， 它是比进程更小的能独立运行的基本单位  

一个进程可以创建多个线程， 同一个进程中的多个线程可以并发执行， 一个程序要运行的话至少有一个进程  

![image-20220123104536180](https://gitee.com/fidjiw/images/raw/master/img/image-20220123104536180.png)



### 1.8.2.2 并行和并发  

**并发**： 多个线程同时竞争一个位置， 竞争到的才可以执行， 每一个时间段只有一个线程在执行。
**并行**： 多个线程可以同时执行， 每一个时间段， 可以有多个线程同时执行。

![image-20220123105147673](https://gitee.com/fidjiw/images/raw/master/img/image-20220123105147673.png)

 

通俗的讲：多线程程序在单核 CPU 上面运行就是并发， 多线程程序在多核 CUP 上运行就是并行， 如果线程数大于 CPU 核数， 则多线程程序在多个 CPU 上面运行既有并行又有并发。

![image-20220123105218835](https://gitee.com/fidjiw/images/raw/master/img/image-20220123105218835.png)

![image-20220123105242025](https://gitee.com/fidjiw/images/raw/master/img/image-20220123105242025.png)





## 1.8.3 协程（goroutine）与主线程  

**Go语言中的主线程**： （可以理解为线程/也可以理解为进程） ，在一个 Go程序的主线程上可以起多个协程。 Golang 中多协程可以实现并行或者并发。  

> 提示：可以把并行理解为 **同步** ，把并发理解为 **异步**

**协程**： 可以理解为用户级线程， 这是对内核透明的， 也就是系统并不知道有协程的存在， 是完全由用户自己的程序进行调度的。

 Go语言的一大特色就是从语言层面 **原生支持协程** ， 在函数或者方法前面加 go 关键字就可创建一个协程。 可以说 Golang 中的协程就是 goroutine  

![image-20220123105824787](https://gitee.com/fidjiw/images/raw/master/img/image-20220123105824787.png)

Go语言中的多协程有点类似其他语言中的多线程  

**多协程和多线程**： Go语言中每个 goroutine (协程) 默认占用内存远比 Java 、 C 的线程少。OS 线程（ 操作系统线程） 一般都有固定的栈内存（通常为 2MB 左右） ,一个 goroutine (协程) 占用内存非常小， 只有 2KB 左右， 多协程 goroutine 切换调度开销方面远比线程要少。这也是为什么越来越多的大公司使用 Golang 的原因之一。  

## 1.8.4 Goroutine 与 sync.WaitGroup  

并行执行需求：

在主线程(可以理解成进程)中， 开启一个 goroutine, 该协程每隔 50 毫秒秒输出 "你好 golang"

在主线程中也每隔 50 毫秒输出"你好 golang", 输出 10 次后， 退出程序

要求主线程和goroutine 同时执行  

```go
package main

import (
	"fmt"
	"strconv"
	"time"
)

func test() {
	for i := 1; i <= 10; i++ {
		fmt.Println("test () hello,world " + strconv.Itoa(i))
		time.Sleep(time.Second)
	}
}
func main() {
	go test() // 开启了一个协程
	for i := 1; i <= 10; i++ {
		fmt.Println(" main() hello,golang" + strconv.Itoa(i))
		time.Sleep(time.Second)
	}
}
```

注意主线程执行完毕后即使协程没有执行完毕， 程序都会退出， 所以我们需要对上面代码进行改造

![image-20220123122541036](https://gitee.com/fidjiw/images/raw/master/img/image-20220123122541036.png)

sync.WaitGroup 可以实现主线程等待协程执行完毕

```go
package main

import (
	"fmt"
	"strconv"
	"sync"
	"time"
)

var wg sync.WaitGroup //1、 定义全局的 WaitGroup
func test() {
	for i := 1; i <= 10; i++ {
		fmt.Println("test () 你好 golang " + strconv.Itoa(i))
		time.Sleep(time.Millisecond * 50)
	}
	wg.Done() // 4、 goroutine 结束就登记-1
}
func main() {
	wg.Add(1) //2、 启动一个 goroutine 就登记+1
	go test()
	for i := 1; i <= 2; i++ {
		fmt.Println(" main() 你好 golang" + strconv.Itoa(i))
		time.Sleep(time.Millisecond * 50)
	}
	wg.Wait() // 3、 等待所有登记的 goroutine 都结束
}
```

  

## 1.8.5 启动多个 Goroutine  

在 Go 语言中实现并发就是这样简单， 我们还可以启动多个 goroutine 

使用 sync.WaitGroup 来实现等待 goroutine 执行完毕  

```go
package main

import (
	"fmt"
	"sync"
)

var wg sync.WaitGroup

func hello(i int) {
	defer wg.Done() // 3、goroutine 结束就登记-1
	fmt.Println("Hello Goroutine!", i)
}
func main() {
	for i := 0; i < 10; i++ {
		wg.Add(1) // 1、启动一个 goroutine 就登记+1
		go hello(i)
	}
	wg.Wait() // 2、等待所有登记的 goroutine 都结束
}
```

多次执行上面的代码， 会发现每次打印的数字的顺序都不一致。 这是因为 10 个 goroutine是并发执行的， 而 **goroutine 的调度是随机的**  

## 1.8.6 设置并行运行时的 cup 数量  

Go 运行时的调度器使用 GOMAXPROCS 参数来确定需要使用多少个 OS 线程来同时执行 Go代码。 默认值是机器上的 CPU 核心数。 例如在一个 8 核心的机器上，调度器会把 Go 代码同时调度到 8 个 OS 线程上。  

Go 语言中可以通过 **runtime.GOMAXPROCS()** 函数设置当前程序并发时占用的 CPU 逻辑核心数。  

Go1.5 版本之前， 默认使用的是单核心执行。 Go1.5 版本之后， 默认使用全部的 CPU 逻辑核心数。  

```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	//获取当前计算机上面的 cpu 个数
	cpuNum := runtime.NumCPU()
	fmt.Println("cpuNum=", cpuNum)
	//可以自己设置使用多个 cpu
	runtime.GOMAXPROCS(cpuNum - 1)
	fmt.Println("ok")
}
```



## 1.8.7 Goroutine 统计素数  

需求：要统计 1-120000 的数字中那些是素数  

1、通过传统的 for 循环来统计  

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	start := time.Now().Unix()
	for num := 1; num <= 120000; num++ {
		flag := true //假设是素数
		for i := 2; i < num; i++ {
			if num%i == 0 { //说明该 num 不是素数
				flag = false
				break
			}
		}
		if flag {
			// fmt.Println(num)
		}
	}
	end := time.Now().Unix()
	fmt.Println("普通的方法耗时=", end-start)
}
```

2、goroutine 开启多个协程统计  

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

var wg sync.WaitGroup

func fn1(n int) {
	for num := (n-1)*30000 + 1; num <= n*30000; num++ {
		flag := true //假设是素数
		for i := 2; i < num; i++ {
			if num%i == 0 {
				flag = false
				break
			}
		}
		if flag {
			// fmt.Println(num)
		}
	}
	wg.Done()
}
func main() {
	start := time.Now().Unix()
	for i := 1; i <= 4; i++ {
		wg.Add(1)
		go fn1(i)
	}
	wg.Wait()
	end := time.Now().Unix()
	fmt.Println("普通的方法耗时=", end-start)
}
```

上面我们使用 goroutine 已经能大大的提升性能了， 但是如果我们想统计数据和打印数据同时进行， 这个时候如何实现呢， 这个时候我们就可以使用 channel （管道）  





## 1.8.8 Channel 管道  

管道是 Go在语言级别上提供的 goroutine 间的通讯方式， 我们可以使用 channel 在多个 goroutine 之间传递消息。 如果说 goroutine 是 Go 程序并发的执行体， channel 就是它们之间的连接。 channel 是可以让一个 goroutine 发送特定值到另一个 goroutine 的通信机制。  

Go 语言的并发模型是 CSP（Communicating Sequential Processes） ，提倡 **通过通信共享内存而不是通过共享内存而实现通信 **。

Go 语言中的管道（channel） 是一种特殊的类型。 管道像一个传送带或者队列， 总是遵循 **先入先出（First In First Out）** 的规则， 保证收发数据的顺序。 每一个管道都是一个具体类型的导管， 也就是声明 channel 的时候需要为其指定元素类型。

### 1.8.8.1 channel 类型  

channel 是一种类型， 一种引用类型  

```go
var 变量 chan 元素类型

var ch1 chan int // 声明一个传递整型的管道
var ch2 chan bool // 声明一个传递布尔型的管道
var ch3 chan []int // 声明一个传递 int 切片的管道
```



### 1.8.8.2 创建 channel  

声明的管道后需要使用 make 函数初始化之后才能使用  

```go
make(chan 元素类型, 容量)

//创建一个能存储 10 个 int 类型数据的管道
ch1 := make(chan int, 10)
//创建一个能存储 4 个 bool 类型数据的管道
ch2 := make(chan bool, 4)
//创建一个能存储 3 个[]int 切片类型数据的管道
ch3 := make(chan []int, 3)
```



### 1.8.8.3 channel 操作  

管道有发送（send）、接收(receive）和关闭（close） 三种操作  

发送和接收都使用<-符号  

```go
ch := make(chan int, 3)

1、 发送（将数据放在管道内）
ch <- 10 // 把 10 发送到 ch 中

2、 接收（从管道内取值）
x := <- ch // 从 ch 中接收值并赋值给变量 x
<-ch // 从 ch 中接收值，忽略结果

3、 关闭管道
close(ch)
```

关于关闭管道需要注意的事情是， 只有在通知接收方 goroutine 所有的数据都发送完毕的时候才需要关闭管道  

管道是可以被垃圾回收机制回收的， 它和关闭文件是不一样的， 在结束操作之后关闭文件是必须要做的， 但 **关闭管道不是必须的** 。

关闭后的管道有以下特点：  

- 对一个关闭的管道再发送值就会导致 panic。
- 对一个关闭的管道进行接收会一直获取值直到管道为空。
- 对一个关闭的并且没有值的管道执行接收操作会得到对应类型的零值。
- 关闭一个已经关闭的管道会导致 panic。  

![image-20220123131107029](https://gitee.com/fidjiw/images/raw/master/img/image-20220123131107029.png)



### 1.8.8.4 管道阻塞  

1、无缓冲的管道：

如果创建管道的时候没有指定容量， 那么我们可以叫这个管道为无缓冲的管道  

无缓冲的管道又称为 **阻塞的管道**  

```go
package main

import (
	"fmt"
)

func main() {
	ch := make(chan int)
	ch <- 10
	fmt.Println("发送成功")
}
```

能够通过编译， 但是执行的时候会出现错误  

2、有缓冲的管道：  

解决上面问题的一种方法就是使用有缓冲区的管道  

可以在使用 make 函数初始化管道的时候为其指定管道的容量  

```go
package main

import (
	"fmt"
)

func main() {
	ch := make(chan int, 1)
	ch <- 10
	fmt.Println("发送成功")
}
```

只要管道的容量大于零， 那么该管道就是有缓冲的管道， 管道的容量表示管道中能存放元素的数量  

就像你小区的快递柜只有那么个多格子， 格子满了就装不下了， 就阻塞了， 等到别人取走一个快递员就能往里面放一个  



### 1.8.8.5 从管道循环取值  

当向管道中发送完数据时， 我们可以通过 close 函数来关闭管道  

当管道被关闭时， 再往该管道发送值会引发 panic， 从该管道取值的操作会先取完管道中的值， 再然后取到的值一直都是对应类型的零值 

如何判断一个管道是否被关闭了呢？  

```go
package main

import "fmt"

//循环遍历管道数据

func main() {
	var ch1 = make(chan int, 5)
	for i := 0; i < 5; i++ {
		ch1 <- i + 1
	}
	close(ch1) //关闭管道
	//使用 for range 遍历管道， 当管道被关闭的时候就会退出 for range,
	//如果没有关闭管道 就会报个错误 fatal error: all goroutines are asleep - deadlock!
	//通过 for range 来遍历管道数据 管道没有 key
	for val := range ch1 {
		fmt.Println(val)
	}
}
```

 从上面的例子中我们看到有两种方式在接收值的时候判断该管道是否被关闭， 不过我们通常使用的是 for range 的方式。使用 for range 遍历管道， **当管道被关闭的时候就会退出 for range**  

## 1.8.9 Goroutine 结合 Channel 管道  

## 1.8.10 单向管道  

## 1.8.11 select 多路复用  

## 1.8.12 并发安全和锁  

## 1.8.13 Recover 解决协程中出现的 Panic  

