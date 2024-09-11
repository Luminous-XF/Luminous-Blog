# 结构体

结构体可以存储一组不同类型的数据，是一种复合类型。Go 抛弃了类与继承，同时也抛弃了构造方法，刻意弱化了面向对象的功能，Go
并非是一个传统的 OOP 的语言，
但是 Go 依旧有着 OOP 的影子，通过结构体和方法也可以模拟出一个类。


<br/>

## 声明

结构体的声明非常简单

```Go
type Person struct {
    Name string
    Age int
}
```

结构体本身以及其内部的字段都遵守大小写命名的暴露方式。对于一些类型相同地相邻字段，可以不需要重复声明类型。

```Go
type Rectangle struct {
    width, height float64
    Area          float64
    color         string
}
```

<note>
在声明结构体字段时，字段名不能与方法名重复
</note>

<br/>
<br/>

## 实例化

Go 不存在构造方法，大多数情况下采用如下的方式来实例化结构体，初始化的时候像 `map` 一样指定字段名再初始化字段值。

```Go
package main

import "fmt"

type Programmer struct {
    Name     string
    Age      int
    Job      string
    Language []string
}

func main() {
    programmer := Programmer{
        Name:     "Jack",
        Age:      42,
        Job:      "Coder",
        Language: []string{"Golang", "Python"},
    }

    fmt.Println(programmer)
}
```

不过也可以省略字段名称，当省略字段名称时，就必须初始化所有字段，通常不建议使用这种方式，因为可读性非常糟糕。

```Go
package main

import "fmt"

type Programmer struct {
    Name     string
    Age      int
    Job      string
    Language []string
}

func main() {
    programmer := Programmer{
        "Jack",
        42,
        "Coder",
        []string{"Golang", "Python"},
    }

    fmt.Println(programmer)
}
```

如果实例化过程比较复杂，也可以编写一个函数来实例化结构体，就如下面示例一样，可以把其理解为一个构造函数。

```Go
package main

type Person struct {
    Name    string
    Age     int
    Address string
    Salary  float64
}

func NewPerson(name string, age int, address string, salary float64) *Person {
    return &Person{Name: name, Age: age, Address: address, Salary: salary}
}

func main() {

}
```

不过 Go
并不支持函数与方法重载，所以无法为同一个函数或方法定义不同的参数。如果想要以多种方式实例化结构体，要么创建多个构造函数，要么建议使用 `options`
模式。


<br/>
<br/>

## 选项模式

选项模式是 Go 语言中一种很常见的设计模式，可以更为灵活的实例化结构体，拓展性强，并不需要改变构造函数的函数签名。

```Go
type Person struct {
    Name     string
    Age      int
    Address  string
    Salary   float64
    Birthday string
}
```

声明一个 `PersonOptions` 类型，它接收一个 `*Persion` 类型的参数，它必须是指针，因为我们要在闭包中对 `Person` 赋值。

```Go
type PersonOptions func(p *Person)
```

接下来创建选项函数，它们一般是 `with` 开头，它们的返回值就是一个闭包函数。

```Go
func WithName(name string) PersonOptions {
    return func(p *Person) {
        p.Name = name
    }
}

func WithAge(age int) PersonOptions {
    return func(p *Person) {
        p.Age = age
    }
}

func WithAddress(address string) PersonOptions {
    return func(p *Person) {
        p.Address = address
    }
}

func WithSalary(salary float64) PersonOptions {
    return func(p *Person) {
        p.Salary = salary
    }
}
```

实际声明的构造函数签名如下，它就受一个可变长 `PersonOptions` 类型的参数。

```Go
func NewPerson(options ...PersonOptions) *Person {
    p := &Person{}

    // 优先应用 options
    for _, option := range options {
        option(p)
    }

    // 默认值处理
    if p.Age < 0 {
        p.Age = 0
    }

    return p
}
```

这样一来对于不同实例化的需求只需要一个构造函数即可完成，只需要传入不同的 `Options` 函数即可。

```Go
func main() {
    p1 := NewPerson(
        WithName("Alice"),
        WithAge(18),
        WithAddress("World-001"),
        WithSalary(1.5),
    )

    fmt.Printf("%+v\n", p1)

    p2 := NewPerson(
        WithName("Bob"),
        WithAge(18),
    )

    fmt.Printf("%+v\n", p2)
}
```

完整示例代码

```Go
package main

import "fmt"

type Person struct {
    Name     string
    Age      int
    Address  string
    Salary   float64
    Birthday string
}

type PersonOptions func(p *Person)

func WithName(name string) PersonOptions {
    return func(p *Person) {
        p.Name = name
    }
}

func WithAge(age int) PersonOptions {
    return func(p *Person) {
        p.Age = age
    }
}

func WithAddress(address string) PersonOptions {
    return func(p *Person) {
        p.Address = address
    }
}

func WithSalary(salary float64) PersonOptions {
    return func(p *Person) {
        p.Salary = salary
    }
}

func NewPerson(options ...PersonOptions) *Person {
    p := &Person{}

    // 优先应用 options
    for _, option := range options {
        option(p)
    }

    // 默认值处理
    if p.Age < 0 {
        p.Age = 0
    }

    return p
}

func main() {
    p1 := NewPerson(
        WithName("Alice"),
        WithAge(18),
        WithAddress("World-001"),
        WithSalary(1.5),
    )

    fmt.Printf("%+v\n", p1)

    p2 := NewPerson(
        WithName("Bob"),
        WithAge(18),
    )

    fmt.Printf("%+v\n", p2)
}
```

函数式选项模式在很多开源项目中都能看见，`gRPC Server` 的实例化方式也是采用了该设计模式。函数式选项模式只适合于复杂的实例化，如果参数只有简单几个，
建议还是使用普通的构造函数来解决。

