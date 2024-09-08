# 字符串

在 Go 中，字符串本质上是一个可不变的只读的字节数组，也是一片连续的内存空间。


<br/>

## 字面量

### 普遍字符串

普通字符串由 ```""``` 双引号表示，支持转义，不支持多行书写

```Go
package main

import "fmt"

func main() {
    var str string = "Hello\nWorld\n!\n"

    fmt.Printf("%s", str)
}
```

![Golang-Golang基础-语法基础-字符串-字面量-普通字符串-示例-001.png](Golang-Golang基础-语法基础-字符串-字面量-普通字符串-示例-001.png)

### 原生字符串

原生字符串由反引号表示，不支持转义，支持多行书写，原生字符串里面所有的字符都会原封不动的输出，包括换行和缩进。

```Go
package main

import "fmt"

func main() {
    var str string = `Hello\nWorld\n!\n`

    fmt.Printf("%s", str)
}
```

![Golang-Golang基础-语法基础-字符串-字面量-原生字符串-示例-001.png](Golang-Golang基础-语法基础-字符串-字面量-原生字符串-示例-001.png)

<br/>
<br/>

## 访问

因为字符串本质是字节数组，所以字符串的访问形式跟数组切片完全一致

```Go
package main

import "fmt"

func main() {
    var str string = "Hello World"

    for i := 0; i < len(str); i++ {
        fmt.Println(str[i])
    }
}
```

![Golang-Golang基础-语法基础-字符串-访问-示例-001.png](Golang-Golang基础-语法基础-字符串-访问-示例-001.png)

切割字符串

```Go
package main

import "fmt"

func main() {
    var str string = "Hello World"

    fmt.Println(str[0:5])
}
```

![Golang-Golang基础-语法基础-字符串-访问-示例-002.png](Golang-Golang基础-语法基础-字符串-访问-示例-002.png)



⚠️ 尝试修改字符串元素

```Go
package main

import "fmt"

func main() {
    var str string = "Hello World"

    // 不可修改，无法通过编译
    str[0] = 'h'

    fmt.Println(str)
}
```

![Golang-Golang基础-语法基础-字符串-访问-示例-003.png](Golang-Golang基础-语法基础-字符串-访问-示例-003.png)


虽然无法修改字符串，但是可以覆盖

```Go
package main

import "fmt"

func main() {
    var str string = "Hello World"

    str = "你好世界"

    fmt.Println(str)
}
```

![Golang-Golang基础-语法基础-字符串-访问-示例-004.png](Golang-Golang基础-语法基础-字符串-访问-示例-004.png)


<br/>
<br/>

## 转换

字符串可以转换为字节切片，而字节切片或字节数组也可以转换为字符串

```Go
package main

import "fmt"

func main() {
    var str1 string = "Hello World"
    fmt.Println(str1)

    // 字符串显式类型转换为字节切片
    bytes := []byte(str1)
    fmt.Println(bytes)

    // 字节切片显式类型转换为字符串
    var str2 string = string(bytes)
    fmt.Println(str2)
}
```

![Golang-Golang基础-语法基础-字符串-转换-示例-001.png](Golang-Golang基础-语法基础-字符串-转换-示例-001.png)


字符串的内容是只读不可变的，无法修改，但字节切片是可以修改的

```Go
package main

import "fmt"

func main() {
    var str1 string = "Hello World"
    fmt.Println(str1)

    // 字符串显式类型转换为字节切片
    bytes := []byte(str1)
    fmt.Println(bytes)

    // 修改字节切片
    bytes = append(bytes, ' ', 'a', 'b', 'c', 100, 101, 102)
    fmt.Println(bytes)

    str2 := string(bytes)
    fmt.Printf("str1 = %v\n", str1)
    fmt.Printf("str2 = %v\n", str2)
}
```

![Golang-Golang基础-语法基础-字符串-转换-示例-002.png](Golang-Golang基础-语法基础-字符串-转换-示例-002.png)


