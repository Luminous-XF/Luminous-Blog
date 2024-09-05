# 常量

常量的值无法在运行时改变，一旦赋值过后就无法修改，其值只能来源于：

* 字面量
* 其它常量标识符
* 常量表达式
* 结果是常量的类型转换
* iota

<br/>

常量只能是基本数据类型，不能是：

* 除基本类型以外的其它类型，如结构体、切片、映射表、数组等
* 函数的返回值

<br/>

**常量的值不能被修改，否则无法通过编译**

<br/>
<br/>

## 初始化

常量的声明需要用到 ```const``` 关键字，常量在声明时就必须要初始化一个值，并且常量的类型可以省略，例如

```Go
// 字面量
const name string = "IU"

// 字面量
const msg = "Hello World"

// 字面量
const num = 10

// 常量表达式
const numExpression = (1 + 2 + 3) * 10

// 其它常量表示符
const PI = math.Pi
```

<br/>

如果仅仅只是声明而不指定值，则无法通过编译

```Go
// 仅声明
const PI int
```

在 GoLand 中会有 **Missing value in the const declaration** 错误提示

<br/>

声明多个常量可以使用 ```()``` 来提升可读性

```Go
const (
    size = 15
    length = 10
)
```

在同一个常量分组中，在已经赋值的常量后面的常量可以不用赋值，其值默认就是前一个的值

```Go
const (
    a = 1
    b // 1
    c // 1
    d // 1
)
```

<br/>
<br/>

## iota

```iota``` 是一个内置的常量标识符，通常用于表示一个常量声明中的无类型整数序数，一般在括号中使用

```Go
const iota = 0
```

```Go
const (
    a = iota
    b // 1
    c // 2
    d // 3
)
```

