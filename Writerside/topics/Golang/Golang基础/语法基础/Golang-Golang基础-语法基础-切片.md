# 切片

在 Go 中，数组和切片两者看起来长得几乎一模一样，但功能有着不小的区别，数组是定长的数据结构，长度被指定后就不能被改变，而切片是不定长的，切片在容量不
够时会自行扩容。


<br/>

## 数组

如果事先知道了要存放数据的长度，且后续使用中不会有扩容的需求，就可以考虑使用数组，Go 中的数组是**值类型**
，而非引用，并不是指向头部元素的指针。
**（数 组作为值类型，将数组作为参数传递给函数时，由于 Go 中函数是值传递，所以会将整个数组拷贝）**。

### 初始化

数组在声明时长度只能是一个**常量**，不能是变量。

```Go
var 数组名 [长度]数据类型
```

初始化一个长度为 5 的整型数组（数组中每个元素为该类型零值）

```Go
package main

import "fmt"

func main() {
    // 初始化一个长度为 5 的整型数组
    var arr [5]int

    fmt.Printf("arr = %v\n", arr)
}
```

![Golang-Golang基础-语法基础-切片-数组-初始化-示例-001.png](Golang-Golang基础-语法基础-切片-数组-初始化-示例-001.png)

也可以使用元素初始化

```Go
package main

import "fmt"

func main() {
    arr := [5]int{1, 2, 3}

    fmt.Printf("arr = %v\n", arr)
}
```

![Golang-Golang基础-语法基础-切片-数组-初始化-示例-002.png](Golang-Golang基础-语法基础-切片-数组-初始化-示例-002.png)

可以通过 ```new``` 函数获得一个指针

```Go
package main

import "fmt"

func main() {
    arr := new([5]int)

    fmt.Printf("arr = %v\n", arr)
}
```

![Golang-Golang基础-语法基础-切片-数组-初始化-示例-003.png](Golang-Golang基础-语法基础-切片-数组-初始化-示例-003.png)

以上这几种方式都会给 ```arr``` 分配一片固定大小的内存，区别只是使用 ```new``` 得到的是指针。

<br/>

**需要注意的是，在数组初始化时长度必须为常量表达式（常量表达式即表达式的最终结果是一个常量），否则将无法通过编译。**


<br/>
<br/>

### 使用

通过数组名和下标（索引）来访问数组中对应的元素

```Go
package main

import "fmt"

func main() {
    arr := [5]int{1, 2, 3, 4, 5}

    fmt.Println(arr[0], arr[1], arr[2], arr[3], arr[4])
}
```

![Golang-Golang基础-语法基础-切片-数组-使用-示例-001.png](Golang-Golang基础-语法基础-切片-数组-使用-示例-001.png)

也可以通过数组名和下标（索引）来修改数组中对应的元素

```Go
package main

import "fmt"

func main() {
    arr := [5]int{1, 2, 3, 4, 5}
    fmt.Println(arr[0], arr[1], arr[2], arr[3], arr[4])
    
    arr[0], arr[1], arr[2], arr[3], arr[4] = 5, 4, 3, 2, 1
    fmt.Println(arr[0], arr[1], arr[2], arr[3], arr[4])
}
```

![Golang-Golang基础-语法基础-切片-数组-使用-示例-002.png](Golang-Golang基础-语法基础-切片-数组-使用-示例-002.png)

可以通过内置函数 ```len``` 来获取数组中元素的数量

```Go
package main

import "fmt"

func main() {
    arr := [5]int{1, 2, 3, 4, 5}

    fmt.Println(len(arr))
}
```

![Golang-Golang基础-语法基础-切片-数组-使用-示例-003.png](Golang-Golang基础-语法基础-切片-数组-使用-示例-003.png)

可以通过内置函数 ```cap``` 来获取数组容量（数组的容量和数组的长度相同）

```Go
package main

import "fmt"

func main() {
    arr := [5]int{1, 2, 3, 4, 5}

    fmt.Println(cap(arr))
}
```

