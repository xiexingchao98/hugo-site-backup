---
title: "使用Fail2ban拒绝恶意访问"
date: 2019-05-01T16:42:49+08:00
categories:
- 问题解决
tags:
- Linux
---



查看当前服务器安全日志 `cat /var/log/secure `。

下面是部分输出结果：

```shell
Apr 30 16:27:40 vultr sshd[8168]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=218.92.0.138  user=root
Apr 30 16:27:40 vultr sshd[8168]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Apr 30 16:27:40 vultr sshd[8164]: Received disconnect from 213.32.20.236 port 33700:11: Bye Bye [preauth]
Apr 30 16:27:40 vultr sshd[8164]: Disconnected from 213.32.20.236 port 33700 [preauth]
Apr 30 16:27:42 vultr sshd[8168]: Failed password for root from 218.92.0.138 port 46532 ssh2
Apr 30 16:27:43 vultr sshd[8168]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Apr 30 16:27:45 vultr sshd[8168]: Failed password for root from 218.92.0.138 port 46532 ssh2
Apr 30 16:27:46 vultr sshd[8168]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Apr 30 16:27:47 vultr sshd[8168]: Failed password for root from 218.92.0.138 port 46532 ssh2
Apr 30 16:27:48 vultr sshd[8168]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Apr 30 16:27:50 vultr sshd[8178]: Invalid user maryl from 104.248.69.142 port 33900
Apr 30 16:27:50 vultr sshd[8178]: input_userauth_request: invalid user maryl [preauth]
Apr 30 16:27:50 vultr sshd[8178]: pam_unix(sshd:auth): check pass; user unknown
Apr 30 16:27:50 vultr sshd[8178]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=104.248.69.142
Apr 30 16:27:50 vultr sshd[8168]: Failed password for root from 218.92.0.138 port 46532 ssh2
Apr 30 16:27:51 vultr sshd[8168]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Apr 30 16:27:52 vultr sshd[8178]: Failed password for invalid user maryl from 104.248.69.142 port 33900 ssh2
Apr 30 16:27:52 vultr sshd[8178]: Received disconnect from 104.248.69.142 port 33900:11: Bye Bye [preauth]
Apr 30 16:27:52 vultr sshd[8178]: Disconnected from 104.248.69.142 port 33900 [preauth]
Apr 30 16:27:52 vultr sshd[8168]: Failed password for root from 218.92.0.138 port 46532 ssh2
Apr 30 16:27:53 vultr sshd[8168]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Apr 30 16:27:55 vultr sshd[8168]: Failed password for root from 218.92.0.138 port 46532 ssh2
Apr 30 16:27:55 vultr sshd[8168]: error: maximum authentication attempts exceeded for root from 218.92.0.138 port 46532 ssh2 [preauth]
Apr 30 16:27:55 vultr sshd[8168]: Disconnecting: Too many authentication failures [preauth]
Apr 30 16:27:55 vultr sshd[8168]: PAM 5 more authentication failures; logname= uid=0 euid=0 tty=ssh ruser= rhost=218.92.0.138  user=root
Apr 30 16:27:55 vultr sshd[8168]: PAM service(sshd) ignoring max retries; 6 > 3
Apr 30 16:27:58 vultr sshd[8186]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=218.92.0.138  user=root
Apr 30 16:27:58 vultr sshd[8186]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Apr 30 16:28:00 vultr sshd[8186]: Failed password for root from 218.92.0.138 port 2631 ssh2
Apr 30 16:28:00 vultr sshd[8186]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Apr 30 16:28:02 vultr sshd[8186]: Failed password for root from 218.92.0.138 port 2631 ssh2
```

总有些莫名其妙的登陆尝试，估计它们是在专门扫描“肉鸡”。

为了遏制这种现象，同时加强服务器的安全性，有必要对登陆尝试进行限制。

这里用 `fail2ban` 来拒绝恶意访问。

>  下载地址：<https://github.com/fail2ban/fail2ban/releases>

下载最新发布的版本，依照官方提示进行安装。

安装后，为了使用 `systemctl` 控制服务，将解压后目录下的 `build/fail2ban.service` 文件复制到 `/usr/lib/systemd/system/` 目录下。自启动的话，将`files/redhat-initd` 复制到 `/etc/init.d` 目录下就行。 

```shell
cd fail2ban-0.10.x
cp build/fail2ban.service /usr/lib/systemd/system/
cp files/redhat-initd /etc/init.d/fail2ban
```

现在就可以使用 `systemctl` 指令管理 `fail2ban` 服务了 。

```shell
systemctl [start|reload|stop|restart] fail2ban
```

安装完成后，需要进行配置。

创建并修改配置文件：

```shell
cd /etc/fail2ban
touch jail.local
vi jail.local
```

配置文件格式如下：

```shell
[sshd]
# 是否开启当前监狱
enabled  = true

# 设置 Filter，过滤器会判定哪些 IP 需要被封禁
filter   = sshd

# 设置 Action，执行器会将那些需要被封禁的 IP 统统封禁
action   = firewallcmd-ipset

# 系统日志目录，根据日志记录，得出哪些 IP 该被封禁
logpath  = /var/log/secure

# 失败重试次数
maxretry = 3

# 指定时间间隔（单位：s），在间隔内失败达到指定次数，即封禁，否则计数器归零
findtime = 86400

# 封禁时间（单位：s），负数表示永久封禁
bantime = -1
```

> 从 CentOS 7 开始，其防火墙由 `firewallcmd` 而不是 `iptables` 控制，所以 `action` 选择了 `firewallcmd-ipset`，具体的 `filter` 和 `action` 可以在 `filter.d` 和 `action.d` 目录下查看。

接下来，启动 `fail2ban`。

```shell
systemctl start fail2ban
```

然后使用 `fail2ban-client` 指令查看监狱情况：

```shell
# 查看概览信息
fail2ban-client status

# 查看指定监狱（sshd）详情
fail2ban-client status sshd

# 封禁 ip
fail2ban-client set sshd banip x.x.x.x

# 解封 ip
fail2ban-client set sshd unbanip x.x.x.x
```

不熟悉的指令可以使用 `fail2ban-client -help` 获取帮助。

## 参考

<http://www.fail2ban.org/wiki/index.php/MANUAL_0_8>

<https://www.linode.com/docs/security/using-fail2ban-for-security/>

