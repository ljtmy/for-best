### 1.1 环境准备

- **Windows 用户**：安装 WSL2 (Ubuntu) 或者在虚拟机（VirtualBox）里装 Ubuntu Server。
    

### 1.2 目录和文件操作

```bash
# 我在哪
pwd
# 看当前目录下的文件
ls
ls -l          # 详细信息
ls -a          # 显示隐藏文件
# 跳转
cd /tmp        # 去临时目录
cd ~           # 回家目录
cd -           # 回上一目录
# 创建/删除目录
mkdir myapp
mkdir -p a/b/c   # 递归创建
rmdir myapp      # 删除空目录
# 文件操作
touch file.txt          # 创建空文件
echo "hello" > file.txt # 写入内容(覆盖)
echo "world" >> file.txt # 追加内容
cat file.txt            # 查看全部内容
less file.txt           # 分页查看（q退出）
head -n 5 file.txt      # 头5行
tail -f file.txt        # 跟踪末尾（日志常用）
# 复制、移动、删除
cp file.txt file2.txt
mv file2.txt file3.txt
rm file3.txt            # 删除文件
rm -rf myapp/           # 强制递归删除目录（慎用！）
```

**练习**

- 在 `/tmp` 下创建一个 `linux-lab` 目录，在里面创建 `log.txt`，写入三行内容，然后复制到 `log2.txt`，最后查看内容。
    

### 1.3 权限管理

```bash

# 查看权限
ls -l file.txt
# 输出示例: -rw-r--r-- 1 user group 0 May 6 12:00 file.txt
# 类型-拥有者-组-其他人
# 修改权限
chmod +x script.sh      # 加可执行
chmod 755 file          # rwxr-xr-x
chmod 600 secret.txt    # 只有拥有者可读写
# 更改归属
chown user:group file
```

你只需要能看懂 `rwx` 和用 `chmod` 修改权限就够了，别深钻。

### 1.4 进程和资源查看


```bash
# 进程快照
ps aux                 # 所有进程
ps aux | grep nginx    # 查找nginx进程
# 动态查看（类似任务管理器）
top   # q 退出
# 磁盘
df -h   # 人类可读的磁盘用量
# 内存
free -h
# CPU信息
lscpu
```

### 1.5 文本处理三剑客（入门用法）


```bash
# grep 搜索文本
grep "error" log.txt          # 包含error的行
grep -i "error" log.txt       # 忽略大小写
grep -r "server" /etc/        # 递归搜索目录
# awk 按列处理（例如取出第1列和第3列）
echo "name age city" | awk '{print $1, $3}'
# 常用于提取ps或kubectl输出的某一列
# sed 替换文本
echo "hello world" | sed 's/world/cloud/g'
sed -i 's/old/new/g' file.txt   # 直接修改文件
```

这三个工具在查看容器日志、统计分析时会反复出现，刚开始会基本检索就够了。

### 1.6 软件包管理

```bash
# Ubuntu/Debian
sudo apt update              # 更新索引
sudo apt install nginx        # 安装
sudo apt remove nginx         # 卸载
# CentOS/RHEL
sudo yum install nginx
```

### 1.7 systemd 服务管理

几乎所有云上 Linux 都用 systemd 管理后台服务。

```bash
systemctl status nginx    # 查看状态
systemctl start nginx     # 启动
systemctl stop nginx      # 停止
systemctl restart nginx   # 重启
systemctl enable nginx    # 开机自启
journalctl -u nginx -f    # 查看服务日志（跟随）
```

### 1.8 基础网络命令


```bash
ip addr show              # 查看IP地址
ping -c 4 8.8.8.8         # 测试连通性
curl http://example.com    # 发HTTP请求
curl -I http://example.com # 只显示响应头
# 查看端口占用
ss -tlnp                  # 列出监听端口
ss -tlnp | grep 80        # 谁在监听80端口
```