![Golang-Golang基础-语法基础-切片-数组-使用-示例-004.png](Golang-Golang基础-语法基础-切片-数组-使用-示例-004.png)


<br/>
<br/>

### 切割

切割数组的语法为 ```arr[startIndex:endIndex]```，切割的区间为**左闭右开**

```Go
package main

import "fmt"

func main() {
    arr := [5]int{1, 2, 3, 4, 5}

    fmt.Printf("arr = %v\n", arr)
    fmt.Printf("arr[1:] = %v\n", arr[1:])
    fmt.Printf("arr[:2] = %v\n", arr[:2])
    fmt.Printf("arr[2:3] = %v\n", arr[2:3])
    fmt.Printf("arr[1:3] = %v\n", arr[1:3])
}
```

![Golang-Golang基础-语法基础-切片-数组-切割-示例-001.png](Golang-Golang基础-语法基础-切片-数组-切割-示例-001.png)

数组在切割后，就会变为切片类型

```Go
package main

import "fmt"

func main() {
    arr := [5]int{1, 2, 3, 4, 5}

    fmt.Printf("arr = %v, type = %T\n", arr, arr)
    fmt.Printf("arr[1:3] = %v, type = %T\n", arr[1:3], arr[1:3])
}
```

![Golang-Golang基础-语法基础-切片-数组-切割-示例-002.png](Golang-Golang基础-语法基础-切片-数组-切割-示例-002.png)

若像将数组转为切片类型，不带参数进行切割即可，**转换后的切片于原数组指向的是同一片内存**，修改切片会导致原数组内容的变化

```Go
package main

import "fmt"

func main() {
    arr := [5]int{1, 2, 3, 4, 5}

    fmt.Printf("arr = %v\n", arr)

    slice := arr[:]
    fmt.Printf("slice = %v\n\n", slice)

    arr[0] = 10
    fmt.Printf("arr = %v\n", arr)
    fmt.Printf("slice = %v\n\n", slice)

    slice[2] = 20
    fmt.Printf("arr = %v\n", arr)
    fmt.Printf("slice = %v\n", slice)
}
```

![Golang-Golang基础-语法基础-切片-数组-切割-示例-003.png](Golang-Golang基础-语法基础-切片-数组-切割-示例-003.png)

如果要对转换后的切片进行修改，建议使用下面这种方式进行转换

```Go
package main

import (
    "fmt"
    "slices"
)

func main() {
    arr := [5]int{1, 2, 3, 4, 5}

    fmt.Printf("arr = %v\n", arr)

    slice := slices.Clone(arr[:])
    fmt.Printf("slice = %v\n\n", slice)

    slice[0] = 100
    fmt.Printf("arr = %v\n", arr)
    fmt.Printf("slice = %v\n", slice)
}
```

![Golang-Golang基础-语法基础-切片-数组-切割-示例-004.png](Golang-Golang基础-语法基础-切片-数组-切割-示例-004.png)


<br/>
<br/>

## 切片

在 Go 中，切片的应用要比数组广泛的多，它用于存放不确定长度的数据，且后续使用中可能会频繁地插入和删除元素。
切片的底层实现依旧是数组，是引用类型，可以简单理解为是指向底层数组的指针。

### 初始化

```Go
package main

import "fmt"

func main() {
    // 声明
    var arr []int

    // 初始化
    arr = make([]int, 5)

    fmt.Println(arr)
}
```

![Golang-Golang基础-语法基础-切片-切片-初始化-示例-001.png](Golang-Golang基础-语法基础-切片-切片-初始化-示例-001.png)

```Go
package main

import "fmt"

func main() {
    arr := []int{1, 2, 3}

    fmt.Println(arr)
}
```

![Golang-Golang基础-语法基础-切片-切片-初始化-示例-002.png](Golang-Golang基础-语法基础-切片-切片-初始化-示例-002.png)

