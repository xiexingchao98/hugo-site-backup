---
title: "在Windows上使用Linux"
date: 2019-05-31T08:59:47+08:00
tags:
- Virtual Machine
draft: true
---

本文介绍如何在 Windows 上使用 Linux 。

## 第一步：安装虚拟机软件

常用的虚拟机软件有 Virtual Machine，Virtual Box，Hyper-V（Windows 专业版自带）。

这里我们使用 Virtual Machine 。

下载地址：https://download3.vmware.com/software/wkst/file/VMware-workstation-full-12.5.7-5813279.exe

密匙：`5A02H-AU243-TZJ49-GTC7K-3C61N`

## 第二步：下载系统镜像ISO文件

Linux 有许多不同的版本，这里我们使用 Fedora（它与 CentOS 并无太大区别，只是一般来说，前者在客户端使用，后者在服务端使用，它们都隶属于 Red Hat）。

官方地址：https://download.fedoraproject.org/pub/fedora/linux/releases/30/Workstation/x86_64/iso/Fedora-Workstation-Live-x86_64-30-1.2.iso

如果上面的地址下载太慢，可以去我搭建的资源站点下载：<http://120.78.163.56/>

## 第三步：创建新的虚拟机

### 1. 打开 Virtual Machine，选择创建新的虚拟机

![](/img/use-linux-on-windows/2019-05-31_124313.png)

### 2. 设置安装类型为典型

![](/img/use-linux-on-windows/2019-05-31_124445.png)

### 3. 选择刚才下载的镜像

![](/img/use-linux-on-windows/2019-05-31_124436.png)

### 4. 编辑虚拟机配置

内存 2G、4G 都可以，CPU 核心数取决于你的逻辑处理器数量

![](/img/use-linux-on-windows/2019-05-31_124621.png)
![](/img/use-linux-on-windows/2019-05-31_124701.png)

> 如何查看逻辑处理器数量？
>
> 打开任务管理器，在性能一栏中即可看到。

![](/img/use-linux-on-windows/2019-05-31_124749.png)

### 5. 开启虚拟机

之后按照提示操作即可。
