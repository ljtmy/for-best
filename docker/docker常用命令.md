# 0. 帮助命令
```text
docker version        # 显示docker的版本信息
docker info              # 显示docker的系统信息，包括镜像和容器的数量
docker 命令 --help         # 帮助命令
```
# 1.镜像操作
1. `docker pull [镜像名]`
- 将线上的镜像下载到本地

2. `docker images`
- 查看本地库存
![[1.png]]
```bash
-a, --all # 显示所有镜像 (docker images -a)
-q, --quiet # 仅显示镜像id (docker images -q)
```

3. `docker rmi [镜像名]`
- 清理库存。删除不再需要的镜像

4. `docker build -t [名称] .`
- 根据当前目录（`.`）下的 `Dockerfile` 制作镜像

5. `docker search [OPTIONS] TERM`
- `TERM`：你要搜索的关键词（如 nginx、python）
- `[OPTIONS]`：可选参数，用于过滤或控制输出
- **options常用选项**：

|选项|作用|示例|
|---|---|---|
|`--limit N`|限制返回结果数量（默认 25，最大 100）|`docker search --limit 5 redis`|
|`--filter "is-official=true"`|只显示官方镜像|`docker search --filter "is-official=true" mysql`|
|`--filter "stars=100"`|只显示 stars ≥ 100 的镜像|`docker search --filter "stars=1000" python`|
|`--no-trunc`|不截断 DESCRIPTION 内容|`docker search --no-trunc alpine`|

# 2.容器生命周期
1. `docker run [参数] [镜像名]`
将镜像化作实体运行
**常用参数：**
- - `-d`：后台运行。
    
- `-p 主机端口:容器端口`：端口映射。
    
- `--name [名字]`：贴标签。

2. `docker ps`
- 列出当前**正在运行**的容器。
- `docker ps -a` 可以查看所有的容器，包括那些已经停止运行的。

3. `docker stop [容器名或ID]`
- 停止一个正在运行的容器。

4. `docker start [容器名或ID]`
- 唤醒一个已经停止的容器。

5. `docker rm [容器名或ID]`
- 删除容器（注意：为了安全，必须先 `stop` 停止容器后，才能使用 `rm` 删除它）。

# 3.容器交互与排错
1. `docker logs [容器名]`
- 查看工作日记。查看容器在后台运行时的输出日志（例如 Nginx 的网页访问记录，或者代码报错信息）。

2. **`docker exec -it [容器名] /bin/bash`** (或 `sh`)
- 这是一条非常强大的命令，它允许你直接进入正在运行的容器内部，就像进入一台全新的 Linux 电脑一样，可以在里面浏览文件、执行命令。