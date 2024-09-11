# 方法

方法与函数的区别在于，方法拥有接收者，而函数没有，且只有自定义类型能够拥有方法。

```Go
type IntSlice []int

func (intSlice IntSlice) Get(index int) int {
    return intSlice[index]
}

func (intSlice IntSlice) Set(index int, value int) {
    intSlice[index] = value
}

func (intSlice IntSlice) Len() int {
    return len(intSlice)
}
```

先声明了一个类型 `IntSlice`，其底层类型为 `[]int`，再声明了三个方法 `Get`、`Set`、`Len`，方法的语法与函数并无太大区别，只是多了一小段 
`intSlice IntSlice`。`intSlice` 就是接收者，`IntSlice` 就是接收者的类型，接收者就类似于其它语言中的 `this` 或 `self`，只不过在 Go 中
需要显示的指明。

```Go
package main

import "fmt"

type IntSlice []int

func (intSlice IntSlice) Get(index int) int {
    return intSlice[index]
}

func (intSlice IntSlice) Set(index int, value int) {
    intSlice[index] = value
}

func (intSlice IntSlice) Len() int {
    return len(intSlice)
}

func main() {
    var intSlice IntSlice

    intSlice = []int{1, 2, 3, 4, 5}
    fmt.Println(intSlice)

    intSlice.Set(0, 2)
    fmt.Println(intSlice)

    fmt.Println(intSlice.Len())
}
```

![Golang-Golang基础-语法基础-方法-示例-001.png](Golang-Golang基础-语法基础-方法-示例-001.png)

方法的使用就类似于调用一个类的成员方法，先声明，再初始化，再调用。


<br/>
<br/>

## 值接收者

接收者也分为两种类型，值接收者和指针接收者

```Go
package main

import "fmt"

type MyInt int

func (myInt MyInt) Set(value int) {
    myInt = MyInt(value)
}

func main() {
    myInt := MyInt(10)
    fmt.Println(myInt)
    myInt.Set(100)
    fmt.Println(myInt)
}
```

![Golang-Golang基础-语法基础-方法-值接收者-示例-001.png](Golang-Golang基础-语法基础-方法-值接收者-示例-001.png)


运行示例中的代码，会发现 `myInt` 的值依旧是 10，并没有被修改为 100。方法在被调用时，会将接收者的值传入方法中，示例中的接收者就是一个值接收者，可以
简单的看成一个形参，而修改一个形参的值，并不会对方法外的值造成任何影响，那么如果通过指针调用又会如何呢？

```Go
package main

import "fmt"

type MyInt int

func (myInt MyInt) Set(value int) {
    myInt = MyInt(value)
}

func main() {
    myInt := MyInt(10)
    fmt.Println(myInt)
    (&myInt).Set(100)
    fmt.Println(myInt)
}
```

![Golang-Golang基础-语法基础-方法-值接收者-示例-002.png](Golang-Golang基础-语法基础-方法-值接收者-示例-002.png)

运行后发现这样不行！这样的代码依旧不能修改内部的值，为了能够匹配上接收者的值，Go 会将其解引用，解释为 `(*(&myInt)).Set(100)`


<br/>
<br/>

## 指针接收者

修改一下，将接收者的类型改为指针，就可以正常修改 `myInt` 的值了

```Go
package main

import "fmt"

type MyInt int

func (myInt *MyInt) Set(value int) {
    *myInt = MyInt(value)
}

func main() {
    myInt := MyInt(10)
    fmt.Println(myInt)
    myInt.Set(100)
    fmt.Println(myInt)
}
```

![Golang-Golang基础-语法基础-方法-指针接收者-示例-001.png](Golang-Golang基础-语法基础-方法-指针接收者-示例-001.png)

现在的接收者就是一个指针接收者，虽然 `myInt` 是一个值类型，在通过值类型调用指针接收者的方法时，Go 回将其解释为 `(&myInt).Set(100)`。所以方法的
接收者为指针时，不管调用者是不是指针，都可以修改内部的值。

<br/>

函数的参数传递过程中，是值拷贝的，如果传递的是一个整型，那就拷贝这个整型，如果是一个切片，那就拷贝这个切片，但如果是一个指针，就只需要拷贝这个指针，显然
传递一个指针比起传递一个切片所消耗的资源更小，接收者也一样，值接收者和指针接收者都是同样的道理。在大多数情况下，都推荐使用指针接收者，不过两者并不应该
混合使用，要么都用，要么就不用，看下面这个示例。

```Go
package main

import "fmt"

type Animal interface {
    Run()
}

type Dog struct{}

func (dog *Dog) Run() {
    fmt.Println("Run")
}

func main() {
    var animal Animal
    animal = Dog{}
    animal.Run()
}
```

