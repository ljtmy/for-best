## 1.dockerfile基本语法与结构
**基本结构：**
```dockerfile
# 注释以 # 开头 
指令 参数
```
### 常用指令：**

```text
FROM        # 基础镜像，一切从这里开始构建
MAINTAINER    # 镜像是谁写的：姓名+邮箱
RUN            # 镜像构建的时候需要运行的命令
ADD            # 步骤：tomcat镜像，这个tomcat压缩包！添加内容
WORKDIR        # 镜像的工作目录
VOLUME        # 挂载的目录
EXPOSE        # 暴露端口配置
CMD            # 指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT    # 指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILD        # 当构建一个被继承DockerFile这个时候就会运行ONBUILD的指令。触发指令。
COPY        # 类似ADD，将我们文件拷贝到镜像中
ENV            # 构建的时候设置环境变量！
```
```

#### 1.1 `FROM`命令
指定基础镜像，必须是第一条非注释指令。
```dockerfile
# 推荐：明确指定标签（避免隐式使用 latest） 
FROM ubuntu:22.04 
FROM node:18-alpine # 轻量级选择 
FROM python:3.11-slim # 官方精简版
```
- **永远不要用 `FROM latest`**！可能导致构建结果不可预测
- 优先选择 `-alpine` 或 `-slim` 镜像（体积小、攻击面小）

#### 1.2 `RUN`指令
在镜像中执行 shell 命令，每条 RUN 会创建一个新层。
```
#  反模式：多条 RUN 导致层数过多 
RUN apt-get update
RUN apt-get install -y nginx 
RUN rm -rf /var/lib/apt/lists/* 
#  最佳实践：合并命令 + 清理缓存 
RUN apt-get update && \ 
apt-get install -y --no-install-recommends nginx && \ 
rm -rf /var/lib/apt/lists/*
```
- 使用 --no-install-recommends 减少依赖
- 合并命令减少镜像层数（但注意可读性）

#### 1.3 `COPY` 和`ADD`

| 指令     | 功能                     | 安全性    |
| ------ | ---------------------- | ------ |
| `COPY` | 仅复制本地文件/目录             | 安全（推荐） |
| `ADD`  | 复制 + 自动解压 tar + 下载 URL | 危险（慎用） |
```
# ✅ 正确用法 COPY ./app /app COPY requirements.txt . 
# ❌ 避免用 ADD 下载远程文件（破坏可重现性） ADD https://example.com/file.tar.gz . 
# 不推荐！ 
# ✅ 改用 curl/wget + RUN RUN curl -L https://example.com/file.tar.gz | tar xz
```
- **优先用 `COPY`，除非你需要自动解压 tar 包**

#### 1.4 `WORKDIR` —— 设置工作目录
设置后续指令的**默认工作路径**（类似 `cd`，但更可靠）

```dockerfile
# ✅ 推荐：显式设置 
WORKDIR WORKDIR /app 
COPY . . 
RUN pip install -r requirements.txt
```
#### 1.5 `ENV` —— 环境变量管理
设置环境变量，**贯穿整个构建过程和容器运行时**。
```dockerfile
# 设置 Python 缓存目录（加速 pip 安装） 
ENV PYTHONDONTWRITEBYTECODE=1 \ 
PYTHONUNBUFFERED=1 
# 版本号作为变量（便于更新） 
ENV NGINX_VERSION=1.25.3 
RUN apt-get install -y nginx=${NGINX_VERSION}
```
- 合并多个 `ENV` 减少层数
- 敏感信息**不要**硬编码在 `ENV` 中（用 `--build-arg` 或 secrets）

#### 1.6 EXPOSE —— 声明端口（文档作用）
**仅声明**容器监听的端口，**不实际发布端口**。
```dockerfile
EXPOSE 80 443 # 告诉用户此镜像使用这些端口
```
#### 1.7`CMD` vs `ENTRYPOINT` 

| 指令           | 作用                   | 可被覆盖？ |
| ------------ | -------------------- | ----- |
| `CMD`        | 默认参数（提供给 ENTRYPOINT） | ✅ 是   |
| `ENTRYPOINT` | 主命令（容器启动时执行）         | ❌ 否   |

#### 1.8 `LABEL` —— 镜像元数据
添加镜像描述信息，便于管理和追踪。
```dockerfile
LABEL maintainer="your@email.com" 
LABEL version="1.0" 
LABEL description="My awesome web app"
```