# 工具名：opensnoop

## 1. 作用
用来观察进程打开了哪些文件。它非常适合排查：
```
程序为什么找不到配置文件？
程序到底读取了哪个路径？
权限错误发生在哪个文件？
某个进程频繁访问什么文件？
```
## 2. 常用命令
```bash
sudo execsnoop-bpfcc
```
### 按进程过滤：
假设你只想看 `nginx`：

sudo opensnoop-bpfcc -n nginx

只看某个 PID：

sudo opensnoop-bpfcc -p <PID>

## 3. 关键字段

|字段|含义|
|---|---|
|`PID`|进程 ID|
|`COMM`|进程名|
|`FD`|文件描述符，成功时通常是非负数|
|`ERR`|错误码，`0` 表示成功|
|`PATH`|文件路径
常见错误码

|错误码|含义|
|---|---|
|`2`|`ENOENT`，文件不存在|
|`13`|`EACCES`，权限不足|
|`20`|`ENOTDIR`，路径中某部分不是目录|
## 5. 实验命令
终端1
```bash
cat /etc/hostname
cat /etc/passwd >/dev/null
ls /not-exist-file
```
终端2：
```bash
sudo opensnoop-bpfcc
```
## 6. 我的观察
能看到 date 被执行 5 次。

## 7. 工作场景
排查 CPU 抖动、异常脚本、可疑进程执行。