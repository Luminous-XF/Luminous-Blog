# 条件控制

在 Go 中，条件控制语句共有三种，```if```、```switch```、```select```。

## if else

```if else``` 至多两个判断分支，其中 ```expression``` 必须是一个布尔表达式，即结果要么为真要么为假，必须是一个布尔值。

```Go
if expression {
    
}
```

或

```Go
if expression {
    
} else {

}
```

或

```Go
if 初始化; expression {

}
```

<br/>
<br/>

## else if

```else if``` 语句可以在 ```if else``` 的基础上创建更多的判断分支。

```Go
if expression1 {

} else if expression2 {

} else if expression3 {

} else {

}
```

<br/>
<br/>

## switch

```switch``` 也是一种多分支判断语句

```Go
    switch expr {
    case 1:
        statement1
    case 2:
        statement2
    default:
        statement3
    }
```

示例

```Go
package main

import "fmt"

func main() {
    var num int = 10
    switch num {
    case 1:
        fmt.Println("1")
    case 2:
        fmt.Println("2")
    case 3:
        fmt.Println("3")
    default:
        fmt.Println("no")
    }
}
```

可以在表达式之前编写一些简单语句，如声明新变量

```Go
package main

import "fmt"

func f(num int) int {
    return num * num * num
}

func main() {
    switch num := f(1); { // 等价于 switch num := f(1); true {
    case num < 0:
        fmt.Println("negative")
    case num == 0:
        fmt.Println("zero")
    case num > 0:
        fmt.Println("positive")
    default:
        fmt.Println("other")
    }
}
```

```switch``` 语句也可以没有入口处的表达式

```Go
package main

import "fmt"

func main() {
    num := 1
    switch { // 等价于 switch true {
    case num < 0:
        fmt.Println("negative")
    case num == 0:
        fmt.Println("zero")
    case num > 0:
        fmt.Println("positive")
    default:
        fmt.Println("other")
    }
}
```

可以使用 ```fallthrough``` 关键字来继续执行相邻的下一个分支

```Go
package main

import "fmt"

func main() {
    num := 1
    switch { // 等价于 switch true {
    case num > 0:
        fmt.Println("positive")
        fallthrough
    case num == 0:
        fmt.Println("zero")
    case num < 0:
        fmt.Println("negative")
    default:
        fmt.Println("other")
    }
}
```

![Golang-Golang基础-条件控制-switch-fallthrough示例.png](Golang-Golang基础-语法基础-条件控制-switch-fallthrough示例.png)


<br/>
<br/>

## label

标签语句，给代码打上标签，可以是 ```goto```、```break```、```continue``` 的目标。

```Go
package main

import "fmt"

func main() {
    // 会出现 "Unused label 'A'" 的错误提示
A:
    a := 10
    fmt.Println(a)

    // 会出现 "Unused label 'B'" 的错误提示
B:
    b := 20
    fmt.Println(b)
}
```

单纯地使用标签是没有任何意义的，需要结合其它关键字来进行使用。


<br/>
<br/>

## goto

```goto``` 将控制权传递给在同一函数中对应标签的语句

```Go
package main

import "fmt"

func main() {
    a := 1
    if a == 1 {
        goto A
        // goto 将控制权传递给标签 A 处，这行命令不会被执行到
        fmt.Println("a")
    } else {
        fmt.Println("b")
    }

A:
    fmt.Println("goto here")
}
```

![Golang-Golang基础-条件控制-goto示例.png](Golang-Golang基础-语法基础-条件控制-goto示例.png)

在实际应用中 ```goto``` 用的很少，一方面会降低代码的可读性，另一方面存在性能消耗。