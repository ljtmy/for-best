---
created: 2026-06-01
tags:
  - go/概念
aliases:
---

切片概念

## 一句话
我的理解是一个可变数组

## 代码锚点
```go
package main

import "fmt"

func main() {
	// ==========================================
	// 【1. 切片的初始化】
	// ==========================================

	// 方式一：var 声明 (零值切片)
	// 此时底层没有分配数组，切片的值为 nil。适用于尚未确定是否需要存入数据的场景。
	var s1 []string
	fmt.Printf("方式一 (var)  : %v, len: %d, cap: %d, 是否为nil: %t\n", s1, len(s1), cap(s1), s1 == nil)

	// 方式二：make 函数 (预分配内存)
	// 指定长度为 0，容量为 3。底层已经分配了真实数组，切片不为 nil。
	// 强烈推荐：如果在 append 前能大概预估容量，用 make 可以避免底层数组频繁扩容造成的性能损耗。
	s2 := make([]string, 0, 3)
	fmt.Printf("方式二 (make) : %v, len: %d, cap: %d, 是否为nil: %t\n", s2, len(s2), cap(s2), s2 == nil)

	// 方式三：字面量直接初始化 (最常用)
	// 直接赋予初始值，Go 会自动计算长度和容量。
	heroes := []string{"蝙蝠侠", "超人", "神奇女侠"}
	fmt.Printf("方式三 (字面量): %v, len: %d, cap: %d\n", heroes, len(heroes), cap(heroes))
	fmt.Println("--------------------------------------------------")

	// ==========================================
	// 【2. 切片的增删改查 (CRUD)】
	// ==========================================

	// 【查 - 索引读取】
	fmt.Printf("第二个英雄是: %s\n", heroes[1]) // 输出: 超人

	// 【增 - 追加元素】
	// 使用 append 向末尾追加元素。注意观察容量 (cap) 可能会随着元素的增加而自动扩容。
	heroes = append(heroes, "闪电侠", "海王")
	fmt.Printf("增加元素后: %v, len: %d, cap: %d\n", heroes, len(heroes), cap(heroes))

	// 【改 - 修改指定索引的值】
	heroes[0] = "黑暗骑士"
	fmt.Printf("修改元素后: %v\n", heroes)

	// 【删 - 删除特定元素】
	// 删除索引为 2 的元素 ("神奇女侠")。
	// 做法：将前面的部分 heroes[:2] 和后面的部分 heroes[3:] 通过 ... 打散并拼接到一起。
	deleteIndex := 2
	heroes = append(heroes[:deleteIndex], heroes[deleteIndex+1:]...)
	fmt.Printf("删除索引2后: %v, len: %d, cap: %d\n", heroes, len(heroes), cap(heroes))
}
```

## 核心要点
- 要点1
- 要点2
- 要点3

## 我的理解
<!-- 用自己的话写，哪怕很幼稚，这是最重要的部分 -->

## 常见坑
<!-- 学习过程中踩过的坑 -->

## 关联
- 和 [[]] 配合使用
- 底层原理是 [[]]
- 使用场景见 [[]]

## 参考
- [链接文字](URL)
