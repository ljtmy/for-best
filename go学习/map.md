# 创建方法

```go
package main

import "fmt"

func main() {
    // 方式一：使用 make() 召唤一个空的 Map (最常用)
    // 意思是：我要一个 Map，它的标签(Key)是字符串(string)，里面的内容(Value)是整数(int)
    m1 := make(map[string]int)
    
    // 方式二：在召唤的同时直接塞入数据 (字面量创建)
    m2 := map[string]int{
        "苹果": 5,
        "香蕉": 10,
    }
    
    // 方式三：只声明，不初始化 (注意！这是一个极其危险的“哑炮”，见后面的防坑指南)
    var m3 map[string]int 
    
    fmt.Println(m1, m2, m3)
}
```
# 增删改查
```go
// 1. 添加或修改 (Add / Update)
// 如果 "橘子" 不存在，就添加它；如果已经存在，就把它的值改成 8。
m1["橘子"] = 8 

// 2. 读取 (Get)
count := m1["橘子"] // count 现在是 8

// 3. 删除 (Delete)
// 使用内置的 delete 工具，撕掉 "香蕉" 这个标签及其内容
delete(m2, "香蕉") 

// 4. 遍历 (Iterate) - 把字典里的东西全倒出来看看
for key, value := range m2 {
    fmt.Println("键:", key, "值:", value)
}
```