# 指针

Go 保留了指针，在一定程度上保证了性能，同时为了更好的 GC 和安全考虑，又限制了指针的使用。


<br/>

## 创建
指针常用的两个操作符，一个是取地址付 `&`，另一个是解引用符 `*`。

<br/>

取地址符 `&`，对一个变量进行取地址，会返回对应类型的指针。

```Go
package main

import "fmt"

func main() {
    num := 10

    // 使用取地址符 & 获取变量 num 的地址
    p := &num

    fmt.Println(p)
}
```

指针 p 中存储的是变量 `num` 的地址

![Golang-Golang基础-语法基础-指针-创建-示例-001.png](Golang-Golang基础-语法基础-指针-创建-示例-001.png)

解引用符 `*` 有两个用途，第一个是访问指针所指向的元素，也就是解引用

```Go
package main

import "fmt"

func main() {
    num := 10

    // 使用取地址符 & 获取变量 num 的地址
    p := &num
    // 使用解引用符 * 对指针 p 解引用，获取指针 p 所指向的元素
    rawNum := *p

    fmt.Println(p, rawNum)
}
```

![Golang-Golang基础-语法基础-指针-创建-示例-002.png](Golang-Golang基础-语法基础-指针-创建-示例-002.png)

`p` 是一个指针，对指针类型解引用就能访问到指针所指向的元素。还有一个用途就是声明一个指针。

```Go
package main

import "fmt"

func main() {
    // 声明一个 int 类型的指针
    var p *int

    fmt.Println(p)
}
```

![Golang-Golang基础-语法基础-指针-创建-示例-003.png](Golang-Golang基础-语法基础-指针-创建-示例-003.png)

`*int` 即代表该变量是一个 `int` 类型的指针，不过指针不能光声明，还得初始化，需要为其分配内存，否则就是一个空指针，无法正常使用。妖媚使用取地址符
将其它变量的地址赋值给该指针，要么就使用内置函数 `new` 手动分配。

```Go
package main

import "fmt"

func main() {
    // 声明一个 int 类型的指针
    var p *int

    // 使用 new 函数为指针分配内存
    p = new(int)

    fmt.Println(p, *p)
}
```

![Golang-Golang基础-语法基础-指针-创建-示例-004.png](Golang-Golang基础-语法基础-指针-创建-示例-004.png)

`new` 函数只有一个参数，那就是类型，并返回一个对应类型的指针，函数会为该指针分配内存，并且指针指向对应类型的零值。

```Go
package main

import "fmt"

func main() {
    fmt.Println(*new(int))
    fmt.Println(*new(float64))
    fmt.Println(*new(bool))
    fmt.Println(*new(string))
    fmt.Println(*new([]string))
    fmt.Println(*new([]int))
    fmt.Println(*new([5]int))
    fmt.Println(*new([5]float64))
}
```

![Golang-Golang基础-语法基础-指针-创建-示例-005.png](Golang-Golang基础-语法基础-指针-创建-示例-005.png)


<br/>
<br/>

## 禁止指针运算

在 Go 中是不支持指针运算的，也就是说指针无法偏移

<note>
标准库 <code>unsafe</code> 提供了许多用于低级编程的操作，其中就包括指针运算。
</note>


<br/>
<br/>

## new 和 make

`new` 和 `make` 有点类型，但也有不同。

<br/>

`new`
```Go
func new(Type) *Type
```
* 返回值是类型指针
* 接收参数是类型
* 专用于给指针分配内存空间

`make`
```Go
func make(t Type, size ...IntergerType) Type
```
* 返回值是值，而不是指针
* 接收的第一个参数是类型，不定长参数根据传入的类型的不同而不同
* 专用于给切片、映射表、通道分配内存

```Go
package main

import "fmt"

func main() {
    fmt.Printf("Type = %T\n", new(int))
    fmt.Printf("Type = %T\n", new(string))
    fmt.Printf("Type = %T\n", new([5]int))

    fmt.Printf("Type = %T\n", make([]int, 10, 100))
    fmt.Printf("Type = %T\n", make(map[string]int, 10))
    fmt.Printf("Type = %T\n", make(chan int, 10))
}
```

![Golang-Golang基础-语法基础-指针-new和make-示例-001.png](Golang-Golang基础-语法基础-指针-new和make-示例-001.png)

