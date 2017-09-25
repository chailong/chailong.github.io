---
title: git推送到服务器自动同步到站点目录
date: 2016-10-28 00:52:35
tags: 
    - git 
    - Linux
categories: GIT
---

有这样一种需求，在本地开发，然后push到服务器上，并且自动同步到web站点目录，这样就可以直接看到网页效果了。
<!--more-->
# git推送到服务器自动同步到站点目录

分几个步骤 ?
* 在服务器上创建裸版本库
* 创建web目录
* 本地初始化版本库
* 本地克隆裸版本库
* 设置裸版本库的钩子post-receive
* 本地推送
* 裸版本库接收到推送内容后自动检出到web目录实现同步

## 创建裸版本库：

```
[root@localhost]$ mkdir /home/workspace
[root@localhost]$ cd /home/workspace
[root@localhost]$ git init -bare wwwroot.git
```

## 创建web目录：

```
[root@localhost]$ mkdir -p /home/website/wwwroot
```

~~暂时不需要~~
~~**本地**初始化一个版本库~~
~~[root@admin.pc]$ cd e:~~
~~[root@admin.pc]$ git init local.git~~
~~暂时不需要~~


## 使用ssh协议从服务器上克隆裸版本库内容，这里如果没有配置公钥的话，会提示输入密码。

```
[root@admin.pc e:/local.git/]$ git clone ssh://root@xxx.xxx.x/home/workspace/wwwroot.git
```

## 设置钩子

```
[root@localhost]$ cd /home/workspace/wwwroot.git/hooks
[root@localhost]$ cat > post-receive <<EOF
>#!/bin/bash
>git --work-tree=/home/website/wwwroot checkout -f 
>EOF
[root@localhost]$ chmod +x post-receive
```

在服务器裸版本库目录下有一个hooks目录，进去后添加一个post-receive的文件，将上面>开头的几行代码输进去，可以用vi，我这里用的cat，保存后记得使用chmod将这个文件赋予可执行权限，成为一个shell脚本。其中 --work-tree对应站点文件目录。

第一次push可能会有一些提示，因为裸版本库还什么都没有，你可能需要 git push origin master写全命令，之后就没必要了。


