# 函数

在 Go 中，函数是一等公民，函数是 Go 最基础的组成部分，也是 Go 的核心。

<br/>
<br/>

## 声明

```Go
func 函数名([参数列表]) [返回值] {
    函数体
}
```

声明函数有两种方法，一种是通过 `func` 关键字直接声明，另一种是通过 `var` 关键字来声明

```Go
package main

import "fmt"

func sum(a int, b int) int {
    return a + b
}

func main() {
    fmt.Println(sum(1, 2))
}
```

```Go
package main

import "fmt"

func main() {
    var sum = func(a int, b int) int {
        return a + b
    }

    fmt.Println(sum(1, 2))
}
```

函数签名由函数名称、参数列表、返回值组成，下面是一个完整的示例，函数名为 `sum`，有两个 `int` 类型的参数 `a`、`b`
，返回值类型为 `int`。

```Go
func Sum(a int, b int) int {
    return a + b
}
```

⚠️ 需要注意，在 Go 函数是不支持重载的！

<br/>

Go 的理念是：如果签名不一样那就是两个完全不同的函数，那么就不应该取一样的名字，函数重载会让代码变得混淆和难以理解。在 Go 中可以仅通过函数名就能知道
这个函数是用来做什么的，而不需要去找它到底是哪一个重载。


<br/>
<br/>

## 参数

Go 中的参数名可以不带名称，一般这种是在接口或函数类型声明时才会用到，不过为了可读性还是建议尽量给参数加上名称。

```Go
type ExWriter func(io.Writer) error

type Writer interface {
    ExWriter([]byte) (int, error)
}
```

对于类型相同的参数而言，可以只需要声明一次类型，不过条件是它们必须相邻

```Go
func Log(format string, a1, aw any) {
    ...
}
```

变长参数可以接收 0 个或多个值，必须声明在参数列表的末尾，最典型的例子就是 `fmt.Printf` 函数

```Go
func Printf(format string, a ...any) (n int, err error) {
	return Fprintf(os.Stdout, format, a...)
}
```

Go 中的函数的参数是传值传递，即在传递参数时会拷贝实参的值。不过不用担心在传递 `slice` 或 `map` 时会复制大量的内存，因为这两个数据结构本质上都是
指针。


<br/>
<br/>

## 返回值

下面示例中 `Sum` 函数返回一个 `int` 类型的值。

```Go
func Sum(a, b int) int {
    return a + b
}
```

当函数没有返回值时，不需要 `void`，不带返回值即可。

```Go
func ErrPrintf(format string, a ...any) {
    _, _ = fmt.Fprintf(os.Stderr, format, a...)
}
```

Go 允许函数有多个返回值，此时就需要用括号将返回值围起来。

```Go
func Dev(a, b float64) (float64, error) {
    if b == 0 {
        return math.NaN(), errors.New("0 不能做除数")
    }

    return a / b, nil
}
```

Go 也支持具名返回值，不能与参数名重复，使用具名返回值时， `return` 关键字可以不需要指定返回那些值。

```Go
func Sum(a, b int) (ans int) {
    ans = a + b
    
    return
}
```

和参数一样，当有多个同类型的具名返回值时，可以省略掉重复的类型声明

```Go
func SumAndMul(a, b int) (c, d int) {
    c = a + b
    d = a * b
    
    return
}
```

不管具名返回值如何声明，永远都是以 `return` 关键字后的值为最高优先级。

```Go
func SumAndMul(a, b int) (c, d int) {
    c = a + b
    d = a * b
    
    // c、d 将不会被返回
    return a + b, a * b
}
```

<br/>
<br/>

## 匿名函数

匿名函数就是没有签名的函数，如示例中的函数 `func(a, b int) int` 它没有名称，所有只能在它的函数体后紧跟括号来进行调用。

```Go
package main

import "fmt"

func main() {
    var sum = func(a, b int) int {
        return a + b
    }(1, 2)

    fmt.Println(sum)
}
```

在调用一个函数时，当它的参数时一个函数类型时，这时名称不再重要，就可以直接传递一个匿名函数。

