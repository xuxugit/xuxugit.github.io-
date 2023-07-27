---
title: Github 总是输入密码解决方法
date: 2023-05-27 18:00:12
tags:
---

## 1、查看项目的提交方式

```shell
git remote -v
```

![](img/3-git-ssh/git-ssh1.png)
<!--more-->

## 2、修改提交方式为 ssh 

先移除旧的提交方式：

```shell
git remote rm origin
```

添加新的提交方式，复制 ssh 地址添加：

![](img/3-git-ssh/git-ssh5.png)

```shell
git remote add origin git@xxx.git
```

此时还没有权限提交，需要将自己的公钥添加到 github 上。

## 3、添加公钥

第一步，生成公钥，在命令行执行（注意替换自己的邮箱）：

密码可填可不填，填的话需要大于5位，不能太简单，一般存储普通项目直接回车跳过即可。
此时再查看.ssh目录，发现多了几个文件

```sh
ssh-keygen -t rsa -C "youemail@example.com"
```

会在自己的家目录生成公钥，之后进入 .ssh 目录：

```
C:\User\(自己用户)\.ssh   # Windows
/home/(自己用户)/.ssh  # Linux
```

![](img/3-git-ssh/git-ssh2.png)

第二步，添加公钥，打开自己的 Github 网页，打开设置，点击 SSH and GPS keys 选项：

![](img/3-git-ssh/git-ssh3.png)

点击 New SSH key ，填写 Title 并拷贝 id_rsa.pub 里边的内容到 Key 中。 

![](img/3-git-ssh/git-ssh4.png)

这样就配置成功了，之后在尝试提交验证一下。ok