```Go
package main

import "fmt"

func main() {
    // make(类型, 元素个数, 容量)
    arr := make([]int, 3, 5)

    fmt.Println(arr)
}
```

![Golang-Golang基础-语法基础-切片-切片-初始化-示例-003.png](Golang-Golang基础-语法基础-切片-切片-初始化-示例-003.png)

```Go
package main

import "fmt"

func main() {
    arr := new([]int)

    fmt.Println(arr)
}
```

![Golang-Golang基础-语法基础-切片-切片-初始化-示例-004.png](Golang-Golang基础-语法基础-切片-切片-初始化-示例-004.png)

通过 ```var arr []int``` 这种方式声明的切片，默认值为 ```nil```，所以不会为其分配内存，而在使用 ```make``` 进行初始化时，建议预分配一个
足够的容量，可以有效减少后续扩容的内存消耗。


<br/>

### 使用

切片的基本使用和数组完全一致，区别只是可以动态变化长度。

<br/>

切片可以通过 ```append``` 函数实现许多操作，函数签名如下，```slice``` 是要添加元素的对象，```elems``` 是待添加的元素，返回值是添加后的切片。

```Go
func append(slice []Type, elems ...Type) []Type
```

首先创建一个长度为 0，容量为 0 的空切片，然后在尾部插入一些元素，最后输出长度和容量。

```Go
package main

import "fmt"

func main() {
    arr := make([]int, 0, 0)
    arr = append(arr, 1)
    arr = append(arr, 2)
    arr = append(arr, 3)
    arr = append(arr, 4, 5, 6)

    fmt.Println(len(arr), cap(arr))
    fmt.Println(arr)
}
```

![Golang-Golang基础-语法基础-切片-切片-初始化-示例-005.png](Golang-Golang基础-语法基础-切片-切片-初始化-示例-005.png)

<br/>

新 ```slice``` 预留的 ```buffer``` 容量大小是有一定规则的。

* 在 Golang 1.18 版本以前：当原 ```slice``` 容量小于 1024 的时候，新 ```slice``` 容量扩容为原来的 2 倍，当原 ```slice```
  容量超过 1024，新 ```slice``` 容量扩容为原来的 1.25 倍；
* 在 Golang 1.18 版本更新后：当 ```slice``` 容量（oldcap） 小于 256 的时候，新 ```slice```（newcap）容量扩容为原来的 2
  倍，当原 ```slice``` 容量超过 256，新 ```slice``` 容量 newcap = oldcap + (oldcap + 3 * 256) / 4

<br/>

### 插入元素

切片元素的插入也需要借助 ```append``` 函数来实现

<br/>

从头部插入元素

```Go
package main

import "fmt"

func main() {
    arr := []int{1, 2, 3, 4, 5}

    fmt.Println(arr)

    arr = append([]int{-1, 0}, arr...)
    fmt.Println(arr)
}
```

![Golang-Golang基础-语法基础-切片-切片-插入元素-示例-001.png](Golang-Golang基础-语法基础-切片-切片-插入元素-示例-001.png)

从中间下标插入元素

```Go
package main

import "fmt"

func main() {
    arr := []int{1, 2, 3, 4, 5}

    fmt.Println(arr)

    index := 3
    arr = 
    append(arr[:index+1], append([]int{0, 0, 0}, arr[index+1:]...)...)

    fmt.Println(arr)
}
```

![Golang-Golang基础-语法基础-切片-切片-插入元素-示例-002.png](Golang-Golang基础-语法基础-切片-切片-插入元素-示例-002.png)

在尾部插入元素

```Go
package main

import "fmt"

func main() {
    arr := []int{1, 2, 3, 4, 5}

    fmt.Println(arr)

    arr = append(arr, 6, 7, 8, 9, 10)

    fmt.Println(arr)
}
```

![Golang-Golang基础-语法基础-切片-切片-插入元素-示例-003.png](Golang-Golang基础-语法基础-切片-切片-插入元素-示例-003.png)


