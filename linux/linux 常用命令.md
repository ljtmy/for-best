# 1.文件操作

|命令|功能|常用参数|示例|说明|
|---|---|---|---|---|
|`ls`|列出目录内容|`-l`：详细信息（权限/大小/时间）  <br>`-a`：显示隐藏文件  <br>`-h`：人类可读大小（KB/MB）  <br>`-t`：按修改时间排序|`ls -lah /etc`|显示 `/etc` 目录下所有文件的详细信息，大小以 KB/MB 显示|
|`cp`|复制文件/目录|`-r`：递归复制目录  <br>`-i`：覆盖前询问  <br>`-v`：显示复制过程  <br>`-p`：保留权限/时间戳|`cp -rv ~/docs /backup/`|递归复制 `~/docs` 目录到 `/backup/`，并显示过程|
|`mv`|移动文件/重命名|`-i`：覆盖前询问  <br>`-v`：显示移动过程  <br>`-f`：强制覆盖（不询问）|`mv old_name.txt new_name.txt`  <br>`mv file.txt /tmp/`|重命名文件 或 将文件移动到 `/tmp/` 目录|
|`rm`|删除文件/目录|`-r`：递归删除目录  <br>`-f`：强制删除（不询问）  <br>`-i`：删除前确认  <br>`-v`：显示删除过程|`rm -rf /tmp/cache/`  <br>`rm -i *.log`|危险！ 强制删除 `/tmp/cache/` 目录  <br>安全删除所有 `.log` 文件（逐个确认）|
|`mkdir`|创建目录|`-p`：递归创建多级目录  <br>`-v`：显示创建过程  <br>`-m`：设置权限（如 `755`）|`mkdir -p /data/logs/app`|创建多级目录 `/data/logs/app`（若父目录不存在则自动创建）|
|`rmdir`|删除空目录|`-p`：递归删除空父目录  <br>`-v`：显示删除过程|`rmdir -p empty_dir/sub`|删除 `sub` 目录后，若 `empty_dir` 为空则一并删除|
|`touch`|创建空文件/更新时间戳|`-a`：仅更新访问时间  <br>`-m`：仅更新修改时间  <br>`-t`：指定时间（格式 `[[CC]YY]MMDDhhmm[.ss]`）|`touch new_file.txt`  <br>`touch -t 202605061000 log.txt`|创建空文件 `new_file.txt`  <br>将 `log.txt` 的时间戳设为 2026-05-06 10:00|

# 2.权限管理
|命令|功能|常用参数|示例|说明|
|---|---|---|---|---|
|`chmod`|修改文件/目录的读、写、执行权限|`-R`：递归修改目录及子项  <br>`-v`：显示详细修改过程  <br>`-c`：仅显示被修改的文件|`chmod 755 script.sh`  <br>`chmod -R g+w /data/`  <br>`chmod u+x,g-w,o=r file.txt`|• 数字模式：`r=4, w=2, x=1`（如 `755 = rwxr-xr-x`）  <br>• 符号模式：`u/g/o/a` + `+/-/=` + `r/w/x`|
|`chown`|修改文件/目录的所有者和所属组|`-R`：递归修改  <br>`-v`：显示详细过程  <br>`--from=OLD_OWNER`：仅当当前所有者匹配时修改|`chown alice:dev team_project/`  <br>`chown -R bob /home/bob/docs`  <br>`chown :admins file.log`|• 格式：`[所有者][:所属组]`  <br>• 仅改组：`chown :group file` 或用 `chgrp`|
|`chgrp`|仅修改文件/目录的所属组|`-R`：递归修改  <br>`-v`：显示详细过程|`chgrp developers app.py`  <br>`chgrp -R staff /shared/`|等价于 `chown :group file`，但语义更清晰|
|`umask`|设置默认权限掩码（影响新建文件/目录）|无常用参数（直接使用值）  <br>`-S`：以符号形式显示（如 `u=rwx,g=rx,o=rx`）|`umask 022` → 文件默认 `644`，目录 `755`  <br>`umask 007` → 文件 `660`，目录 `770`|• 文件默认权限 = `666 - umask`  <br>• 目录默认权限 = `777 - umask`  <br>• 临时生效：直接运行；永久生效：写入 `~/.bashrc`|
# 3.进程查看

