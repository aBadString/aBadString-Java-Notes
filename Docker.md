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
sudo docker run ‐p 8080:8080 ‐‐name Tomcat -v /home/abadstring/tomcat/webapps:/usr/local/tomcat/webapps ‐d tomcat:8.0.52
# -v /home/abadstring/tomcat/webapps:/usr/local/tomcat/webapps
# 挂载目录, 主机目录:容器目录
```