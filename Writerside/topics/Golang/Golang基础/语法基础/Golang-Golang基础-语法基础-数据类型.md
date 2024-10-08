# 数据类型

## 布尔类型

布尔类型只有**真值**和**假值**。需要注意，在 Go 中，整数 **0** 并不代表假值，非零整数也不能代表真值，即数字无法代替布尔值进行逻辑判断，两者是完全
不同的类型。

| 类型   | 描述                              |
|------|---------------------------------|
| bool | ```true``` 为真值; ```false``` 为假值 |

<br/>
<br/>

## 整型

Go 中为不同位数的整数分配了不同的类型，主要分为无符号整型和有符号整型。

| 类型            | 描述                                 |
|---------------|------------------------------------|
| ```uint8```   | 无符号 8 位整型                          |
| ```uint16```  | 无符号 16 位整型                         |
| ```uint32```  | 无符号 32 位整型                         |
| ```uint64```  | 无符号 64 位整型                         |
| ```int8```    | 有符号 8 位整型                          |
| ```int16```   | 有符号 16 位整型                         |
| ```int32```   | 有符号 32 位整型                         |
| ```int64```   | 有符号 64 位整型                         |
| ```uint```    | 无符号整型(至少 32 位)                     |
| ```int```     | 整型(至少 32 位)                        |
| ```uintptr``` | 等价于无符号64位整型，但是专用于存放指针运算，用于存放死的指针地址 |

<br/>
<br/>

## 浮点型

```IEEE-754``` 浮点数，主要分为单精度浮点数和双精度浮点数
| 类型 | 描述 |
|----|----|
| ```float32```   |  ```IEEE-754``` 32 位浮点数 |
| ```float64```   |  ```IEEE-754``` 64 位浮点数 |

<br/>
<br/>
<br/>

## 复数类型

| 类型               | 描述        |
|------------------|-----------|
| ```complex64```  | 32 位实数和虚数 |
| ```complex128``` | 64 位实数和虚数 |

<br/>
<br/>

## 字符类型

Go 中字符串完全兼容 ```UTF-8```

| 类型           | 描述                                   |
|--------------|--------------------------------------|
| ```byte```   | 等价 ```uint8``` 可以表示 ```ASCII``` 字符   |
| ```rune```   | 等价 ```int32``` 可以表示 ```Unicode``` 字符 |
| ```string``` | 字符串即字节序列，可以转换为 ```[]byte``` 类型即切片    |

<br/>
<br/>

## 派生类型

| 类型  | 示例                                            |
|-----|-----------------------------------------------|
| 数组  | ```[5]int``` 长度为 5 的整型数组                      |
| 切片  | ```[]float64``` 64 位浮点数切片                     |
| 映射表 | ```map[string]int``` key 为字符串类型，value 为整型的映射表 |
| 结构体 | ```type Node struct {}``` Node 结构体            |
| 指针  | ```*int``` 一个整型指针                             |
| 函数  | ```type f func()``` 一个没有参数、没有返回值的函数类型         |
| 接口  | ```type Node interface {}``` Node 接口          |
| 通道  | ```chan int``` 整型通道                           |

<br/>
<br/>

## 零值

官方文档中零值称为 ```zero value```，零值并不仅仅只是字面上的数字零，而是一个类型的空值或默认值。

| 类型                 | 零值             |
|--------------------|----------------|
| 数字类型               | ```0```        |
| 布尔类型               | ```false```    |
| 字符串类型              | ```""```       |
| 数组                 | 固定长度的对应类型的零值集合 |
| 结构体                | 内部字段都是零值的结构体   |
| 切片、映射表、函数、接口、通道、指针 | ```nil```      |


<br/>
<br/>

## nil

源代码中的 ```nil```，可以看出 ```nil``` 仅仅只是一个变量
```Go
var nil Type
```

<br/>

Go 中的 ```nil``` 并不等同于其它语言的 ```null```，```nil``` 仅仅只是一些类型的零值，并且不属于任何类型，所以 ```nil == nil``` 这样的语句
是无法通过编译的。