|命令|功能|常用参数|示例|说明|
|---|---|---|---|---|
|`ps`|静态快照式查看当前进程状态|`-e` / `-A`：显示所有进程  <br>`-f`：完整格式（含 UID、PPID）  <br>`aux`（BSD 风格）：  <br> `a`=所有终端进程, `u`=用户导向, `x`=无控制终端  <br>`-o`：自定义输出字段（如 `pid,cmd,%cpu`）  <br>`-H`：显示线程（而非仅进程）|`ps aux`  <br>`ps -ef \| grep nginx`  <br>`ps -o pid,ppid,cmd,%mem,%cpu -C java`|• 最常用组合：`ps aux`（查看所有进程详情）  <br>• 进程状态码：  <br> `R`=运行中, `S`=睡眠, `D`=不可中断睡眠,  <br> `T`=停止, `Z`=僵尸进程|
|`top`|动态实时监控系统进程与资源|`-d N`：设置刷新间隔（秒）  <br>`-p PID`：仅监控指定 PID  <br>`-u USER`：仅显示某用户进程  <br>`-H`：显示线程（按 `H` 键切换）|`top`  <br>`top -d 2`  <br>`top -p 1234`|• 交互操作：  <br> `P`/`M` = 按 CPU/内存排序  <br> `k` = 终止进程（输入 PID）  <br> `q` = 退出  <br>• 默认每 3 秒刷新一次|
|`htop`|增强版动态监控（需安装）|`-d N`：设置刷新间隔（毫秒）  <br>`-p PID`：监控指定 PID  <br>`-u USER`：过滤用户进程|`htop`  <br>`htop -u www-data`|• 优势：  <br> 彩色界面 + 树状视图（按 `F5`）  <br> 鼠标支持 + 滚动条  <br> 直接通过方向键选择进程并操作（如 `F9` 杀进程）  <br>• 安装：`sudo apt install htop`（Debian/Ubuntu）|
# 4. 网络排查

| 命令        | 功能                       | 常用参数                                                                                         | 示例                                                            | 说明                                                       |
| --------- | ------------------------ | -------------------------------------------------------------------------------------------- | ------------------------------------------------------------- | -------------------------------------------------------- |
| `ping`    | 测试主机间网络连通性（基于 ICMP）      | `-c N`：发送 N 次后停止  <br>`-i S`：间隔 S 秒（默认 1s）  <br>`-W T`：超时 T 秒  <br>`-s SIZE`：指定包大小（字节）       | `ping -c 4 google.com`  <br>`ping -W 2 192.168.1.1`           | • 成功响应表示网络层可达  <br>• 无法 ping 通 ≠ 服务不可用（可能防火墙禁 ICMP）      |
| `netstat` | 显示网络连接、端口监听、路由表等         | `-t`：TCP 连接  <br>`-u`：UDP 连接  <br>`-l`：监听中的端口  <br>`-p`：显示进程 PID/名称  <br>`-n`：以数字形式显示地址/端口   | `netstat -tunlp`  <br>`netstat -an \| grep :80`               | • 经典组合：`-tunlp`（查看所有监听端口及对应进程）  <br>• 已被 `ss` 逐步替代（性能更低） |
| `ss`      | `netstat` 的现代替代品（更快更高效）  | `-t`：TCP 连接  <br>`-u`：UDP 连接  <br>`-l`：监听端口  <br>`-p`：显示进程信息  <br>`-n`：数字格式输出  <br>`-s`：统计摘要 | `ss -tunlp`  <br>`ss -o state established '( dport = :443 )'` | • 输出格式与 `netstat` 类似但速度更快  <br>• 支持高级过滤（如按连接状态筛选）        |
| `curl`    | 测试 HTTP/HTTPS 服务（支持多种协议） | `-I`：仅获取响应头  <br>`-s`：静默模式（                                                                  |                                                               |                                                          |