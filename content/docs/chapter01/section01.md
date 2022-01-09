---
title: 1.1 数据类型
weight: 1
---

# 1.1 数据类型

Go语言中数据类型分为两种：

- 基本数据类型
  - 整型
  - 浮点型
  - 布尔型
  - 字符串型

- 复合数据类型
  - 数组
  - 切片
  - map
  - 函数
  - 结构体
  - channel
  - 接口


这一小节介绍基本数据类型，复合数据类型放在下面的章节介绍。

## 1.1.1 整型

其中整型又分为两大类：

- 有符号整型：int8、int16、int32、int64等
- 无符号整型：uint8、uint16、uint32、uint64等

具体可参考下表

| 类型   | 范围                                                         | 占用空间 | 有无符号 |
| ------ | ------------------------------------------------------------ | -------- | -------- |
| int8   | (-128 到 127) -2^7 到 2^7-1                                  | 1个字节  | 有       |
| int16  | (-32768 到 32767) -2^15 到 2^15-1                            | 2个字节  | 有       |
| int32  | (-2147483648 到 2147483647) -2^31 到 2^31-1                  | 4个字节  | 有       |
| int64  | (-9223372036854775808 到 9223372036854775807) -2^63 到 2^63-1 | 8个字节  | 有       |
| uint8  | (0 到 255) 0 到 2^8-1                                        | 1个字节  | 无       |
| uint16 | (0 到 65535) 0 到 2^16-1                                     | 2个字节  | 无       |
| uint32 | (0 到 4294967295) 0 到 2^32-1                                | 4个字节  | 无       |
| uint64 | (0 到 18446744073709551615) 0 到 2^64-1                      | 8个字节  | 无       |

### 特殊整型

| 类型    | 描述                                                    |
| ------- | ------------------------------------------------------- |
| uint    | 32 位操作系统上就是 uint32， 64 位操作系统上就是 uint64 |
| int     | 32 位操作系统上就是 int32， 64 位操作系统上就是 int64   |
| uintptr | 无符号整型， 用于存放一个指针                           |

注意：在使用int和uint的时候，应该考虑在不同平台上的差异。在涉及二进制传输时，为了保持文件的结构不受编译平台的影响，不要使用int和uint。

```go
package main
import (
	"fmt"
)

func main() {
	var num int
	num = 666
	fmt.Printf("值:%v 类型%T", num, num) //值:666 类型int
}
```

### 不同int长度的转换

```go
package main
import (
	"fmt"
)
func main() {
	var num1 int8
	num1 = 127
	num2 := int32(num1)
	fmt.Printf("值:%v 类型%T", num2, num2) //值:127 类型 int32
}
```

## 1.1.2 浮点型

Go语言支持两种浮点数：float32和float64

这两种浮点型数据格式遵循[IEEE 754标准](http://www.softelectro.ru/ieee754_en.html)。

```go
package main
import (
	"fmt"
	"math"
)
func main() {
	fmt.Printf("%f\n", math.Pi) //默认保留 6 位小数 (math.Pi时圆周率)
	fmt.Printf("%.2f\n", math.Pi) //保留 2 位小数
}
```

Go语言中浮点数默认为：float64

```go
package main
import (
	"fmt"
)
func main() {
	num := 1.1
	fmt.Printf("值： %v--类型:%T", num, num) //值： 1.1--类型:float64
}
```

### float精度丢失问题

与其他编程语言一样，Go语言中的float也有精度丢失的问题，在定长条件下，二进制小数和十进制小数互相转换可能会有精度丢失。

```go
package main
import (
	"fmt"
)
func main() {
	a := 1228.6
	fmt.Println((a * 100)) //输出： 122859.99999999999

	var b float64 = 1228.6
	fmt.Println((b * 100)) //输出： 122859.99999999999

	m1 := 8.8
	m2 := 2.2
	fmt.Println(m1 - m2) // 期望是 6.6， 结果打印出了 6.6000000000000005
}
```

使用[第三方包](https://github.com/shopspring/decimal)解决精度问题

## 1.1.3 布尔型

Go语言中布尔型有true（真）和false（假）两个

- 布尔型变量的默认值为false
- Go语言中不允许将整型强制转换为布尔型
- 布尔型不能参与算数运算，也不能与其他类型转换

```go
package main
import (
	"fmt"
	"unsafe"
)
func main() {
	var a = true
	fmt.Println(a, "占用字节： ", unsafe.Sizeof(a)) //unsafe.Sizeof(a)返回a变量占用的字节
}
```

## 1.1.4 字符串

所有编程语言中，字符串都是比较重要的。Go语言中字符串以原生数据类型出现，使用字符串就像使用其他原生数据类型（int、bool）一样。Go语言中的字符串内部实现使用的是UTF-8编码。

### 字符串转义

常见的转义有：回车、换行、单双引号、制表符等等。

| 转义符 | 含义   |
| ------ | ------ |
| \r     | 回车符 |
| \n     | 换行符 |
| \t     | 制表符 |
| \ '    | 单引号 |
| \ "    | 双引号 |
| \\ \   | 反斜杠 |

### 字符串常用操作

| 方法                                | 介绍       |
| ----------------------------------- | ---------- |
| len(str)                            | 求长度     |
| +                                   | 拼接字符串 |
| string.Split                        | 分割字符串 |
| string.contains                     | 判断包含   |
| string.HasPrefix   string.JasSuffix | 判断前后缀 |
| string.Join(a[]string,sep string)   | join连接   |

```go
package main
import (
	"fmt"
)
func main() {
	var str1 = "hello"
	var str2 = "golang"
	fmt.Println(str1 + str2)
	var str3 = fmt.Sprintf("%v %v", str1, str2)
	fmt.Println(str3)
}
```

### byte和rune

组成字符串的元素是字符，字符串用双引号""，字符用单引号''。

- 字节：计算机处理数据的基本单位，用 **B** 来表示，1B=8bit（一个字节=8位）
- 字符：计算机中使用的字母、数字、字、符号（一个汉字占3个字节、一个字母占1个字节）

```go
package main
import (
	"fmt"
)
func main() {
	a := "z"
	fmt.Println(len(a)) //1
	b := "张"
	fmt.Println(len(b)) //3
}
```

Go语言中的字符有两种：

- uint8（byte）：代表一个ASCII码的字符
- rune：代表一个UTF-8字符

字符串的底层是一个byte数组，可以和 []byte 类型转换。

字符串、byte、rune的关系：

- 字符串由byte字节组成，字符串的长度是byte字节的长度
- 一个rune字符由一个或多个byte组成



















