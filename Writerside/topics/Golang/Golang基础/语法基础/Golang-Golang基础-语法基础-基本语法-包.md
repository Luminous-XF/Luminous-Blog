# 包

在 Go 中，程序是通过将包链接在一起构建的，也可以理解为**包**是最基本的调度单位，而不是文件。包其实就是一个文件夹，包内共享所有源文件中的变量、常量、
函数以及其它类型。包的命名风格建议都是小写，并且尽量简短。


<br/>
<br/>


## 导入

例如创建一个 ```example``` 包，包中有以下函数
```Go
package example

import "fmt"

func SayHello() {
    fmt.Println("Hello World!")
}
```

<br/>


在 ```main``` 函数中导入 ```example``` 包，并调用其中 ```SayHello``` 函数
```Go
package main

import "Demo/example"

func main() {
    example.SayHello()
}
```

<br/>

如果导入多个包，可以使用 ```()``` 来表示
```Go
package main

import (
    "fmt"
    "math"
)

func main() {
    fmt.Println(math.Pi)
}
```

<br/>

可以给包起别名
```Go
package main

import (
    e "Demo/example"
)

func main() {
    e.SayHello()
}
```

<br/>

如果说一个包只导入但不调用，可以用 ```_``` 为包起别名，通常这样做是为了调用该包下的 ```init``` 函数
```Go
package main

import (
    "fmt"
    _ "math"
)

func main() {
    fmt.Println(10)
}
```

<br/>

在 Go 中禁止包循环导入（不管是直接的还是间接的），存在循环导入的话将无法通过编译。

例如包A中导入了包B，包B中也导入了包A，这种属于直接循环导入。
![Golang-Golang基础-基础语法-包-直接循环导入.png](Golang-Golang基础-语法基础-基础语法-包-直接循环导入.png){ style="auto" }

<br/>

包A中导入了包C，包C导入了包B，包B又导入了包A，这就是间接的循环导入。
![Golang-Golang基础-基础语法-包-间接循环导入.png](Golang-Golang基础-语法基础-基本语法-包-间接循环导入.png)


<br/>
<br/>


## 导出

在 Go 中，导出和访问控制是通过命名实现的，如果想要对外暴露一个函数或一个变量，只需要将其名称首字母大写即可，例如 ```example``` 包下的 ```SayHello``` 
函数

```Go
package example

import "fmt"

// 函数名首字母大写，可以被包外访问
func SayHello() {
    fmt.Println("Hello World!")
}
```

<br/>

如果不想对外暴露的话，只需要将名称首字母改为小写即可
```Go
package example

import "fmt"

// 函数名首字母小写，外界无法访问
func sayHello() {
    fmt.Println("Hello World!")
}
```

<br/>

对外暴露的函数和变量可以被包外的调用者导入和访问，如果不对外暴露的话，那么仅包内的调用者可以访问，外部将无法导入和访问，该规则适用于整个 Go 语言。


<br/>
<br/>


## 内部包
Go 中约定，一个包内名为 ```internal``` 包为内部包，外部包将无法访问内部包中的任何内容，否则的话编译不通过。

```Shell
│  example.go
│
├─a
│      a.go
│
└─b
    │  b.go
    │
    ├─bb
    │      bb.go
    │
    └─internal
        └─test
                test.go
```

由文件结构可知，```a``` 包中无法访问 ```test``` 包中的内容。