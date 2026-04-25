`range` 是 Go 语言中配合 `for` 循环使用的一个关键字，专门用来**迭代**（挨个访问）数组、切片、Map、甚至是字符串。
# 创建与基础语法
```go
for 编号, 内容 := range 你的容器 {
    // 在这里对拿到的内容进行操作
}
```
# 使用方法
## 遍历slice
```go
package main
import "fmt"

func main() {
    fruits := []string{"苹果", "香蕉", "橘子"} // 这是一个切片
    
    for index, fruit := range fruits {
        fmt.Printf("第 %d 个水果是 %s\n", index, fruit)
    }
}
// 输出：
// 第 0 个水果是 苹果
// 第 1 个水果是 香蕉
// ...
```
## 遍历map
```go
prices := map[string]int{"苹果": 5, "香蕉": 10}
    
    for key, value := range prices {
        fmt.Println(key, "的价格是:", value, "元")
    }
```