将字符串转换为字节切片后，原字符串和字节切片直接毫无关系，因为 Go 会新分配一片内存空间给字节切片，再将字符串的内存复制过去，对字节切片进行修改也不会
对原字符串产生任何影响，这么做是为了内存安全。

<br/>

这种情况下，如果要转换的字符串或字节切片很大，那么性能开销就会很高。不过也可以通过 ```unsafe``` 库来实现无复制转换，不够背后的安全问题需要我们自己
承担。
```Go
package main

import (
    "fmt"
    "unsafe"
)

func main() {
    var str string = "Hello World!"
    
    bytes := unsafe.Slice(unsafe.StringData(str), len(str))

    fmt.Printf("%s %s\n", str, string(bytes))
    fmt.Printf("%p %p", unsafe.StringData(str), unsafe.SliceData(bytes))
}
```

![Golang-Golang基础-语法基础-字符串-转换-示例-003.png](Golang-Golang基础-语法基础-字符串-转换-示例-003.png)


<br/>
<br/>


## 长度

字符串的长度，其实并不是字面量的长度，而是字节数组的长度，只是大多数时候都是 ```ASCII``` 字符，刚好能用一个字符表示，所以恰好与字面量长度相等，求
字符串长度使用内置函数 ```len```

```Go
package main

import (
    "fmt"
)

func main() {
    // 字面量长度为 11
    var str1 string = "Hello World"
    // 字面量长度为 4
    var str2 string = "你好世界"

    fmt.Println(len(str1), len(str2))
}
```

![Golang-Golang基础-语法基础-字符串-长度-示例-001.png](Golang-Golang基础-语法基础-字符串-长度-示例-001.png)

看起来中文字符串比英文字符串短，但是实际上求得的长度却比英文字符串长。这是因为在 ```unicode``` 编码中，一个汉字在大多数情况下占 3 个字节，一个英
文字符只占一个字符，通过输出字符串第一个元素就可以看出来。

```Go
package main

import (
    "fmt"
)

func main() {
    var str1 string = "Hello World"
    fmt.Println(str1[0], string(str1[0]))

    var str2 string = "你好世界"
    fmt.Println(str2[0], string(str2[0]))
}
```

![Golang-Golang基础-语法基础-字符串-长度-示例-002.png](Golang-Golang基础-语法基础-字符串-长度-示例-002.png)


<br/>
<br/>

## 拷贝

类似数组切片的拷贝方式，字符串拷贝其实是字节切片拷贝，需要借助内置函数 ```copy```

```Go
package main

import "fmt"

func main() {
    var src string = "Hello World"

    desBytes := make([]byte, len(src))
    copy(desBytes, src)

    dest := string(desBytes)
    fmt.Println(src, dest)
}
```

![Golang-Golang基础-语法基础-字符串-拷贝-示例-001.png](Golang-Golang基础-语法基础-字符串-拷贝-示例-001.png)

也可以使用 ```strings.clone``` 函数，但是其内部实现都差不多

```Go
package main

import (
    "fmt"
    "strings"
)

func main() {
    var src string = "Hello World"

    dest := strings.Clone(src)
    fmt.Println(src, dest)
}
```

![Golang-Golang基础-语法基础-字符串-拷贝-示例-002.png](Golang-Golang基础-语法基础-字符串-拷贝-示例-002.png)


<br/>
<br/>

## 拼接

字符串的拼接使用 ```+``` 操作符

```Go
package main

import "fmt"

func main() {
    str1 := "Hello"
    fmt.Println(str1)
    
    str2 := "World"
    fmt.Println(str2)
    
    str := str1 + " " + str2
    fmt.Println(str)
}
```

![Golang-Golang基础-语法基础-字符串-拼接-示例-001.png](Golang-Golang基础-语法基础-字符串-拼接-示例-001.png)

也可以将字符串转为字节切片后再进行元素添加

```Go
package main

import "fmt"

func main() {
    str1 := "Hello"
    fmt.Println(str1)

    bytes := []byte(str1)
    bytes = append(bytes, " World"...)

    str := string(bytes)
    fmt.Println(str)
}
```

