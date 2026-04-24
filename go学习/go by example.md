## hello world
``` go
package main //声明包名，表示所有的代码都组织在包里，若包名不为main，表示为给别人调用的工具包
import "fmt" //导入工具包，fmt是format的缩写
func main(){
fmt.Println("Hello!World")
}
```
## values
```go
package main
import "fmt"
func main() {

    fmt.Println("go" + "lang") //字符串可以使用+进行连接
    fmt.Println("1+1 =", 1+1)
    fmt.Println("7.0/3.0 =", 7.0/3.0)
    fmt.Println(true && false)
    fmt.Println(true || false)
    fmt.Println(!true)

}
```
## variable（变量）
```go
package main
import "fmt"
func main() {
    var a = "initial"
    fmt.Println(a)
    var b, c int = 1, 2
    fmt.Println(b, c)
    var d = true
    fmt.Println(d)
    var e int
    fmt.Println(e)
    f := "short"
    fmt.Println(f)
}
```
1. var可以一次定义多个变量，go会自动推断变量的类型，后面也可以加上类型
2. ：= 表示简短声明，声明赋值给变量f，`：=`只能在函数内部使用
## constants（常量）
```go
package main
import (
    "fmt"
    "math"
)
const s string = "constant"
func main() {
    fmt.Println(s)
    const n = 500000000
    const d = 3e20 / n
    fmt.Println(d)
    fmt.Println(int64(d))
    fmt.Println(math.Sin(n))
}
```
- const用于声明一个常量
## For
```go
package main
import "fmt"
func main() {
    i := 1
    for i <= 3 {
        fmt.Println(i)
        i = i + 1
    }
    for j := 7; j <= 9; j++ {
        fmt.Println(j)
    }
    for {
        fmt.Println("loop")
        break
    }
    for n := 0; n <= 5; n++ {
        if n%2 == 0 {
            continue
        }
        fmt.Println(n)
    }
}
```
## IF/Else
```go
package main
import "fmt"
func main() {

    if 7%2 == 0 {
        fmt.Println("7 is even")
    } else {
        fmt.Println("7 is odd")
    }

    if 8%4 == 0 {
        fmt.Println("8 is divisible by 4")
    }

    if num := 9; num < 0 {
        fmt.Println(num, "is negative")
    } else if num < 10 {
        fmt.Println(num, "has 1 digit")
    } else {
        fmt.Println(num, "has multiple digits")
    }
}
```
- `if num := 9; num < 0`带声明的判断，表示num是临时变量，只在if/else片段生效。
## Switch
```go
package main

import (
    "fmt"
    "time"
)
func main() {

    i := 2
    fmt.Print("write ", i, " as ")
    switch i {
    case 1:
        fmt.Println("one")
    case 2:
        fmt.Println("two")
    case 3:
        fmt.Println("three")
    }

    switch time.Now().Weekday() {
    case time.Saturday, time.Sunday:
        fmt.Println("It's the weekend")
    default:
        fmt.Println("It's a weekday")
    }

    t := time.Now()
    switch {
    case t.Hour() < 12:
        fmt.Println("It's before noon")
    default:
        fmt.Println("It's after noon")
    }

    whatAmI := func(i interface{}) {
        switch t := i.(type) {
        case bool:
            fmt.Println("I'm a bool")
        case int:
            fmt.Println("I'm an int")
        default:
            fmt.Printf("Don't know type %T\n", t)
        }
    }
    whatAmI(true)
    whatAmI(1)
    whatAmI("hey")
}
```
## Arrays
```go
package main
import "fmt"
func main() {
    var a [5]int
    fmt.Println("emp:", a)
    a[4] = 100
    fmt.Println("set:", a)
    fmt.Println("get:", a[4])
    fmt.Println("len:", len(a))
    b := [5]int{1, 2, 3, 4, 5}
    fmt.Println("dcl:", b)
    var twoD [2][3]int
    for i := 0; i < 2; i++ {
        for j := 0; j < 3; j++ {
            twoD[i][j] = i + j
        }
    }
    fmt.Println("2d: ", twoD)
}
```
## Slice (切片)
1. 切片和数组的区别：

| 对比项      | 数组 Array   | 切片 Slice   |
| -------- | ---------- | ---------- |
| 长度       | 固定         | 可变         |
| 类型是否包含长度 | 包含         | 不包含        |
| 是否常用     | 较少直接使用     | Go 中非常常用   |
| 是否引用底层数据 | 否，数组本身就是数据 | 是，切片引用底层数组 |
| 传参成本     | 会复制整个数组    | 只复制切片头部    |

2. 切片的本质结构相当于一个结构体：
``` go
type SliceHeader struct {
    Data uintptr // 指向底层数组的指针
    Len  int     // 当前切片长度
    Cap  int     // 当前切片容量
}
指针 pointer -> 指向底层数组某个位置  
长度 len -> 当前能访问多少个元素  
容量 cap -> 从指针位置开始到底层数组末尾最多能扩展多少个元素
```
3. 切片的创建方式
- 直接声明
``` go
var s []int
```
- 使用字面量创建
```go
s := []int{1, 2, 3}
```
- 从数组切过来
```go
arr := [5]int{10, 20, 30, 40, 50}
s := arr[1:4]

fmt.Println(s) // [20 30 40]，arr[start:end],包含start但是不包含end
```
- 从切片再切片
``` go
s1 := []int{10, 20, 30, 40, 50}  
s2 := s1[1:4]  
  
fmt.Println(s2) // [20 30 40]
```
- 使用make创建
```go
s := make([]int, 3) //创建一个长度为3的切片
```

```go
var s []int
```
```go
package main

import "fmt"

func main() {

    s := make([]string, 3) //使用make创建一个长度为3切片
    fmt.Println("emp:", s)

    s[0] = "a"
    s[1] = "b"
    s[2] = "c"
    fmt.Println("set:", s)
    fmt.Println("get:", s[2])

    fmt.Println("len:", len(s))

    s = append(s, "d") //使用append向切片追加数据，注意append的返回值必须接受
    s = append(s, "e", "f")
    fmt.Println("apd:", s)

    c := make([]string, len(s))
    copy(c, s) //用于复制切片内容，copy返回实际复制的元素数量
    fmt.Println("cpy:", c)

    l := s[2:5] //从切片再切片2
    fmt.Println("sl1:", l)

    l = s[:5]
    fmt.Println("sl2:", l)

    l = s[2:] //这种写法包含后续的元素
    fmt.Println("sl3:", l)

    t := []string{"g", "h", "i"}
    fmt.Println("dcl:", t)

    twoD := make([][]int, 3)
    for i := 0; i < 3; i++ {
        innerLen := i + 1
        twoD[i] = make([]int, innerLen)
        for j := 0; j < innerLen; j++ {
            twoD[i][j] = i + j
        }
    }
    fmt.Println("2d: ", twoD)
}
```
