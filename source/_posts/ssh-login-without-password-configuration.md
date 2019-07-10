title: 配置SSH无密码登陆
s: ssh-login-without-password-configuration
date: 2015-09-25 23:19:08
tags: [ssh, linux]
---
SSH(Secure Shell)是建立在应用层和传输层基础上的安全协议，利用 SSH 协议可以有效防止远程管理过程中的信息泄露问题，他提供两种方式的验证：

1.	基于口令的安全验证
2.	基于密钥的安全验证

> 其中基于密钥的验证比基于账号口令的验证有更高的安全性而且更方便

下文讲解配置基于密钥的安全验证，让我们可以无密码登陆远程主机。

基本原理：

> 假设我们需要从A机器上登陆远程主机B，我只需要在主机A上生成一对公钥和私钥，然后将公钥文件拷贝到主机B的authorized_keys中，这样A就可以无密码连接B了

以下为操作步骤：

###创建public/private Keys

使用`$ ssh-keygen`命令可以创建公钥和私钥（在每个提示处一路回车）

```
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): 
Created directory '/root/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
5a:7c:2c:bf:6c:4a:a7:40:bb:eb:e8:4f:ac:91:9d:8c root@vm1
The key's randomart image is:
+--[ RSA 2048]----+
|                 |
|                 |
|                 |
|       . .       |
|      . S o      |
|     B = +       |
|    E X . o      |
|     = + +..     |
|   .+o=.ooo      |
+-----------------+
```
生成成功，在`~/.ssh`目录下会生成·`id_rsa`和`id_rsa.pub`两个文件。

###拷贝公钥到远程主机

`ssh-copy-id`命令用于拷贝公钥到远程主机的`.ssh/authorized_keys`文件中，过程中会提示输入一次密码。

```
$ ssh-copy-id hadoop@vm2
The authenticity of host 'vm2 (172.16.115.102)' can't be established.
RSA key fingerprint is 9c:43:77:79:08:1e:23:ec:3b:3b:ad:d0:a1:63:0c:e0.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'vm2,172.16.115.102' (RSA) to the list of known hosts.
hadoop@vm2's password: 
Now try logging into the machine, with "ssh 'hadoop@vm2'", and check in:

  .ssh/authorized_keys

to make sure we haven't added extra keys that you weren't expecting.
```


### 验证无密码登陆
```
$ ssh hadoop@vm2
[此处没有提示输入密码]
```

##@see also:
[http://linuxconfig.org/passwordless-ssh](http://linuxconfig.org/passwordless-ssh)[enter link description here](https://en.wikipedia.org/wiki/Ssh-agent)