<br/>

### 删除元素

切片元素的删除也需要借助 ```append``` 函数来实现

<br/>

从头部删除 n 个元素

```Go
package main

import "fmt"

func main() {
    arr := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}

    fmt.Println(arr)

    n := 5
    arr = arr[n:]

    fmt.Println(arr)
}
```

![Golang-Golang基础-语法基础-切片-切片-删除元素-示例-001.png](Golang-Golang基础-语法基础-切片-切片-删除元素-示例-001.png)

从尾部删除 n 个元素

```Go
package main

import "fmt"

func main() {
    arr := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}

    fmt.Println(arr)

    n := 5
    arr = arr[:len(arr)-n]

    fmt.Println(arr)
}
```

![Golang-Golang基础-语法基础-切片-切片-删除元素-示例-002.png](Golang-Golang基础-语法基础-切片-切片-删除元素-示例-002.png)

从中间指定下标 index 位置开始删除 n 个元素

```Go
package main

import "fmt"

func main() {
    arr := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}

    fmt.Println(arr)

    index, n := 3, 4
    arr = append(arr[:index], arr[index+n:]...)

    fmt.Println(arr)
}
```

![Golang-Golang基础-语法基础-切片-切片-删除元素-示例-003.png](Golang-Golang基础-语法基础-切片-切片-删除元素-示例-003.png)

删除所有元素

```Go
package main

import "fmt"

func main() {
    arr := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}

    fmt.Println(arr)

    arr = arr[:0]
    
    fmt.Println(arr)
}
```

![Golang-Golang基础-语法基础-切片-切片-删除元素-示例-004.png](Golang-Golang基础-语法基础-切片-切片-删除元素-示例-004.png)


<br/>

### 拷贝

使用 ```copy``` 函数进行拷贝

```Go
package main

import "fmt"

func main() {
    src := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    dest := make([]int, len(src))
    fmt.Println(src, dest)
    fmt.Println(copy(dest, src))
    fmt.Println(src, dest)
}
```

![Golang-Golang基础-语法基础-切片-切片-拷贝-示例-001.png](Golang-Golang基础-语法基础-切片-切片-拷贝-示例-001.png)

切片在拷贝时需要确保目标切片有足够的长度，否则无法拷贝成功

```Go
package main

import "fmt"

func main() {
    src := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    dest := make([]int, 0, 0)
    fmt.Println(src, dest)
    fmt.Println(copy(dest, src))
    fmt.Println(src, dest)
}
```

![Golang-Golang基础-语法基础-切片-切片-拷贝-示例-002.png](Golang-Golang基础-语法基础-切片-切片-拷贝-示例-002.png)


<br/>

### 遍历

切片的遍历与数组完全一致

<br/>

```for``` 循环

```Go
package main

import "fmt"

func main() {
    arr := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}

    for i := 0; i < len(arr); i++ {
        fmt.Println(arr[i])
    }
}
```

![Golang-Golang基础-语法基础-切片-切片-遍历-示例-001.png](Golang-Golang基础-语法基础-切片-切片-遍历-示例-001.png)

```for range``` 循环

```Go
package main

import "fmt"

func main() {
    arr := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}

    for index, value := range arr {
        fmt.Println(index, value)
    }
}
```

![Golang-Golang基础-语法基础-切片-切片-遍历-示例-002.png](Golang-Golang基础-语法基础-切片-切片-遍历-示例-002.png)


<br/>
<br/>

## 多维切片

```Go
package main

import "fmt"

func main() {
    arr := make([][]int, 5)

    for i := 0; i < len(arr); i++ {
        arr[i] = make([]int, 10)
    }

    for _, value := range arr {
        fmt.Println(value)
    }
}
```

![Golang-Golang基础-语法基础-切片-多维切片-示例.png](Golang-Golang基础-语法基础-切片-多维切片-示例.png)


