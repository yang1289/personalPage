---
title: CentOs7配置静态IP
tags:
  - linux
  - 运维
  - Linux笔记
categories:
  - Linux笔记
date: 2019-05-23 00:13:08
---

# CentOs7配置静态IP

> 以下的笔记都是针对于命令行模式的下的设置，GUI的相关设置不做介绍
> 相关说明如下：
> + `#` 该符号表示root的用户
> + `$` 该符号表示非root的登陆用户
> + `root >`表示是在root用户下进行操作

## 配置静态IP

> 所有的操作都是在root的用户下

### 首先看一下自己的网卡信息

```bash
root > ifconfig 
```

可以看到自己的网卡的相关信息

![ifconfig](https://img-blog.csdn.net/20180319223730933)

其中 `ens33`是我们需要的信息。

<!--more-->

### 网卡信息配置目录

获取到信息之后进入到网卡信息的配置目录

```bash
root > cd /etc/sysconfig/network-scripts
```

该目录下有一些相关的网络配置的相关的信息。通过`ls`命令进行查看

> 其中会包含一个ifcfg-ens33的文件，如果没有包含这个文件，就自己建一个这个名称一样的文件就行了。建立文件通过`vim`去创建。
>
> 例如：
>
> ```ba
> root > vim  ifcfg-ens33
> ```
>
> 打开vim编辑器界面，编写内容。

### 编写配置静态IP内容

通过vim 打开或新建的ifcfg-ens33 之后，填入以下内容。

```properties
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="static"         # 使用静态IP地址，默认为dhcp
IPADDR="192.168.241.100"   # 设置的静态IP地址
NETMASK="255.255.255.0"    # 子网掩码
GATEWAY="192.168.241.2"    # 网关地址
DNS1="192.168.241.2"       # DNS服务器
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33"
UUID="95b614cd-79b0-4755-b08d-99f1cca7271b"
DEVICE="ens33"
ONBOOT="yes"             #是否开机启用
```

> 说明：
>
> 如果存在这个网络配置信息文件，里面`=`号后面的内容可能不包含`""`号。所以这个`""`可以也可以不写

### 重启网络服务

重启服务有两种。一种是通过`service`，另一种就是通过`systemctl`方式：

#### service 方式

```bash
root > service network restart
```

#### systemctl方式

```bash
root > systemctl restart network
```

### 查看IP信息

最后通过`ifconfig`查看IP是否修改成功。

```bash
root > ifconfig
```

可以看到ens33的IP地址变成了我们之前设置的那个IP地址了。

