---
title: Linux命令操作
date: 2019-07-26 18:30:22
categories:
- Linux
tags:
- Linux操作命令
---

Linux使用者逐渐增多，对Linux熟练掌握成为必然，随后，那就必须学会它的操作命令。虽然可能会花费一些时间，不过从长远的角度来说，这的确是一件事半功倍的事情，它会让我们更了解Linux，更灵活地去使用Linux。
<!--more-->
Linux提供了很多命令，接下来简单介绍下Linux的操作命令，系统学习。

#### 查看进程相关
windows操作系统下，已知端口号，查看进程号，以及关闭进程

netstat -ano|findstr 端口号
tskill 进程号
	/f 杀死所有进程及子进程
	/t 强制杀死
	/im 用镜像名称作为进程信息
	/pid 用进程id作为进程信息

linux操作系统下：

1、已知端口号，查看进程号

    命令行：netstat -tunlp|grep 端口号 或 lsof -i :端口号 等等

2、关闭该进程

    命令行：sudo kill 进程号

    注意：如果没有关闭掉，请使用命令'sudo kill -9 进程号'，强制关闭掉

3、查看端口是否已经被占用

    命令行：netstat -tln|grep 端口号


#### 系统操作命令
```
系统
# uname -a               # 查看内核/操作系统/CPU信息
# head -n 1 /etc/issue   # 查看操作系统版本
# cat /proc/cpuinfo      # 查看CPU信息
# hostname               # 查看计算机名
# lspci -tv              # 列出所有PCI设备
# lsusb -tv              # 列出所有USB设备
# lsmod                  # 列出加载的内核模块
# env                    # 查看环境变量
```
```
资源
# free -m                # 查看内存使用量和交换区使用量
# df -h                  # 查看各分区使用情况
# du -sh <目录名>        # 查看指定目录的大小
# grep MemTotal /proc/meminfo   # 查看内存总量
# grep MemFree /proc/meminfo    # 查看空闲内存量
# uptime                 # 查看系统运行时间、用户数、负载
# cat /proc/loadavg      # 查看系统负载
```
```
磁盘和分区
# mount | column -t      # 查看挂接的分区状态
# fdisk -l               # 查看所有分区
# swapon -s              # 查看所有交换分区
# hdparm -i /dev/hda     # 查看磁盘参数(仅适用于IDE设备)
# dmesg | grep IDE       # 查看启动时IDE设备检测状况
```
```
网络
# ifconfig               # 查看所有网络接口的属性
# iptables -L            # 查看防火墙设置
# route -n               # 查看路由表
# netstat -lntp          # 查看所有监听端口
# netstat -antp          # 查看所有已经建立的连接
# netstat -s             # 查看网络统计信息
```
```
进程
# ps -ef                 # 查看所有进程
# top                    # 实时显示进程状态

# ps -e | grep apt		 	# 查看apt-get相关进程
# sudo fuser -k 8000/tcp	#关闭和端口8000相关进程
```
```
用户
# w                      # 查看活动用户
# id <用户名>            # 查看指定用户信息
# last                   # 查看用户登录日志
# cut -d: -f1 /etc/passwd   # 查看系统所有用户
# cut -d: -f1 /etc/group    # 查看系统所有组
# crontab -l             # 查看当前用户的计划任务
```
```
服务
# chkconfig --list       # 列出所有系统服务
# chkconfig --list | grep on    # 列出所有启动的系统服务
```
```
程序
# rpm -qa                # 查看所有安装的软件包
```
