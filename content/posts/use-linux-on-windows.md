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

## 第二步：下载系统镜像

Linux 有许多不同的版本，这里我们使用 Fedora（它与 CentOS 并无太大区别，只是一般来说，前者在客户端使用，后者在服务端使用，它们都隶属于 Red Hat ）。

官方地址：<https://getfedora.org/zh_CN/workstation/download/>

如果上面的地址下载太慢，可以去这里下载：<http://120.78.163.56/>

## 第三步：创建新的虚拟机

1. 打开 Virtual Machine，选择创建新的虚拟机
2. 安装类型为典型
3. 选择刚才下载的镜像
4. 编辑虚拟机配置，（可以设置成这样：RAM 4G，CPU 核心数最大为你的逻辑处理器数量）
5. 开启虚拟机，按照后续引导进行操作即可