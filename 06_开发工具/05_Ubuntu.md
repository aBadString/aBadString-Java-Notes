<h1 id="Ubuntu" align="center">Ubuntu</h1>
<!-- @import "[TOC]" {cmd="toc"} -->

<!-- code_chunk_output -->

- [1. Ubuntu 安装与基本配置](#1-ubuntu-安装与基本配置)
  - [1.1. 换源](#11-换源)
  - [1.2. 设置时区](#12-设置时区)
  - [1.3. 安装 SSH](#13-安装-ssh)
  - [1.4. 固定IP地址](#14-固定ip地址)
  - [1.5. 文件上传](#15-文件上传)

<!-- /code_chunk_output -->

# 1. Ubuntu 安装与基本配置

使用 VMware 安装 Ubuntu 服务器版。
创建虚拟机后，启动虚拟机。  
下载地址: https://cn.ubuntu.com/download

## 1.1. 换源

> https://blog.csdn.net/wangyijieonline/article/details/105360138

查看系统代号
```shell
lsb_release -a
```

上[阿里源](http://archive.ubuntu.com/ubuntu/dists/)看看对应的代号的文件是否存在

先备份一下之前的文件
```shell
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
```

进入 VI 编辑器修改下载源文件
```shell
sudo vi /etc/apt/sources.list
```

按 i 进入编辑模式, 把所有的内容都删除, 添加以下内容:
```
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
```

修改完成之后, 按ESC退出编辑模式, 再按 : 进入命令模式, 按 wq! 保存退出  
换源之后执行下面两条命令
```shell
// 更新 ubuntu 系统的apt资源
sudo apt-get update
// 进行 ubuntu 系统与软件更新
sudo apt-get upgrade
```

## 1.2. 设置时区
```shell
timedatectl set-timezone Asia/Shanghai
```
结果：
```shell
date
Fri 02 Oct 2020 02:04:26 PM CST

date -R
Fri, 02 Oct 2020 14:04:34 +0800
```

## 1.3. 安装 SSH

> ssh 错误 ssh: connect to host 192.168.1.106 port 22: Connection refused
> https://blog.csdn.net/bush2582/article/details/18945553

```shell
sudo apt-get install openssh-server
```

从物理机上输入命令即可连接到 ubuntu
```shell
# 连接SSH
ssh 【ip地址】
# 断开连接
logout
```

## 1.4. 固定IP地址

> https://www.cnblogs.com/ubuntuanzhuang/p/13131138.html
> https://blog.csdn.net/sirobot/article/details/107457473
> [个人专属多节点Linux环境打造 · CodeSheep](https://www.bilibili.com/video/BV1bA411b7vs)

==首先在 VM 虚拟机设置 网络适配器 为 桥接模式。==

先备份一下之前的网络配置
```shell
# 网络配置在这个目录下，是个yaml文件
ls /etc/netplan/
# 备份一下先
sudo cp /etc/netplan/00-installer-config.yaml /etc/netplan/00-installer-config.yaml.bak
```

看看文件内容：
```yml
# This is the network config written by 'subiquity'
network:
  ethernets:
    ens33:
      dhcp4: true
  version: 2
```

修改为
```yml
# This is the network config written by 'subiquity'
network:
  ethernets:
    ens33:
      dhcp4: no                     # 关闭动态分配 IPv4 地址
      addresses: [192.168.1.100/24] # 本机固定 IP 和掩码
      gateway4: 192.168.1.1         # 网关
      nameservers:                                                                                                addresses: [192.168.1.1,119.29.29.29]    # DNS 服务器地址 192.168.1.1是局域网内的DNS 119.29.29.29是公网DNS
  version: 2
```
应用新配置 `sudo netplan apply`

注意：
1. 一定要配置公网DNS，不然无法解析域名，不能 ping www.baidu.com。  
DNS 服务器可以到这里找 https://ip.cn/dns.html
2. ping 不通 win10 宿主机？防火墙-入站规则-ICMP 开启 https://blog.csdn.net/wy_bk/article/details/85013933

## 1.5. 文件上传
上传本地文件到服务器
```shell
scp [本地文件] [服务器用户名]@[服务器IP]:[服务器路径]
scp ./HotelManagementSystem.sql abadstring@192.168.1.105:/home/abadstring/
```
