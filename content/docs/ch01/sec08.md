---
title: 1.8 channel
weight: 8
---

# 1.8 channel

![image-20220122221731658](https://gitee.com/fidjiw/images/raw/master/img/image-20220122221731658.png)

在这个小节可对照该博文 -> [Golang为并发而生](https://xfeng.fun/golang%E4%B8%BA%E5%B9%B6%E5%8F%91%E8%80%8C%E7%94%9F/)



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



## 1.8.5 启动多个 Goroutine  

## 1.8.6 设置并行运行时的 cup 数量  

## 1.8.7 Goroutine 统计素数  

## 1.8.8 Channel 管道  

## 1.8.9 Goroutine 结合 Channel 管道  

## 1.8.10 单向管道  

## 1.8.11 select 多路复用  

## 1.8.12 并发安全和锁  

## 1.8.13 Recover 解决协程中出现的 Panic  

