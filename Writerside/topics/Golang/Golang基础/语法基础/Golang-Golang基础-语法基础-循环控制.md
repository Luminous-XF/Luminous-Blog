# 循环控制

在 Go 中，有仅只有一种循环语句：```for```，Go 抛弃了 ```while``` 语句，```for``` 语句可以被当作 ```while``` 来使用。

<br/>

## for

```for``` 语法格式

```Go
for init statment; expression; post statment {
    execute statment...
}
```

```Go
package main

import "fmt"

func main() {
    // 输出 [0, 9) 区间的数字
    for i := 0; i < 10; i++ {
        fmt.Println(i)
    }
}
```

![Golang-Golang基础-语法基础-循环控制-for示例-001.png](Golang-Golang基础-语法基础-循环控制-for示例-001.png)

当只保留循环条件时，就变成了 ```while```

```Go
for expression {
    execute statment
}
```

```Go
package main

import "fmt"

func main() {
    // 输出 [0, 9) 区间的数字
    var i int = 0
    for i < 10 {
        fmt.Println(i)
        i++
    }
}
```

![Golang-Golang基础-语法基础-循环控制-for示例-002.png](Golang-Golang基础-语法基础-循环控制-for示例-002.png)

这是一个死循环

```Go
for {
    execute statment
}
```

<br/>
<br/>

## for range

```for range``` 可以更加方便的遍历一些可迭代的数据结构，如数组、切片、字符串、映射表、通道。

```Go
for index, value := range iterable {
    // body
}
```

```index``` 为可迭代数据结构的索引，```value``` 则是对应索引下的值，例如遍历一个字符串。

```Go
package main

import "fmt"

func main() {
    var str string = "Hello World"
    for index, value := range str {
        fmt.Println(index, value)
    }
}
```

![Golang-Golang基础-语法基础-循环控制-for_range示例-001.png](Golang-Golang基础-语法基础-循环控制-for_range示例-001.png)

```for range``` 也可以迭代一个整型值、字面量、常量或变量

迭代一个字面量

```Go
package main

import "fmt"

func main() {
    for i := range 10 {
        fmt.Println(i)
    }
}
```

![Golang-Golang基础-语法基础-循环控制-for_range-示例-002.png](Golang-Golang基础-语法基础-循环控制-for_range-示例-002.png)

迭代一个常量

```Go
package main

import "fmt"

func main() {
    const n = 10
    for i := range n {
        fmt.Println(i)
    }
}
```

![Golang-Golang基础-语法基础-循环控制-for_range-示例-003.png](Golang-Golang基础-语法基础-循环控制-for_range-示例-003.png)

迭代一个变量
```Go
package main

import "fmt"

func main() {
    var n int = 10
    for i := range n {
        fmt.Println(i)
    }
}
```

![Golang-Golang基础-语法基础-循环控制-for_range-示例-004.png](Golang-Golang基础-语法基础-循环控制-for_range-示例-004.png)


对于不同的数据结构，```for range``` 的实现都有所不同。具体可以参考 [](https://go.dev/ref/spec#For_statements)


<br/>
<br/>

## break

```break``` 关键字会终止当前循环

```Go
package main

import "fmt"

func main() {
    for i := 1; i <= 10; i++ {
        for j := 1; ; j++ {
            // 当 j > i 时终止当前循环
            if j > i {
                break
            }
            fmt.Printf("%d ", j)
        }
        fmt.Println()
    }
}
```

![Golang-Golang基础-语法基础-循环控制-break-示例-001.png](Golang-Golang基础-语法基础-循环控制-break-示例-001.png)

使用标签来终止外层循环

```Go
package main

import "fmt"

func main() {

Outer:
    for i := range 3 {
        for j := range 3 {
            if i > j {
                break Outer
            }
            fmt.Println(i, j)
        }
    }
}
```

![Golang-Golang基础-语法基础-循环控制-break-示例-002.png](Golang-Golang基础-语法基础-循环控制-break-示例-002.png)

<br/>
<br/>

```continue``` 关键字会结束当前循环的本次迭代，直接进入下一次迭代

```Go
package main

import "fmt"

func main() {
    for i := 1; i <= 10; i++ {
        // 若 i 为偶数则跳过
        if i%2 == 0 {
            continue
        }

        // 输出奇数
        fmt.Println(i)
    }
}
```

![Golang-Golang基础-语法基础-循环控制-continue-示例-001.png](Golang-Golang基础-语法基础-循环控制-continue-示例-001.png)


使用标签来结束当前外层循环，使外层循环直接进入下一次迭代

```Go
package main

import "fmt"

func main() {

Outer:
    for i := range 3 {
        for j := range 3 {
            if i == j {
                continue Outer
            }
            fmt.Println(i, j)
        }
    }
}
```

![Golang-Golang基础-语法基础-循环控制-continue-示例-002.png](Golang-Golang基础-语法基础-循环控制-continue-示例-002.png)