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



### 1.8.2.2 并行和并发  



## 1.8.3 协程（goroutine）与主线程  

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

