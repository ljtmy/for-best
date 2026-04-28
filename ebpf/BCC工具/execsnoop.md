# 工具名：execsnoop

## 1. 作用
`execsnoop` 用来观察系统中有哪些**新进程被执行**。

## 2. 适用问题
- 系统中是否有短生命周期进程
- 是否有脚本频繁拉起命令
- 是否有可疑命令执行

## 3. 常用命令
```bash
sudo execsnoop-bpfcc
```

## 4. 关键字段
| 字段      | 含义                      |
| ------- | ----------------------- |
| `PCOMM` | 进程名                     |
| `PID`   | 当前进程 ID                 |
| `PPID`  | 父进程 ID，有些版本会显示          |
| `RET`   | `exec` 调用返回值，`0` 通常表示成功 |
| `ARGS`  | 执行参数                    |

## 5. 实验命令
终端1打开：
```bash
for i in {1..5}; do date >/dev/null; sleep 0.2; done
```
终端2打开：
```bash
sudo execsnoop-bpfcc
```
## 6. 我的观察
能看到 date 被执行 5 次。
![[Pasted image 20260428112901.png]]
## 7. 工作场景
排查 CPU 抖动、异常脚本、可疑进程执行。