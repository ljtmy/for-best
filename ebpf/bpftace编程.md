## 用法
```bash
bpftrace -e program
```
- **`bpftrace`**：调用 bpftrace 工具本身。
    
- **`-e`**：选项，表示从命令行直接执行一段程序 (e**x**ecute)，而不是从文件读取。
    
- **`'program'`**：用**单引号**括起来的 bpftrace 脚本代码

上述方式为运行单行程序代码，程序还可以被保存在一个文件中，然后通过以下方式运行

```bash
bpftrace file.bt
```
.bt扩展名不是必需的。在文件开始的位置放置一行代码可指定解释器：
```
#!/usr/local/bin/bpftrace
```
告诉操作系统使用哪个解释器

如果将文件使用chmod命令设置为可执行权限，则脚本可像其他文件一样执行
```bash
./file.bt
```
另外bpftrace脚本必须在root下执行

## 程序结构

```bash
probes /filter/ {action}
```
- probes（探针）:指定要监听的内核或用户态事件。
- ​filter （过滤器）：一个可选的布尔表达式，**只有条件为真时**才执行后面的动作。
- {action}（动作）：探针触发且满足条件时要执行的代码块。

### 注释
单行注释使用`//`,多行注释使用`/**/`

## 探针probe

### 探针格式

所有探针都写成：

```text
类型:标识符1[:标识符2[:...]]
```
探针以类型名称开始，然后是一系列以冒号分隔的标识符

标识符的组织形式由探针形式决定:
```
kprobe:vfs_read
uprobe:/bin/bash:readline
```
- kprobe探针对内核态函数进行插桩，只需要一个标识符： 内核函数名
- uprobe探针对用户态函数进行插桩，需要两个标识符：二进制文件路径和函数名


可以使用逗号将多个探针分隔，指向一个动作：

```
probe1,probe2,probe3.......{action}
```

有两个**特殊的探针**类型**不需要标识符**，`BEGIN`和`END`，他们会在程序启动和结束时触发

### 探针通配符

有些探针类型接受通配符

**示例：**

```
kprobe:vfs_*
```
会对所有以vfs开头的内核函数进行插桩。但过多探针同时插桩会造成不必要的性能开销，因此bpftrace可以设置允许同时开启的探针数量。

在使用通配符之前可以使用`bpftrace -l`来进行测试

## 过滤器（filter）
过滤器是一个布尔表达式，决定一个动作是否执行

```
/pid=123/  //只有在内置变量pid=123时才会触发后续动作
/pid/ //等价于/pid!=0/
```

## 动作（action）
一个动作可以是单条语句也可以是使用分号分隔的多条语句。

```
{action1;action2;action3}
```
全部语句的最后也可以加分号。

## Hello world

```
bpftrace -e 'BEGIN {printf{"Hello world!\n"}; }'
```

如果以文本形式书写，则形式如下：

```
#!/usr/local/bin/bpftrace

BEGIN
{
	printf("Hello world!\n")
}
```
程序中的缩进和换行不是必须的。

## 函数
除了`printf`函数，以下为其它常见内置函数：

- exit():退出bpftrace
- str(char *):输入一个指针，返回字符串
- system(format[,arguments...])：在shell中运行命令

## 变量

变量分为3种，**内置变量**，**临时变量**，**映射表变量**

1. **内置变量**：有bpftrace预定义好的，通常是只读的信息源。
内置变量主要包括以下：
- pid：进程id
- comm：进程名称
- nsecs：以纳秒为单位的时间戳单位
- curtask：当前线程的task_struct结构体

2. **临时变量**：用于临时计算，字首加“`$`”为前缀。

```
$x=1;
$y="hello";
$z=(struct task_struct *)curtask;
```
声明`$x`为整数，`$y`为字符串，`$z`为指向task_struct结构体的指针。

3. **映射表变量**：使用BPF映射表来存储对象，名字带有@前缀，可以用作全局存储，在不同动作直接传递数据

```
probe1 {@a=1};
probe2 {$x=@a};
```

以下代码会经常用到：

```
@strat[tid]= nsecs;
```
内置变量nsecs会赋值给一个名为strat,以tid(当前线程id)为key的映射表。这允许每个线程存储自己的时间戳，不用担心被其他线程覆盖。

```
@path[pid,$fd]=str(arg0);
```

这是一个使用复合键的映射表，同时使用内置变量pid和$fd的组合作为key。

## 映射表函数

映射表可以通过特殊的统计函数赋值。这些函数以特殊方式存储和打印数据。

```
@x =count();
```
- 对事件进行统计累计，打印时会打印出累计结果。这里使用了**每CPU特定的映射表**，下面的语句也会进行计数。

```
@x++
```
- 这里使用的是一个全局映射表，不是每个CPU独立的映射表，这里的@x为整数，不是count类型。提供两种支持。

```
@y = sum($x); //对变量$x求和
@z = hist($x); //将$x保存在一个以2的幂为区间的直方图中
```

**映射表函数总结：**

## 实战：对vfs_read()计时

```
#!/usr/local/bin/bpftrace

kprobe:vfs_read
{
        @start[tid] = nsecs; //每个线程的时间戳单独储存

}

kretprobe:vfs_read
/@start[tid]/
{
        $duration_us=(nsecs-@start[tid]) /1000;
        @us =hist($duration_us);
        delete(@start[tid]);
}


```

在函数运行时存储时间戳，在函数退出时计算时间差，从而记录函数用时。