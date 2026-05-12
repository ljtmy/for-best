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