![Golang-Golang基础-语法基础-字符串-拼接-示例-002.png](Golang-Golang基础-语法基础-字符串-拼接-示例-002.png)

但是上面这两种拼接方式的性能都很差，一般情况下可以使用，但如果对性能有更高的要求，可以使用 ```strings.Builder```

```Go
package main

import (
    "fmt"
    "strings"
)

func main() {
    builder := strings.Builder{}
    builder.WriteString("Hello")
    builder.WriteString(" ")
    builder.WriteString("World")

    fmt.Println(builder.String())
}
```

![Golang-Golang基础-语法基础-字符串-拼接-示例-003.png](Golang-Golang基础-语法基础-字符串-拼接-示例-003.png)


<br/>
<br/>

## 遍历

Go 中的字符串就是一个只读的字节切片，也就是说字符串的组成单位是字节而不是字符。这种情况经常会在遍历字符串时遇到。

```Go
package main

import "fmt"

func main() {
    var str string = "Hello World!"

    for i := 0; i < len(str); i++ {
        fmt.Printf("%d\t%x\t%s\n", str[i], str[i], string(str[i]))
    }
}
```

![Golang-Golang基础-语法基础-字符串-遍历-示例-001.png](Golang-Golang基础-语法基础-字符串-遍历-示例-001.png)


上面例子中的字符都属于 ```ASCII``` 字符，只需要一个字节就能表示，所以结果恰巧每一个字节对应一个字符。但如果包含非 ```ASCII``` 字符结果就不同了。

```Go
package main

import "fmt"

func main() {
    var str string = "Hello 世界!"

    for i := 0; i < len(str); i++ {
        fmt.Printf("%d\t%x\t%s\n", str[i], str[i], string(str[i]))
    }
}
```

![Golang-Golang基础-语法基础-字符串-遍历-示例-002.png](Golang-Golang基础-语法基础-字符串-遍历-示例-002.png)

按照字节来遍历会把中文字符拆开，这样显然会出现乱码。Go 字符串是明确支持 ```UTF-8``` 的，应对这种情况就需要用到 ```rune``` 类型，在使用
```for range``` 进行遍历时，其默认的遍历单位类型就是一个 ```rune```

```Go
package main

import "fmt"

func main() {
    var str string = "Hello 世界!"

    for i, v := range str {
        fmt.Printf("%d\t%x\t%s\n", i, v, string(v))
    }
}
```

![Golang-Golang基础-语法基础-字符串-遍历-示例-003.png](Golang-Golang基础-语法基础-字符串-遍历-示例-003.png)


```rune``` 本质上是 ```int32``` 的类型别名，```unicode``` 字符集的范围位于 0x0000 - 0x10FFFF 之间，最大也只有 3 个字节，合法的 ```UTF-8```
编码最大字节数只有 4 个字节，所以使用 ```int32``` 来存储是理所当然，上面的示例中将字符串换为 ```[]rune``` 再遍历也是一样的道理。

```Go
package main

import "fmt"

func main() {
    var str string = "Hello 世界!"

    runes := []rune(str)
    for i := 0; i < len(runes); i++ {
        fmt.Println(string(runes[i]))
    }
}
```

![Golang-Golang基础-语法基础-字符串-遍历-示例-004.png](Golang-Golang基础-语法基础-字符串-遍历-示例-004.png)

还可以使用 ```utf8``` 包下的工具

```Go
package main

import (
    "fmt"
    "unicode/utf8"
)

func main() {
    var str string = "Hello 世界!"

    for i, w := 0, 0; i < len(str); i += w {
        r, width := utf8.DecodeRuneInString(str[i:])
        fmt.Printf("%c\t%d\n", r, width)
        w = width
    }
}
```

![Golang-Golang基础-语法基础-字符串-遍历-示例-005.png](Golang-Golang基础-语法基础-字符串-遍历-示例-005.png)

<br/>

关于字符串的更多细节，可以查阅 [Strings, bytes, runes and characters in Go](https://go.dev/blog/strings)
