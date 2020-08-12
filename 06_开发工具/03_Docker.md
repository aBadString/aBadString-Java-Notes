<h1 align="center">Docker</h1>
<!-- @import "[TOC]" {cmd="toc"} -->

<!-- code_chunk_output -->

- [1. Docker 的安装和启动](#1-docker-的安装和启动)
- [2. Docker常用命令](#2-docker常用命令)
  - [2.1. 镜像操作](#21-镜像操作)
  - [2.2. 容器操作](#22-容器操作)
  - [2.3. 安装 MySQL 和 Tomcat](#23-安装-mysql-和-tomcat)
  - [2.4. 配置 Docker 远程连接](#24-配置-docker-远程连接)
- [3. Dockerfile](#3-dockerfile)
  - [3.1. 使用 Dockerfile 定制镜像](#31-使用-dockerfile-定制镜像)
  - [3.2. 指令](#32-指令)

<!-- /code_chunk_output -->

# 1. Docker 的安装和启动

安装 Docker

```shell
sudo apt install docker.io
```

启动 Docker 服务
```shell
# 启动docker
systemctl start docker 
# 开机启动docker 
systemctl enable docker
```

# 2. Docker常用命令

## 2.1. 镜像操作

> 镜像网站 https://hub.docker.com/

```shell
# 检索 根据关键字在仓库检索所有镜像
docker search mysql

# 拉取 docker pull 镜像名:tag :tag是可选的，tag表示标签，多为软件的版本，默认是 latest, 最新版本
docker pull mysql:8.0.15

# 列表 查看所有本地镜像
docker images

# 删除 删除指定的本地镜像
docker rmi [image-id] 
```


## 2.2. 容器操作

```shell
# 根据镜像创建并启动容器
docker run ‐‐name mytomcat ‐d ‐p 8888:8080 tomcat:latest
# ‐‐name: 容器名字
# -d: 后台运行
# tomcat:latest 镜像
# ‐p 8888:8080: 端口映射, 主机端口:Docker容器端口


# 查看运行中的容器
docker ps

# 查看所有的容器 
docker ps ‐a 

# 停止运行中的容器 
docker stop 容器的id

# 启动容器 
docker start 容器id 

# 删除一个容器 
docker rm 容器id
```


## 2.3. 安装 MySQL 和 Tomcat

```shell
# 1、下载 MySQL 镜像
sudo docker pull mysql:8.0.15 

# 2、根据镜像创建并启动 MySQL 容器
sudo docker run ‐p 3306:3306 ‐‐name MySQL ‐e MYSQL_ROOT_PASSWORD=123456 ‐d mysql:8.0.15
# ‐e MYSQL_ROOT_PASSWORD=123456: 指定 root 的密码

# 3、下载 Tomcat 镜像
sudo docker pull tomcat:8.0.52
# 4、根据镜像创建并启动 Tomcat 容器
sudo docker run ‐p 8080:8080 ‐‐name Tomcat --restart=always -v /home/abadstring/tomcat/webapps:/usr/local/tomcat/webapps ‐d tomcat:8.0.52
# -v /home/abadstring/tomcat/webapps:/usr/local/tomcat/webapps
# 挂载目录, 主机目录:容器目录
# --restart=always : 开机自启动
```

如果已经创建好的容器，更改为自启动
```shell
docker update --restart=always <容器名字>
```

## 2.4. 配置 Docker 远程连接

1、修改docker配置文件
```
vi /usr/lib/systemd/system/docker.service
```
把`ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock`这一行
修改成`ExecStart=/usr/bin/dockerd   -H tcp://0.0.0.0:2375 -H unix://var/run/docker.sock`

2、运行下面两条命令，使配置生效
```shell
systemctl daemon-reload && service docker restart
```

3、测试连接
```shell
docker -H 192.168.1.100 info
```
浏览器访问：http://192.168.1.100:2375/version

4、IDEA 连接
1. 打开：File | Settings | Build, Execution, Deployment | Docker
2. 添加一个，选择 TCP socket，填写：tcp://192.168.1.100:2375


# 3. Dockerfile

## 3.1. 使用 Dockerfile 定制镜像

```dockerfile
FROM nginx
RUN echo '这是一个本地构建的nginx镜像' > /usr/share/nginx/html/index.html
```
定制一个 nginx 镜像，构建好的镜像内会有一个 /usr/share/nginx/html/index.html 文件

## 3.2. 指令

**FROM**：
定制的镜像都是基于 FROM 的镜像，这里的 nginx 就是定制需要的基础镜像。后续的操作都是基于 nginx。

**RUN**：
用于执行后面跟着的命令行命令。有以下俩种格式：
- Shell 格式 `RUN <命令行命令>`
- Exec 格式 `RUN ["可执行文件", "参数1", "参数2"]` 例如：`RUN ["./test.php", "dev", "offline"]`

**ENV**
设置环境变量，定义了环境变量，那么在后续的指令中，就可以使用这个环境变量。
```dockerfile
ENV <key> <value>
ENV <key1>=<value1> <key2>=<value2> ...
```

**ARG**
构建参数。ARG 设置的环境变量仅对 Dockerfile 内有效，也就是说只有 docker build 的过程中有效，构建好的镜像内不存在此环境变量。
```dockerfile
ARG <参数名>[=<默认值>]
```

**COPY**
复制指令，从上下文目录中复制文件或者目录到容器里指定路径。
```dockerfile
COPY [--chown=<user>:<group>] <源路径1>...  <目标路径>
COPY [--chown=<user>:<group>] ["<源路径1>",...  "<目标路径>"]
```
[--chown=<user>:<group>]：可选参数，用户改变复制到容器内文件的拥有者和属组。
<源路径>：源文件或者源目录，这里可以是通配符表达式
<目标路径>：容器内的指定路径，该路径不用事先建好，路径不存在的话，会自动创建。

**VOLUME**
定义匿名数据卷。在启动容器时忘记挂载数据卷，会自动挂载到匿名卷。
```dockerfile
VOLUME ["<路径1>", "<路径2>"...]
VOLUME <路径>
```
匿名volume在host机器上的目录路径类似于：`/var/lib/docker/volumes/300c2264cd0acfe862507eedf156eb61c197720f69e7e9a053c87c2182b2e7d8/_data`。

**WORKDIR**
指定工作目录。指定的工作目录，必须是提前创建好的。
```dockerfile
WORKDIR <工作目录路径>
```