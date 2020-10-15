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


# 2. CMD

查看WiFi密码
for /f "skip=9 tokens=1,2 delims=:" %i in ('netsh wlan show profiles') do  @echo %j | findstr -i -v echo | netsh wlan show profiles %j key=clear

ipconfig /displaydns      //显示本机上的DNS域名解析列表
ipconfig /flushdns        //删除本机上的DNS域名解析列表

查看指定端口的占用情况
netstat -aon|findstr "9050"

attrib G:\"*" -s -h -r /s /d

磁盘清理
cleanmgr

电源效率诊断报告
powercfg /energy

关机
shutdown     shutdown.exe

DirectX 诊断工具 - 系统和各硬件的信息
dxdiag

软件信息
systeminfo

星球大战
telnet towel.blinkenlights.nl

磁盘检查
chkdsk

explorer－－－－－－－打开资源管理器
logoff－－－－－－－－注销命令
tsshutdn－－－－－－－60秒倒计时关机命令
lusrmgr.msc－－－－－ 本机用户和组
services.msc－－－－－本地服务设置
oobe/msoobe /a－－－－检查XP是否激活
notepad－－－－－－－－打开记事本
cleanmgr－－－－－－－**整理
net start messenger－－－－开始信使服务
net stop messenger－－－－－停止信使服务
compmgmt.msc－－－－－计算机管理
conf－－－－－－－－－－－启动 netmeeting
dvdplay－－－－－－－－DVD播放器
charmap－－－－－－－－启动字符映射表
diskmgmt.msc－－－－磁盘管理实用程序
calc－－－－－－－－－－－启动计算器
dfrg.msc－－－－－－－磁盘碎片整理程序
chkdsk.exe－－－－－Chkdsk磁盘检查
devmgmt.msc－－－ 设备管理器
regsvr32 /u *.dll－－－－停止dll文件运行
drwtsn32－－－－－－ 系统医生
rononce －p －－－－15秒关机
dxdiag－－－－－－－－－检查DirectX信息
regedt32－－－－－－－注册表编辑器
Msconfig.exe－－－系统配置实用程序
rsop.msc－－－－－－－组策略结果集
mem.exe－－－－－－－－显示内存使用情况
regedit.exe－－－－注册表
winchat－－－－－－－－XP自带局域网聊天
progman－－－－－－－－程序管理器
winmsd－－－－－－－－－系统信息
perfmon.msc－－－－计算机性能监测程序
winver－－－－－－－－－检查Windows版本
sfc /scannow－－－－－扫描错误并复原
taskmgr－－－－－任务管理器（2000／xp／2003）
wmimgmt.msc－－－－打开windows管理体系结构(WMI)
wupdmgr－－－－－－－－windows更新程序
w脚本－－－－－－－－windows脚本宿主设置
write－－－－－－－－－－写字板
winmsd－－－－－－－－－系统信息
wiaacmgr－－－－－－－扫描仪和照相机向导
mem.exe－－－－－－－－显示内存使用情况
Msconfig.exe－－－系统配置实用程序
mplayer2－－－－－－－简易widnows media player
mspaint－－－－－－－－画图板
mstsc－－－－－－－－－－远程桌面连接
mplayer2－－－－－－－媒体播放机
magnify－－－－－－－－放大镜实用程序
mmc－－－－－－－－－－－－打开控制台
mobsync－－－－－－－－同步命令
dcomcnfg－－－－－－－打开系统组件服务
ddeshare－－－－－－－打开DDE共享设置
dvdplay－－－－－－－－DVD播放器
net stop messenger－－－－－停止信使服务
net start messenger－－－－开始信使服务
notepad－－－－－－－－打开记事本
nslookup－－－－－－－网络管理的工具向导
ntbackup－－－－－－－系统备份和还原
narrator－－－－－－－屏幕“讲述人”
ntmsmgr.msc－－－－移动存储管理器
ntmsoprq.msc－－－移动存储管理员*作请求
netstat －an－－－－(TC)命令检查接口
syncapp－－－－－－－－创建一个公文包
sysedit－－－－－－－－系统配置编辑器
sigverif－－－－－－－文件签名验证程序
sndrec32－－－－－－－录音机
shrpubw－－－－－－－－创建共享文件夹
secpol.msc－－－－－本地安全策略
syskey－－－－－－－－－系统加密，一旦加密就不能解开，保护windows xp系统的双重密码
services.msc－－－本地服务设置
Sndvol32－－－－－－－音量控制程序
sfc.exe－－－－－－－－系统文件检查器
sfc /scannow－－－windows文件保护
tsshutdn－－－－－－－60秒倒计时关机命令
tourstart－－－－－－xp简介（安装完成后出现的漫游xp程序）
taskmgr－－－－－－－－任务管理器
eventvwr－－－－－－－事件查看器
eudcedit－－－－－－－造字程序
explorer－－－－－－－打开资源管理器
packager－－－－－－－对象包装程序
perfmon.msc－－－－计算机性能监测程序
progman－－－－－－－－程序管理器
regedit.exe－－－－注册表
rsop.msc－－－－－－－组策略结果集
regedt32－－－－－－－注册表编辑器
rononce －p －－－－15秒关机
regsvr32 /u *.dll－－－－停止dll文件运行
regsvr32 /u zipfldr.dll－－－－－－取消ZIP支持
cmd.exe－－－－－－－－cmd命令提示符
chkdsk.exe－－－－－Chkdsk磁盘检查
certmgr.msc－－－－证书管理实用程序
calc－－－－－－－－－－－启动计算器
charmap－－－－－－－－启动字符映射表
cliconfg－－－－－－－SQL SERVER 客户端网络实用程序
Clipbrd－－－－－－－－剪贴板查看器
conf－－－－－－－－－－－启动netmeeting
cleanmgr－－－－－－－**整理
ciadv.msc－－－－－－索引服务程序
osk－－－－－－－－－－－－打开屏幕键盘
iexpress－－－－－－－木马捆绑工具，系统自带
fsmgmt.msc－－－－－共享文件夹管理器
utilman－－－－－－－－辅助工具管理器
gpedit.msc－－－－－组策略