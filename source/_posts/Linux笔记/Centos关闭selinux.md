---
title: Centos7关闭selinux
tags:
  - linux
  - 运维
  - Linux笔记
categories:
  - Linux笔记
date: 2019-05-23 09:24:36
---

安装centos默认是开启了selinux服务的。他的功能也是类似防火墙。但是开启selinux 有些连接不能使用，所有我们需要关闭它
## 查看
通过命令`sestatus`可以查看当前的selinux的状态
* 激活的状态显示的内容如下：
```bash
SELinux status:                 enabled  
SELinuxfs mount:                /sys/fs/selinux  
SELinux root directory:         /etc/selinux  
Loaded policy name:             targeted  
Current mode:                   enforcing  
Mode from config file:          enforcing  
Policy MLS status:              enabled  
Policy deny_unknown status:     allowed  
Max kernel policy version:      28
```
* 未激活的状态显示如下：
```bash
SELinux status:                 disabled
```
## 临时关闭设置
通过命令`setenforce`可以临时关闭selinux
```bash
[root] setenforce 0
```
## 永久关闭selinux
永久关闭selinux需要修改selinux的配置文件。文件地址在 */etc/selinux/config* 。
* 通过vim 编辑配置文件
```bash
[root] vim /etc/selinux/config
```
* 将SELINUX = enforcing 设置为 SELINUX=disabled

* 重启电脑 
```bash
[root] reboot
```