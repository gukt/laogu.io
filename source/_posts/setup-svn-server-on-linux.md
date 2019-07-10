title: 在linux上搭建svn服务器
s: setup-svn-server-on-linux
date: 2013-02-26 08:40:53
tags: [svn, linux]
---
### 创建svn仓库根目录

在创建代码仓库之前，我们需要创建一个svn仓库根目录，以后所有的代码仓库都是在这个根目录下创建

```
$ mkdir ~/.svn
```

### 创建仓库
```
$ svnadmin create ~/.svn/coconut
```
成功创建后，会在coconut目录下生成一些文件：

```
$ cd ~/.svn/coconut
```

```
$ ls -l
total 16
-rw-r--r--   1 ktgu  staff  229  7 31 17:28 README.txt
drwxr-xr-x   5 ktgu  staff  170  7 31 17:28 conf
drwxr-sr-x  15 ktgu  staff  510  7 31 17:28 db
-r--r--r--   1 ktgu  staff    2  7 31 17:28 format
drwxr-xr-x  11 ktgu  staff  374  7 31 17:28 hooks
drwxr-xr-x   4 ktgu  staff  136  7 31 17:28 locks
```

### 设置访问策略

进入conf目录，编辑svnserve.conf文件（移除行首`#`并顶格）
```
password-db = passwd
```

你也许需要配置匿名用户的访问权限（移除行首`#`并顶格）

```
anon-access = read 
```
> 如果你需要任何人都可以提交更改，则将read改为`write`，如果不希望匿名用户可以访问，则将read改为`none`。建议将改行改为`none`

另外，移除下面两行前面的`#`并顶格，以便设置用户以及访问权限等

```
password-db = passwd
authz-db = authz
```

现在，打开passwd文件，配置用户

```
 ...
[users]
dev1 = aaa
dev2 = aaa
```

上面的配置表示：添加了两个用户dev1，dev2，密码都是aaa。

配置authz，在[groups]下定义用户组，多个用户之间用逗号隔开

```
[groups]
dev = dev1,dev2
```

然后可以对版本库设置不同的访问权限

```
[/]
@dev=rm
```

以上表示：让`dev`用户组对版本库的所有路径都拥有读写（`rw`）权限


### 启动svn服务器
```
$ svnserve -d -r ~/.svn/  
```
没有提示表示成功（Linux中没有提示就是最好的提示）


### 停止svn服务器
```
$ killall svnserve
```
 
### 测试正常运行

如果以下命令可以正常运行，说明配置成功

```
$ svn checkout svn://127.0.0.1/coconut/ ~/coconut
```

以上命令首次运行会提示输入用户名和密码（使用在passwd中配置的用户名和密码即可）

或者可以在运行`svn checkout`命令时指定用户名和密码

```
$svn checkout svn://127.0.0.1/coconut --username=dev1 --password=aaa ~/coconut
```