```Go
package main

import (
    "fmt"
    "slices"
)

type Person struct {
    Name   string
    Age    int
    Salary float64
}

func main() {
    people := []Person{
        {Name: "A", Age: 18, Salary: 3000},
        {Name: "B", Age: 24, Salary: 1000},
        {Name: "C", Age: 20, Salary: 2000},
    }

    slices.SortFunc(people, func(p1, p2 Person) int {
        if p1.Name > p2.Name {
            return 1
        } else if p1.Name < p2.Name {
            return -1
        }
        return 0
    })

    fmt.Println(people)
}
```

这是一个自定义排序规则的例子，`slices.SortFunc` 接收两个参数，一个是切片，另一个是比较函数，在不考虑复用的情况下，可以直接传递匿名函数。


<br/>
<br/>

## 闭包

闭包（Closure）这一概念，在一些语言中又被称为 Lambda 表达式，与匿名函数一起使用。

```Go
package main

import "fmt"

func Exp(n int) func() int {
    e := 1
    return func() int {
        temp := e
        e *= n
        return temp
    }
}

func main() {
    grow := Exp(2)

    for i := range 10 {
        fmt.Printf("2 ^ %d\t=\t%d\n", i, grow())
    }
}
```

![Golang-Golang基础-语法基础-函数-闭包-示例-001.png](Golang-Golang基础-语法基础-函数-闭包-示例-001.png)

`Exp` 函数的返回值是一个函数，这里将其称为 `grow` 函数，每将它调用一次，变量 `e` 就会以指数级增长一次。`grow` 函数引用了 `Exp` 函数的两个变量：
`e` 和 `n`，它们诞生在 `Exp` 函数的作用域内，在正常情况下随着 `Exp` 函数的调用结束，这些变量的内存会随着出栈而被回收。但是由于 `grow` 函数引用
了它们，所以它们无法被回收，而是逃逸到了堆上，即使 `Exp` 函数的生命周期已经结束了，但变量 `e` 和 `n` 的生命周期并没有结束，在 `grow` 函数内还能
直接修改这两个变量，`grow` 函数就是一个闭包函数。

<br/>

利用闭包，可以非常简单的实现一个求斐波那契数列的函数

```Go
package main

import "fmt"

func Fib(n int) func() (int, bool) {
    a, b, c := 1, 1, 2

    i := 0
    return func() (int, bool) {
        if i >= n {
            return 0, false
        } else if i < 2 {
            f := i
            i++
            return f, true
        }

        a, b = b, c
        c = a + b
        i++
        return a, true
    }
}

func main() {
    fib := Fib(10)
    for n, next := fib(); next; n, next = fib() {
        fmt.Println(n)
    }
}
```

![Golang-Golang基础-语法基础-函数-闭包-示例-002.png](Golang-Golang基础-语法基础-函数-闭包-示例-002.png)


<br/>
<br/>

## 延迟调用

`defer` 关键字可以使得一个函数延迟一段时间调用，在函数返回之前这些 `defer` 描述的函数最后都会被逐个执行

```Go
package main

import "fmt"

func Do() {
    defer func() {
        fmt.Println("1")
    }()

    fmt.Println("2")
}

func main() {
    Do()
}
```

![Golang-Golang基础-语法基础-函数-延迟调用-示例-001.png](Golang-Golang基础-语法基础-函数-延迟调用-示例-001.png)

因为 `defer` 是在函数返回前执行的，也可以在 defer 中修改函数的返回值

```Go
package main

import "fmt"

func Sum(a, b int) (sum int) {
    defer func() {
        sum -= 10
    }()

    sum = a + b

    return
}

func main() {
    sum := Sum(2, 3)
    fmt.Println(sum)
}
```

![Golang-Golang基础-语法基础-函数-延迟调用-示例-002.png](Golang-Golang基础-语法基础-函数-延迟调用-示例-002.png)

当又多个 `defer` 描述的函数时，就会像栈一样先进后出的顺序执行。

```Go
package main

import "fmt"

func Do() {
    defer fmt.Println(1)
    fmt.Println(2)
    defer fmt.Println(3)
    fmt.Println(4)
    defer fmt.Println(5)
    fmt.Println(6)
}

func main() {
    fmt.Println(0)
    Do()
}
```

延迟调用通常用于释放文件资源，关闭网络连接等操作，还有一个用法是捕获 `panic`

<br/>

### 循环

虽然没有明令禁止，但一般不建议在 `for` 循环中使用 `defer`

```Go
package main

import "fmt"

func main() {
    for i := range 5 {
        defer fmt.Println(i)
    }
}
```

![Golang-Golang基础-语法基础-函数-延迟调用-循环-示例-001.png](Golang-Golang基础-语法基础-函数-延迟调用-循环-示例-001.png)

这段代码结果是正确的，但过程也许不对，在 Go 中，每创建一个 `defer`，就需要在当前协程申请一片内存空间。假设在上面示例中不是简单固定次数的 `for` 
循环，而是一个较为复杂的数据处理流程，当外部请求数突然激增时，那么在短时间内就会创建大量的 `defer`，在循环次数很大或次数不确定时，就可能会导致内存
占用突然暴涨，这种情况一般称为内存泄露。

<br/>

### 参数预计算

对于延迟调用有一些反直觉的细节。

```Go
package main

import "fmt"

func Do() int {
    fmt.Println("2")
    return 1
}

func main() {
    defer fmt.Println(Do())
    fmt.Println("3")
}
```

这段代码结果我们能认为是
```text
3
2
1
```

但结果却是

![Golang-Golang基础-语法基础-函数-延迟调用-参数预计算-示例-001.png](Golang-Golang基础-语法基础-函数-延迟调用-参数预计算-示例-001.png)

按照使用者的初衷来看，`fmt.Println(Do())` 这部分应该是希望它们在函数体执行结束后再执行，`fmt.Println()` 确实是最后执行的，但 `Do` 是在意料
之外的

```Go
package main

import "fmt"

func sum(a, b int) int {
    return a + b
}

func main() {
    var a, b int
    a = 1
    b = 2

    defer fmt.Println(sum(a, b))
    
    a = 3
    b = 4
}
```

对于这个例子，输出的结果是 3 而不是 7

![Golang-Golang基础-语法基础-函数-延迟调用-参数预计算-示例-002.png](Golang-Golang基础-语法基础-函数-延迟调用-示例-002.png)

如果换成闭包，结果又会不一样

```Go
package main

import "fmt"

func sum(a, b int) int {
    return a + b
}

func main() {
    var a, b int
    a = 1
    b = 2

    f := func() {
        fmt.Println(sum(a, b))
    }

    a = 3
    b = 4

    f()
}
```

闭包输出的结果是 7

![Golang-Golang基础-语法基础-函数-延迟调用-参数预计算-示例-003.png](Golang-Golang基础-语法基础-函数-延迟调用-参数预计算-示例-003.png)

如果把延迟调用和闭包结合起来呢

```Go
package main

import "fmt"

func sum(a, b int) int {
    return a + b
}

func main() {
    var a, b int
    a = 1
    b = 2

    defer func() {
        fmt.Println(sum(a, b))
    }()

    a = 3
    b = 4
}
```

输出的结果是 7

![Golang-Golang基础-语法基础-函数-延迟调用-参数预计算-示例-004.png](Golang-Golang基础-语法基础-函数-延迟调用-参数预计算-示例-004.png)


再改一下，没有闭包了

```Go
package main

import "fmt"

func sum(a, b int) int {
    return a + b
}

func main() {
    var a, b int
    a = 1
    b = 2

    defer func(num int) {
        fmt.Println(num)
    }(sum(a, b))

    a = 3
    b = 4
}
```

输出结果为 3

![Golang-Golang基础-语法基础-函数-延迟调用-参数预计算-示例-005.png](Golang-Golang基础-语法基础-函数-延迟调用-参数预计算-示例-005.png)

通过观察上面几个示例可以发现下面这段代码

```Go
defer fmt.Println(sum(a, b))
```

等价于

```Go
defer fmt.Println(3)
```

Go 不会等到最后才去调用 `sum` 函数，`sum` 函数早在延迟调用被执行以前就被调用了，并作为参数传递了 `fmt.Println()`。总结就是，对于 `derfer` 
直接作用的函数而言，它的参数是会被预计算的，这也就导致了第一个示例的奇怪现象，对于这种情况，尤其是在延迟调用中将函数返回值作为参数的情况尤其需要注意。