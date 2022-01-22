---
title: 1.5 map
weight: 5
---

# 1.5 map



![11111](https://gitee.com/fidjiw/images/raw/master/img/11111.png)



## 1.5.1 map介绍

map 是一种无序的基于 key-value 的数据结构

Go 语言中的 map 是引用类型， 必须初始化才能使用  

```go
map[KeyType]ValueType

// KeyType:表示键的类型。
// ValueType:表示键对应的值的类型
```

map 类型的变量默认初始值为 nil， 需要使用 make()函数来分配内存  

```go
make(map[KeyType]ValueType, [cap])

// cap 表示 map 的容量， 该参数虽然不是必须的
```

> 注意：获取 map 的容量不能使用 cap, cap 返回的是 **数组切片分配的空间大小** , 根本不能用于map。 要获取 map 的容量， 可以用 len 函数  



## 1.5.2 map基本使用

map 中的数据都是成对出现的  

```go
scoreMap := make(map[string]int, 8)
scoreMap["张三"] = 90
scoreMap["小明"] = 100
```

map 也支持在声明的时候填充元  

```go
userInfo := map[string]string{
    "username": "admin",
    "password": "123456",
}
```



## 1.5.3 判断某个键是否存在

Go 语言中有个判断 map 中键是否存在的特殊写法  

```go
value, ok := map 对象[key]
```



```go

scoreMap := make(map[string]int)
scoreMap["张三"] = 90
scoreMap["小明"] = 100
// 如果 key 存在 ok 为 true,v 为对应的值； 不存在 ok 为 false,v 为值类型的零值
v, ok := scoreMap["张三"]
if ok {
	fmt.Println(v)
} else {
	fmt.Println("查无此人")
}
```



## 1.5.4 map遍历

Go 语言中使用 for range 遍历 map  

```go
scoreMap := make(map[string]int)
scoreMap["张三"] = 90
scoreMap["小明"] = 100
scoreMap["娜扎"] = 60
for k, v := range scoreMap {
	fmt.Println(k, v)
}
```

只遍历key

```go
scoreMap := make(map[string]int)
scoreMap["张三"] = 90
scoreMap["小明"] = 100
scoreMap["娜扎"] = 60
for k := range scoreMap {
	fmt.Println(k)
}
```

> 注意：遍历 map 时的元素顺序与添加键值对的顺序无关  

按照指定顺序遍历map

```go
rand.Seed(time.Now().UnixNano()) //初始化随机数种子

var scoreMap = make(map[string]int, 200)

for i := 0; i < 100; i++ {
    key := fmt.Sprintf("stu%02d", i) //生成 stu 开头的字符串
    value := rand.Intn(100) //生成 0~99 的随机整数
    scoreMap[key] = value
} 

//取出 map 中的所有 key 存入切片 keys
var keys = make([]string, 0, 200)
for key := range scoreMap {
	keys = append(keys, key)
} 

//对切片进行排序
sort.Strings(keys)

//按照排序后的 key 遍历 map
for _, key := range keys {
	fmt.Println(key, scoreMap[key])
}
```



## 1.5.5 使用delete函数删除键值对

使用 delete()内建函数从 map 中删除一组键值对  

```go
delete(map对象, key)

// map 对象:表示要删除键值对的 map 对象
// key:表示要删除的键值对的键
```

```go
scoreMap := make(map[string]int)
scoreMap["张三"] = 90
scoreMap["小明"] = 100
scoreMap["娜扎"] = 60
delete(scoreMap, "小明")//将小明:100 从 map 中删除
for k,v := range scoreMap{
	fmt.Println(k, v)
}
```



## 1.5.6 元素为map类型的切片

```go
var mapSlice = make([]map[string]string, 3)
for index, value := range mapSlice {
	fmt.Printf("index:%d value:%v\n", index, value)
} 
fmt.Println("after init")
// 对切片中的 map 元素进行初始化
mapSlice[0] = make(map[string]string, 10)
mapSlice[0]["name"] = "小王子"
mapSlice[0]["password"] = "123456"
mapSlice[0]["address"] = "海淀区"
for index, value := range mapSlice {
	fmt.Printf("index:%d value:%v\n", index, value)
}
```



## 1.5.7 值为切片类型的map

```go
var sliceMap = make(map[string][]string, 3)
fmt.Println(sliceMap)
fmt.Println("after init")
key := "中国"
value, ok := sliceMap[key]
if !ok {
	value = make([]string, 0, 2)
}
value = append(value, "北京", "上海")
sliceMap[key] = value
fmt.Println(sliceMap)
```