这段代码将无法通过编译，编译器会出现如下错误
```text
Cannot use Dog{} (type Dog) as the type Animal
Type does not implement Animal as the Run method has a pointer receiver
```

![Golang-Golang基础-语法基础-方法-指针接收者-示例-002.png](Golang-Golang基础-语法基础-方法-指针接收者-示例-002.png)

错误的意思是，无法使用 `Dog{}` 初始化 `Animal` 类型的变量，因为 `Dog` 没有实现 `Animal`，解决办法有两种，一个是将指针接收者改为值接收者，二是
将 `Dog{}` 改为 `&Dog{}`。

<br/>

改为值接收者，可以正常运行

```Go
package main

import "fmt"

type Animal interface {
    Run()
}

type Dog struct{}

func (dog Dog) Run() { // 这里改为值接收者
    fmt.Println("Run")
}

func main() {
    var animal Animal
    animal = Dog{}
    animal.Run()
}
```

![Golang-Golang基础-语法基础-方法-指针接收者-示例-003.png](Golang-Golang基础-语法基础-方法-指针接收者-示例-003.png)

将 `Dog{}` 改为 `&Dog{}`，可以正常运行

```Go
package main

import "fmt"

type Animal interface {
    Run()
}

type Dog struct{}

func (dog *Dog) Run() { // 这里改为值接收者
    fmt.Println("Run")
}

func main() {
    var animal Animal
    animal = &Dog{}
    animal.Run()
}
```

![Golang-Golang基础-语法基础-方法-指针接收者-示例-004.png](Golang-Golang基础-语法基础-方法-指针接收者-示例-004.png)

在原来的代码中，`Run` 方法接收者是 `*Dog`，自然而然实现 `Animal` 接口就是 `Dog` 指针，而不是 `Dog` 结构体，这是两个不同的类型，所以编译器就会
认为 `Dog{}` 并不是 `Animal` 的实现，因为无法赋值给变量 `animal`，所以第二种解决办法就是赋值 `Dog` 指针给变量 `animal`。不过在使用值接收者时，
`Dog` 指针依然可以正常赋值给 `animal`，这是因为 Go 会在适当情况下对指针进行解引用，因为通过指针可以找到 `Dog` 结构体，但是反过来的话，无法通过
`Dog` 结构体找到 `Dog` 指针。如果单纯的在结构体中混用值接收者和指针接收者的话无伤大雅，但是和接口一起使用后，就会出现错误，倒不如无论何时要么都用
值接收者，要么就都用指针接收者，形成一个良好地规范，也可以减少后续维护的负担。

<br/>

还有一种情况，就是当值接收者是可以寻址的时候，Go 会自动地插入指针运算符来进行调用，例如切片是可寻址，依旧可以通过值接收者来修改其内部值。如下面这个
示例。

```Go
package main

import "fmt"

type Slice []int

func (slice Slice) Set(index int, value int) {
    slice[index] = value
}

func main() {
    slice := make(Slice, 1)
    fmt.Println(slice)
    slice.Set(0, 100)
    fmt.Println(slice)
}
```

![Golang-Golang基础-语法基础-方法-指针接收者-示例-005.png](Golang-Golang基础-语法基础-方法-指针接收者-示例-005.png)

但这样会引发另一个问题，如果对其添加元素的话，情况就不同了。

```Go
package main

import "fmt"

type Slice []int

func (slice Slice) Set(index int, value int) {
    slice[index] = value
}

func (slice Slice) Append(value int) {
    slice = append(slice, value)
}

func main() {
    slice := make(Slice, 1, 2)
    fmt.Println(slice)
    slice.Set(0, 100)
    fmt.Println(slice)
    slice.Append(200)
    fmt.Println(slice)
}
```

![Golang-Golang基础-语法基础-方法-指针接收者-示例-006.png](Golang-Golang基础-语法基础-方法-指针接收者-示例-006.png)

它的输出还是和之前一样，`append` 函数是有返回值的，向切片添加完元素之后必须覆盖原切片，尤其是在扩容后，在方法中对值接收者修改并不会产生任何影响，这
也就导致了示例中的结果，改成指针接收者就正常了。

```Go
package main

import "fmt"

type Slice []int

func (slice *Slice) Set(index int, value int) {
    (*slice)[index] = value
}

func (slice *Slice) Append(value int) {
    *slice = append(*slice, value)
}

func main() {
    slice := make(Slice, 1, 2)
    fmt.Println(slice)
    slice.Set(0, 100)
    fmt.Println(slice)
    slice.Append(200)
    fmt.Println(slice)
}
```

![Golang-Golang基础-语法基础-方法-指针接收者-示例-007.png](Golang-Golang基础-语法基础-方法-指针接收者-示例-007.png)


