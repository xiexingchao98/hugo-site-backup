---
title: "了解SSH"
date: 2019-05-13T22:35:52+08:00
categories:
- 笔记
tags:
- ssh
---
## SSH登陆方法

SSH 登陆有 2 种方法：
1. 密码登陆
2. 使用 `sshkey` 登陆

第 1 种，使用密码登陆。简单易记的短密码方便输入，但是不安全。复杂难记的长密码，每次都输一遍，简直在折磨人。

第 2 种，使用 `sshkey` 登陆。本地生成密匙对（公、私匙），将公匙复制到服务端，以后每次登陆默认使用私匙登陆，无需输入密码，只需要保管好私匙即可。

```shell
// 查看本地有无密匙，有的话直接拿来用就可以了
ls ~/.ssh
// 生成密匙，出现提示一路回车默认即可
ssh-keygen -t rsa -C "yourname@email"
```
公匙是以 `.pub` 结尾的文件，如：`id_rsa.pub` 。

最后将公匙内容复制到服务端的`~/.ssh/authorized_keys` 文件中即可。

结束会话，重新登陆，这时已经不需要再输入密码了。

## SSH登陆原理

1. Client 向 Server 发起会话，并指定要使用的 Key id
2. Server 使用已存储的 Public Key 当中对应的 Key 加密一条随机生成的消息，返回给 Client
3. Client 使用 Private key 解密来自 Server 的密文，并将解密的消息返回至 Server
4. Server 对来自 Client 的消息进行验证，如与加密前的消息一致，则允许登陆

上述流程采用的思想叫做[Challenge response authentication](https://en.wikipedia.org/wiki/Challenge%E2%80%93response_authentication)。

简而言之就是这样：Server 给所有想登陆的 Client 出一个挑战，这个挑战只有真正拥有 Private Key 的人才能够完成，完成挑战的 Client 才能登陆到 Server 。

### 参考：
+ https://www.digitalocean.com/community/tutorials/understanding-the-ssh-encryption-and-connection-process
+ https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys