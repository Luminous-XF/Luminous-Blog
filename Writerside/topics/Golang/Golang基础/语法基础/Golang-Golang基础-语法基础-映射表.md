# 映射表

一般来说，映射表数据结构实现通常有两种，**哈希表（Hash Table）**和**搜索树（Search Tree）**，区别在于在于前者无序，后者有序。在
Go 中，
`map` 的实现是基于哈希桶（也是一种哈希表），所以也是无序的。

<br/>

## 初始化

在 Go 中，`map` 的键类型必须是可比较的，比如 `string`、`int` 是可比较的，而 `[]int` 是不可比较的，也就无法作为 `map`
的键。初始化一个 `map` 有两种方法。

<br/>

使用字面量初始化

```Go
map[keyType]valueType{}
```

```Go
package main

import "fmt"

func main() {
    mp := map[int]string{
        1: "a",
        2: "b",
        3: "c",
        4: "d",
        5: "e",
    }

    fmt.Println(mp)
}
```

![Golang-Golang基础-语法基础-映射表-初始化-示例-001.png](Golang-Golang基础-语法基础-映射表-初始化-示例-001.png)

第二种是使用内置函数 `make`，对于 `map` 而言，接收两个参数，分别是类型与初始容量

```Go
mp := make(map[int]string, 8)
```

⚠️ `map` 是引用类型，零值或未初始化的 `map` 可以访问，但无法存放元素，所以必须为其分配内存。

```Go
package main

import "fmt"

func main() {
    var mp map[string]int

    mp["a"] = 1

    fmt.Println(mp)
}

```

![Golang-Golang基础-语法基础-映射表-初始化-示例-002.png](Golang-Golang基础-语法基础-映射表-初始化-示例-002.png)


<note style="width: 100%">
在初始化 map 时尽量为其分配一个合理的容量，以减少扩容次数
</note>


<br/>
<br/>

## 访问

访问一个 `map` 的方式就像通过索引访问一个数组一样

```Go
package main

import "fmt"

func main() {
    mp := map[string]int{
        "a": 1,
        "b": 2,
        "c": 3,
    }

    fmt.Printf("key = %s, value = %d\n", "a", mp["a"])
    fmt.Printf("key = %s, value = %d\n", "b", mp["b"])
    fmt.Printf("key = %s, value = %d\n", "c", mp["c"])
    fmt.Printf("key = %s, value = %d\n", "e", mp["e"])
    fmt.Printf("key = %s, value = %d\n", "f", mp["f"])
    fmt.Printf("key = %s, value = %d\n", "g", mp["g"])
}
```

![Golang-Golang基础-语法基础-映射表-访问-示例-001.png](Golang-Golang基础-语法基础-映射表-访问-示例-001.png)


通过上面的示例可以观察到，即使在 `map` 中不存在 `"e"`、`"f"`、`"g"` 这几个键值对，但依旧有返回值。`map` 对于不存在的键其返回值是对应类型的零值，
并且在访问 `map` 的时候其实有两个返回值，第一个返回值是对应类型的值，第二个返回值是一个布尔值，代表该键是否存在。

```Go
package main

import "fmt"

func main() {
    mp := map[string]int{
        "a": 1,
        "b": 2,
        "c": 3,
    }

    if value, exists := mp["a"]; exists {
        fmt.Printf("key = %s, value = %d\n", "a", value)
    } else {
        fmt.Printf("key = %s not exist!\n", "a")
    }

    if value, exists := mp["e"]; exists {
        fmt.Printf("key = %s, value = %d\n", "e", value)
    } else {
        fmt.Printf("key = %s not exist!\n", "e")
    }
}
```

![Golang-Golang基础-语法基础-映射表-访问-示例-002.png](Golang-Golang基础-语法基础-映射表-访问-示例-002.png)


使用 `len` 函数获取 `map` 的长度（`map` 中元素的个数）

```Go
package main

import "fmt"

func main() {
    mp := map[string]int{
        "a": 1,
        "b": 2,
        "c": 3,
        "d": 4,
        "e": 5,
    }

    fmt.Println(len(mp))
}
```

![Golang-Golang基础-语法基础-映射表-访问-示例-003.png](Golang-Golang基础-语法基础-映射表-访问-示例-003.png)


<br/>
<br/>

## 存值

`map` 存值的方式与数组存值的方式类似

```Go
package main

import "fmt"

func main() {
    mp := make(map[string]int)

    mp["a"] = 1
    mp["b"] = 2
    mp["c"] = 3

    fmt.Printf("len = %d, %v\n", len(mp), mp)
}
```

![Golang-Golang基础-语法基础-映射表-存值-示例-001.png](Golang-Golang基础-语法基础-映射表-存值-示例-001.png)


存值时使用已存在的键会覆盖原有的值

```Go
package main

import "fmt"

func main() {
    mp := make(map[string]int)

    mp["a"] = 1
    mp["b"] = 2
    mp["c"] = 3

    fmt.Printf("len = %d, %v\n", len(mp), mp)

    if _, exists := mp["a"]; exists {
        mp["a"] = 100
    }

    fmt.Printf("len = %d, %v\n", len(mp), mp)
}
```

![Golang-Golang基础-语法基础-映射表-存值-示例-002.png](Golang-Golang基础-语法基础-映射表-存值-示例-002.png)


⚠️ 但也存在一个特殊情况，那就是键为 `math.NaN()` 时

```Go
package main

import (
    "fmt"
    "math"
)

func main() {
    mp := make(map[float64]string)

    mp[math.NaN()] = "a"
    mp[math.NaN()] = "b"
    mp[math.NaN()] = "c"

    fmt.Printf("len = %d, %v\n", len(mp), mp)

    _, exists := mp[math.NaN()]
    fmt.Println(exists)

    mp[math.NaN()] = "d"
    fmt.Printf("len = %d, %v\n", len(mp), mp)
}
```

![Golang-Golang基础-语法基础-映射表-存值-示例-003.png](Golang-Golang基础-语法基础-映射表-存值-示例-003.png)

通过结果可以发现相同的键值并没有覆盖，反而还可以存在多个，也无法判断其是否存在，也就无法正常取值。因为 `NaN` 是 `IEEE754` 标准所定义的，其实现是
由底层的汇编指令 `UCOMISD` 完成，只是一个无序比较标量双精度浮点值指令，该指令会考虑到 `NaN` 的情况，因此结果就是任何数字都不等于 `NaN`，`NaN`
也不等于自身，这就造成了每次哈希值都不相同。**所以在使用时应当尽量避免使用 `NaN` 作为 `map` 的键**


<br/>
<br/>

## 删除

删除一个键值对需要使用到内置函数 `delete`

```Go
func delete(m map[Type]Type1, key Type)
```

```Go
package main

import (
    "fmt"
)

func main() {
    mp := map[string]int{
        "a": 1,
        "b": 2,
        "c": 3,
        "d": 4,
        "e": 5,
    }
    fmt.Printf("len = %d, %v\n", len(mp), mp)

    delete(mp, "a")

    fmt.Printf("len = %d, %v\n", len(mp), mp)
}
```

![Golang-Golang基础-语法基础-映射表-删除-示例-001.png](Golang-Golang基础-语法基础-映射表-删除-示例-001.png)


⚠️ 需要注意，如果键值是 `NaN`，甚至无法删除该键值对

```Go
package main

import (
    "fmt"
    "math"
)

func main() {
    mp := make(map[float64]string)

    mp[math.NaN()] = "a"
    mp[math.NaN()] = "b"
    mp[math.NaN()] = "c"

    fmt.Printf("len = %d, %v\n", len(mp), mp)

    delete(mp, math.NaN())

    fmt.Printf("len = %d, %v\n", len(mp), mp)
}
```

![Golang-Golang基础-语法基础-映射表-删除-示例-002.png](Golang-Golang基础-语法基础-映射表-删除-示例-002.png)


<br/>
<br/>

## 遍历

通过 `for range` 可以遍历 `map`
 
```Go
package main

import (
    "fmt"
)

func main() {
    mp := map[string]int{
        "a": 1,
        "b": 2,
        "c": 3,
        "d": 4,
        "e": 5,
    }

    for key, value := range mp {
        fmt.Println(key, value)
    }
}
```

![Golang-Golang基础-语法基础-映射表-遍历-示例-001.png](Golang-Golang基础-语法基础-映射表-遍历-示例-001.png)


`NaN` 虽然无法正常获取，但可以通过遍历访问到

```Go
package main

import (
    "fmt"
    "math"
)

func main() {
    mp := make(map[float64]string)

    mp[math.NaN()] = "a"
    mp[math.NaN()] = "b"
    mp[math.NaN()] = "c"

    for key, value := range mp {
        fmt.Println(key, value)
    }
}
```

![Golang-Golang基础-语法基础-映射表-遍历-示例-002.png](Golang-Golang基础-语法基础-映射表-遍历-示例-002.png)

<br/>
<br/>

## 清空

在 Go 1.21 之前，想要清空 `map`，就只能对 `map` 中的每一个 `key` 进行 `delete`

```Go
package main

import (
    "fmt"
)

func main() {
    mp := map[string]int{
        "a": 1,
        "b": 2,
        "c": 3,
        "d": 4,
        "e": 5,
    }

    fmt.Printf("len = %d, %v\n", len(mp), mp)

    for key, _ := range mp {
        delete(mp, key)
    }

    fmt.Printf("len = %d, %v\n", len(mp), mp)
}
```

![Golang-Golang基础-语法基础-映射表-清空-示例-001.png](Golang-Golang基础-语法基础-映射表-清空-示例-001.png)


Go 1.21 更新了 `clear` 函数，就不用再进行上面的操作了，只需要一个 `clear` 就可以清空

```Go
package main

import (
    "fmt"
)

func main() {
    mp := map[string]int{
        "a": 1,
        "b": 2,
        "c": 3,
        "d": 4,
        "e": 5,
    }

    fmt.Printf("len = %d, %v\n", len(mp), mp)

    clear(mp)

    fmt.Printf("len = %d, %v\n", len(mp), mp)
}
```

![Golang-Golang基础-语法基础-映射表-清空-示例-002.png](Golang-Golang基础-语法基础-映射表-清空-示例-002.png)


<br/>
<br/>

## Set

set 是一种无序的，不包含重复元素的集合，Go 中并没有提供类似的数据结构实现，但是 `map` 的键正是无序且不能重复的，所以可以使用 `map` 来代替

```Go
package main

import (
    "fmt"
    "math/rand"
)

func main() {
    set := make(map[int]struct{}, 10)

    for i := 0; i < 10; i++ {
        set[rand.Intn(100)] = struct{}{}
    }

    fmt.Println(set)
}
```

![Golang-Golang基础-语法基础-映射表-Set-示例-001.png](Golang-Golang基础-语法基础-映射表-Set-示例-001.png)

<note>
一个空的结构体不会占用内存
</note>

<br/>
<br/>

## 注意

`map` 并不是一个并发安全的数据结构，Go 团队认为大多数情况下 `map` 的使用并不涉及高并发场景，引入互斥锁会极大的降低性能，`map` 内部有读写检测机制，
如果冲突会触发 `fatal error`。下面示例中情况有非常大的可能性会触发 `fatal`。

```Go
package main

import (
    "fmt"
    "sync"
)

func main() {
    group := sync.WaitGroup{}

    group.Add(10)
    // map
    mp := make(map[string]int, 10)
    for i := 0; i < 10; i++ {
        go func() {
            // 写操作
            for i := 0; i < 100; i++ {
                mp["KEY"] = 1
            }
            // 读操作
            for i := 0; i < 10; i++ {
                fmt.Println(mp["KEY"])
            }
            group.Done()
        }()
    }
    group.Wait()
}
```

![Golang-Golang基础-语法基础-映射表-注意-示例-001.png](Golang-Golang基础-语法基础-映射表-注意-示例-001.png)