<br/>
<br/>

## 拓展表达式

在 Go 1.2 版本更新后，切片支持使用拓展表达式。需要满足关系 ```low <= high <= max <= cap```
，使用拓展表达式切割的切片容量为 ```max - low```。

```Go
slice[low:high:max]
```

```max``` 指最大容量，若省略 ```max```，则切割后切片的容量为 ```cap(原切片) - low```

```Go
package main

import "fmt"

func main() {
    arr1 := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    fmt.Printf("len=%d, cap=%d, arr1=%v\n",
              len(arr1), cap(arr1), arr1)

    low, high := 3, 7
    arr2 := arr1[low:high]
    fmt.Printf("len=%d, cap=%d, arr1=%v\n", 
              len(arr2), cap(arr2), arr2)
}
```

![Golang-Golang基础-语法基础-切片-拓展表达式-示例-001.png](Golang-Golang基础-语法基础-切片-拓展表达式-示例-001.png)

但是这样会存在一个问题，```arr1``` 和 ```arr2``` 是共享同一个底层数组，在对 ```arr2```
进行读写时，有可能会影响到 ```arr1``` 的数据

```Go
package main

import "fmt"

func main() {
    arr1 := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    fmt.Printf("len=%d, cap=%d, arr1=%v\n", 
              len(arr1), cap(arr1), arr1)

    low, high := 3, 7
    arr2 := arr1[low:high]
    fmt.Printf("len=%d, cap=%d, arr1=%v\n\n",
              len(arr2), cap(arr2), arr2)

    arr2 = append(arr2, 100)
    fmt.Printf("len=%d, cap=%d, arr1=%v\n", 
              len(arr1), cap(arr1), arr1)
    fmt.Printf("len=%d, cap=%d, arr1=%v\n",
              len(arr2), cap(arr2), arr2)
}
```

![Golang-Golang基础-语法基础-切片-拓展表达式-示例-002.png](Golang-Golang基础-语法基础-切片-拓展表达式-示例-002.png)

拓展表达式可以解决该问题

```Go
package main

import "fmt"

func main() {
    arr1 := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    fmt.Printf("len=%d, cap=%d, arr1=%v\n",
              len(arr1), cap(arr1), arr1)

    low, high := 3, 7
    arr2 := arr1[low:high:7]
    fmt.Printf("len=%d, cap=%d, arr1=%v\n\n", 
               len(arr2), cap(arr2), arr2)

    arr2 = append(arr2, 100)
    fmt.Printf("len=%d, cap=%d, arr1=%v\n", 
              len(arr1), cap(arr1), arr1)
    fmt.Printf("len=%d, cap=%d, arr1=%v\n", 
              len(arr2), cap(arr2), arr2)
}
```

![Golang-Golang基础-语法基础-切片-拓展表达式-示例-003.png](Golang-Golang基础-语法基础-切片-拓展表达式-示例-003.png)


<br/>
<br/>

## clear

Go 1.21 新增了 ```clear``` 内置函数，```clear``` 会将切片内所有的值都置为其零值。

```Go
package main

import "fmt"

func main() {
    arr := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    fmt.Println(arr)

    clear(arr)

    fmt.Println(arr)
}
```

![Golang-Golang基础-语法基础-切片-clear-示例-001.png](Golang-Golang基础-语法基础-切片-clear-示例-001.png)


如果想要清空切片可以这样

```Go
package main

import "fmt"

func main() {
    arr := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    fmt.Printf("len:%d, cap:%d, %v\n", len(arr), cap(arr), arr)

    arr = arr[:0:0]

    fmt.Printf("len:%d, cap:%d, %v\n", len(arr), cap(arr), arr)
}
```

![Golang-Golang基础-语法基础-切片-clear-示例-002.png](Golang-Golang基础-语法基础-切片-clear-示例-002.png)

限制了切割后的容量，这样可以避免覆盖原切片的后续元素
