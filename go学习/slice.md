# 创建方法
```go
// make([]类型, 长度len, 容量cap) cars := make([]int, 3, 5)
cars := make([]int, 3, 5)
cars := []int{10, 20, 30}

arr := [5]int{1, 2, 3, 4, 5} 
cars := arr[1:4] //包含头但不包含尾
```

# 增删改查
```go
cars := []int{10, 20, 30}

// 【查】: 
fmt.Println(cars[1]) // 输出: 20

// 【改】: 
cars[1] = 999
fmt.Println(cars)    // 输出: [10 999 30]
cars := []int{10, 20}
cars = append(cars, 30)       // 单个增加
cars = append(cars, 40, 50)   // 多个增加

//删：
cars := []int{10, 20, 30, 40}

// 目标索引：index = 1
// cars[:1] 是 [10] (拿前面的)
// cars[2:] 是 [30, 40] (拿后面的)
// 最后的 ... 叫做“打散操作符”，负责把后面的切片拆开，一个个塞进前面的切片里
cars = append(cars[:1], cars[2:]...) 

fmt.Println(cars) // 成功输出: [10 30 40]
```