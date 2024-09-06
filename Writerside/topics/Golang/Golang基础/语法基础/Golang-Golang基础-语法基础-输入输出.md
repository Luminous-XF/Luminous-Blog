# 输入输出

## 标准

```Go
var (
	Stdin  = NewFile(uintptr(syscall.Stdin), "/dev/stdin")
	Stdout = NewFile(uintptr(syscall.Stdout), "/dev/stdout")
	Stderr = NewFile(uintptr(syscall.Stderr), "/dev/stderr")
)
```

在 ```os``` 包下有三个对外暴露的文件描述符，其类型都是 ```*os.File```，分别是：

* ```Stdin``` - 标准输入
* ```Stdout``` - 标准输出
* ```Stderr``` - 标准错误

Go 中控制台输入输出都依赖它们。


<br/>

## 输出

输出 ```Hello World```，常用的有三种方法

1. ```os.Stdout```

```Go
os.Stdout.WriteString("Hello World")
```

2. 内置函数 ```println```

```Go
println("Hello World")
```

3. (推荐) 使用 ```fmt``` 包下的 ```Println``` 函数

```Go
fmt.Println("Hello World")
```

```fmt.Println("Hello World")``` 会用到反射，因此输出的内容通常更容易使人阅读，但性能差强人意。


<br/>
<br/>

## 格式化

使用 ```fmt.Sprintf``` 格式化字符串，使用 ```fmt.Printf``` 输出格式化字符串

| 序号 | 格式化       | 描述                           | 接收类型           |
|----|-----------|------------------------------|----------------|
| 0  | ```%%```  | 输出百分号 %                      | 任意类型           |
| 1  | ```%s```  | 输出 string/[] byte 值          | string,[] byte |
| 2  | ```%q```  | 格式化字符串，输出的字符串两端有双引号 ""       | string,[] byte |
| 3  | ```%d```  | 输出十进制整型值                     | 整型类型           |
| 4  | ```%f```  | 输出浮点数                        | 浮点类型           |
| 5  | ```%e```  | 输出科学计数法形式，也可以用于复数            | 浮点类型           |
| 6  | ```%E```  | 与 %e 相同                      | 浮点类型           |
| 7  | ```%g```  | 根据实际情况判断输出 %f 或者 %e，会去掉多余的 0 | 浮点类型           |
| 8  | ```%b```  | 输出整型的二进制表现形式                 | 数字类型           |
| 9  | ```%#b``` | 输出二进制完整的表现形式                 | 数字类型           |
| 10 | ```%o```  | 输出整型的八进制表示                   | 整型             |
| 11 | ```%#o``` | 输出整型的完整八进制表示                 | 整型             |
| 12 | ```%x```  | 输出整型的小写十六进制表示                | 数字类型           |
| 13 | ```%#x``` | 输出整型的完整小写十六进制表示              | 数字类型           |
| 14 | ```%X```  | 输出整型的大写十六进制表示                | 数字类型           |
| 15 | ```%#X``` | 输出整型的完整大写十六进制表示              | 数字类型           |
| 16 | ```%v```  | 输出值原本的形式，多用于数据结构的输出          | 任意类型           |
| 17 | ```%+v``` | 输出结构体时将加上字段名                 | 任意类型           |
| 18 | ```%#v``` | 输出完整 Go 语法格式的值               | 任意类型           |
| 19 | ```%t```  | 输出布尔值                        | 布尔类型           |
| 20 | ```%T```  | 输出值对应的 Go 语言类型值              | 任意类型           |
| 21 | ```%c```  | 输出 Unicode 码对应的字符            | int32          |
| 22 | ```%U```  | 输出字符对应的 Unicode 码            | rune,byte      |
| 23 | ```%p```  | 输出指针所指向的地址                   | 指针类型           |


<br/>
<br/>

## 输入
输入通常使用 ```fmt``` 包下提供的三个函数

```Go
// 扫描从os.Stdin读入的文本，根据空格分隔，换行也被当作空格
func Scan(a ...any) (n int, err error) 

// 与Scan类似，但是遇到换行停止扫描
func Scanln(a ...any) (n int, err error)

// 根据格式化的字符串扫描
func Scanf(format string, a ...any) (n int, err error)
```

1. ```fmt.Scan```
```Go
package main

import "fmt"

func main() {
    var s1, s2 string
    cnt, _ := fmt.Scan(&s1, &s2)
    fmt.Println(cnt, s1, s2)
}
```

![Golang-Golang基础-输入输出-Scan示例.png](Golang-Golang基础-语法基础-输入输出-Scan示例.png)


2. ```fmt.Scanln```
```Go
package main

import "fmt"

func main() {
    var s1, s2 string
    cnt, _ := fmt.Scanln(&s1, &s2)
    fmt.Println(cnt, s1, s2)
}
```

![Golang-Golang基础-输入输出-Scanln示例.png](Golang-Golang基础-语法基础-输入输出-Scanln示例.png)


3. ```fmt.Scanf```
```Go
package main

import "fmt"

func main() {
    var s1, s2, s3 string
    cnt, _ := fmt.Scanf("%s %s \n %s", &s1, &s2, &s3)
    fmt.Println(cnt, s1, s2, s3)
}
```

![Golang-Golang基础-输入输出-Scanf示例.png](Golang-Golang基础-语法基础-输入输出-Scanf示例.png)

<br/>
<br/>

## 缓冲
对性能有要求时可以使用 ```bufio``` 包进行读取。

**输入**
```Go
package main

import (
    "bufio"
    "fmt"
    "os"
)

func main() {
    scanner := bufio.NewScanner(os.Stdin)
    scanner.Scan()
    fmt.Println(scanner.Text())
}
```
![Golang-Golang基础-输入输出-bufio输入.png](Golang-Golang基础-语法基础-输入输出-bufio输入.png)

**输出**

```Go
package main

import (
    "bufio"
    "fmt"
    "os"
)

func main() {
    writer := bufio.NewWriter(os.Stdout)
    writer.WriteString("Hello World!\n")
    writer.Flush()
    fmt.Println(writer.Buffered())
}
```

![Golang-Golang基础-输入输出-bufio输出.png](Golang-Golang基础-语法基础-输入输出-bufio输出.png)