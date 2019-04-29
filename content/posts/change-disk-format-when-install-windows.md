---
title: "安装Windows时更改磁盘格式"
date:  2019-04-29T19:16:56+08:00
categories:
- 问题解决
---
## 安装Window过程中提示：无法在 MBR 格式的磁盘上安装系统

只能想办法在当前环境下将磁盘转换成 GPT 格式。

在网上找了找，解决步骤大致如下：

1. `Shift + F10` 调出 `cmd.exe`
2. 输入 `diskpart`
3. 查看磁盘列表 `list disk`
4. 选择要操作的磁盘 `select disk [磁盘序号(0,1,2 ...) or 卷标（c:,d: ...）]`
5. 清除磁盘数据 `clean`
6. 转换格式 `convert gpt` 

有点疑惑的是 `convert gpt` 这个东西，从 `convert` 命令说明来看，这个指令只能用来将 FAT 和 FAT32 文件系统转换成 NTFS，没提到还可以这样用。

## 参考文章：

1. <https://www.cnblogs.com/showonce/p/8905072.html>
2. <https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/diskpart>
3. <https://docs.microsoft.com/en-us/windows-server/storage/disk-management/change-an-mbr-disk-into-a-gpt-disk>