<br/>
<br/>

## 组合

在 Go 中，结构体之间的关系是通过组合来表示的，可以是显式组合，也可以是匿名组合，后者使用起来更类似于继承，但本质上没有任何变化。

<br/>

显示组合的方式

```Go
type Person struct {
    Name string
    Age  int
}

type Student struct {
    p      Person
    school string
}

type Employee struct {
    p   Person
    job string
}
```

在使用时需要显式的指定字段 `p`。

```Go
package main

import "fmt"

type Person struct {
    Name string
    Age  int
}

type Student struct {
    p      Person
    school string
}

type Employee struct {
    p   Person
    job string
}

func main() {
    student := Student{
        p:      Person{Name: "Bob", Age: 20},
        school: "MIT",
    }

    fmt.Println(student.p.Name)
    fmt.Println(student.p.Age)
    fmt.Println(student.school)
}
```

匿名组合方式

<br/>

匿名组合可以不用显式的指定字段

```Go
type Person struct {
    Name string
    Age  int
}

type Student struct {
    Person
    school string
}

type Employee struct {
    Person
    job string
}
```

匿名字段的名称默认为类型名，调用者可以之间访问该类型的字段和方法，但除了更加方便以外与第一种方式没有任何区别

```Go
package main

import "fmt"

type Person struct {
    Name string
    Age  int
}

type Student struct {
    Person
    school string
}

type Employee struct {
    Person
    job string
}

func main() {
    student := Student{
        Person: Person{Name: "Bob", Age: 20},
        school: "MIT",
    }

    fmt.Println(student.Name)
    fmt.Println(student.Age)
    fmt.Println(student.school)
}
```

<br/>
<br/>

## 指针

结构体标签式一种元编程的形式，结合反射可以做出很多奇妙的功能。

<br/>

使用格式

```Go
`key1:"value1`" key2:"value2"`
```

标签是一种键值对的形式，使用空格进行分隔。结构体标签的容错性很低，如果没能按照正确的格式书写结构体，那么将会导致无法正常读取，但是在编译时却不会有
任何的报错。

```Go
type Programmer struct {
    Name     string   `json:"name"`
    Age      int      `yaml:"age"`
    Job      string   `toml:"job"`
    Language []string `properties:"language"`
}
```

结构体标签最广泛的应用就是在各种序列化格式中的别名定义，标签的使用需要结合反射才能完整的发挥出其功能。


<br/>
<br/>

## 内存对齐

Go 结构体字段的内存分布遵循内存对齐的规则，这么做可以减少 CPU 访问内存的次数，相应的占用的内存要多一些，属于空间换时间的一种手段。

```Go
type Num struct {
    A int64
    B int32
    C int16
    D int8
    E int32
}
```

已知这些类型的占用字节数

* `int64` 占 8 个字节
* `int32` 占 4 个字节
* `int16` 占 2 个字节
* `int8`  占 1 个字节

整个结构体的内存占用看起来是 `8 + 4 + 2 + 1 + 4 = 19` 个字节，但实际上不是的，根据内存对齐规则，结构体的内存占用长度至少是最大字段的整数倍，
不足的则会补齐。该结构体中最大的是 `int64` 占用 8 个字节，那么内存分布如下图所示：

![Golang-Golang基础-语法基础-结构体-内存对齐-示例-001.png](Golang-Golang基础-语法基础-结构体-内存对齐-示例-001.png)

所以实际上是占用 24 个字节，其中有 5 个字节是无用的。

<br/>

```Go
type Num struct {
    A int8
    B int64
    C int8
}
```

了解了上面的规则后，可以算出该结构体也占 24 个字节，尽管它只有 3 个字段，其中足足浪费了 14 个字节。

![Golang-Golang基础-语法基础-结构体-内存对齐-示例-002.png](Golang-Golang基础-语法基础-结构体-内存对齐-示例-002.png)

但是如果调整字段，改为如下顺序

```Go
type Num struct {
    A int8
    B int8
    C int64
}
```

如此一来，占用的内存就变成了 16 字节，浪费了 6 个字节，减少了 8 个字节的内存浪费。

![Golang-Golang基础-语法基础-结构体-内存对齐-示例-003.png](Golang-Golang基础-语法基础-结构体-内存对齐-示例-003.png)

从理论上说，让结构体中的字段按照合理的顺序分布，可以减少其内存占用。不过实际编码过程中，并没有必要的理由去这样做，它不一定能在减少内存占用这方面带来
实质性的提升，但一定会提高开发人员的血压和心智负担，尤其是在业务中一些结构体的字段数可能多大几十个或者数百个，所以仅做了解即可。


<note>
如果真的想通过此种方式来节省内存，可以了解一下这两个库
<list>
    <li><a href="https://github.com/dkorunic/betteralign">BetterAlign</a></li>
    <li><a href="https://github.com/dominikh/go-tools">go-tools</a></li>
</list>
</note>

<br/>
<br/>

## 空结构体

空结构体没有字段，不占用内存空间，可以通过 `unsafe.SizeOf` 函数来计算占用字节大小

```Go
package main

import (
    "fmt"
    "unsafe"
)

type Empty struct{}

func main() {
    emptyStruct := Empty{}
    fmt.Println(unsafe.Sizeof(emptyStruct))
}
```

![Golang-Golang基础-语法基础-结构体-空结构体-示例-001.png](Golang-Golang基础-语法基础-结构体-空结构体-示例-001.png)

空结构体的使用场景有很多，如之前提到过的，作为 `map` 的值类型，从而可以将 `map` 作为 `set` 来进行使用，又或者是作为通道的值类型，表示仅做通知类型
的通道。
