---
title: Laravel 从零开始
date: 2016-09-09 01:15:39
tags:
    - Laravel
    - git
    - phpstome
    - oh my zsh
categories: Laravel奇淫技巧
---
# Laravel 安装
1.下载[composer 官网](https://getcomposer.org)
2.`mv composer.pr /usr/local/bin/composr`
3.`laravel new blog` 或 `composer create-project --prefer-dist laravel/laracel Laravel5`
4.使用 `php artisna serve` 来启动 laravel 框架
<!--more-->
# Laravel 基本工作流程 
routes 路由文件：
你可以想象成为专门处理http请求的，比如get、post等。

以前访问页面都是某个文件夹下面某个PHP文件 访问地址如下：

```
images/index.php
或
www.xxx.com/images/index.php
```

打开 routes/web.php 你会发现里面有三行代码，

```
Route::get('/', function () {
    return view('welcome');
});
```

在Laravel中 你会神奇的发现这是一条get 请求 「／」 目录 下一个匿名方法。
你可以将匿名函数内容改为  `return 'Hello world'` 保存 ，访问 http://127.0.0.1:8000 

你会发现每次访问都要写一个匿名函数，我们通常会采取 Controller控制器 来访问。
下面我们来创建 Controller控制器 ，首先要使用 **终端** 进入你的项目根目录。

```
php artisan make:controller NameController    # 创建一个不带方法的路由

php artisan make:controller NameController --resource #创建一个带方法的路由
```
⚠️ 控制器的默认目录放在 app/Http/Controller 下

打开刚刚创建的 控制器 在里面如下方法

```
public function index()
{
    return view('welcome');
}
```

我们修改 `routes/web.php`

```
 Route::get('/','NameController@index');
 
 //$router->get( '/' , 'NameController@index' );
 
 //Route::resource('／', 'PhotoController'); #我们在后面会讲解到，不要在这里纠结这是什么意思
```

⚠️ 上面2行代码是等效的。

打开浏览器访问 http://127.0.0.1:80 你会惊奇的发现页面没有变化

# Laravel向视图传递变量
这里 「name」是发送给视图的变量，「$name」是变量值

### 单个变量
```
return vier('about.about')->with('name',$name); 
```

### 多个变量
```
#方法1:
return vier('about.about')->with([
    'first'=> 'one',
    'last'=> 'two'
]);     


#方法2:
$array=[];
$array['farst']= 'one';
$array['lost']= 'two';
return vier('about.about',$array);
```

### 多个或单个变量

```
$first= 'one';
$lost= 'two';
return view('about.about',compact('first','lost')); # 个人倾向
```

在模板中使用变量 
>传统写法：

```
<?php $name ?>
```

>laravel写法

```
{{ $name }}   #会转义变量

{!! $name !!}  #不会转义变量 注意xss攻击
```

# Laravel Blade 模板简介
比如我们需要新建一个页面，考虑页面CSS一至性，每次都要 cp ？ ❌ 
其实网们可以这样做 在 views/目录下新建 app.blade.php 

```
<!DOCTYPE HTML>
<html>
<head>
<title></title>
<link rel="stylesheet" type="text/css" href="style/css/style.css"/>
<link rel="stylesheet" type="text/css" href="style/css/external.min.css"/>
</head>
<body>

@yield('content') #重点在这里  设置一块content缓冲区

</body>
</html>
```
其他html页面可以删除body以上代码
```
@extends('app')  #继承刚才新建的app.blade.php

@section('content')  #创建一块content缓冲区
    #body正文
@stop #一个section对应1个stop
```
⚠️ 如果一个页面西药特定文件怎么办 没错就是在创建一块缓冲区

这个你学会了，在来的新技能吧！
## 在模版中使用条件 循环
### 条件判断

```
@if(true)
    <h1> true </h1>
@else
    flast
@endif
```

### 循环

```
@foreach($array as $key) 
    {{ $key }}
@endforeach
```

# Laravel环境配置.env
env('.env名称','默认值')
# 美化phpstorm
https://www.laravist.com/series/phpstorm-the-best-php-ide-you-ever-met/episodes/2

# mac终端美化：
## 安装「oh my zsh」可以自动安装也可以手动安装
**推荐手动安装**
### 手动安装 
[Github地址](https://github.com/zsh-users/zsh-autosuggestions)
```
git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```

### 自动安装

```
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
```

### 替换bash为zsh

```
chsh -s /bin/zsh
```

## 自动提示插件安装
### 下载插件

```
git clone git://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions

#将下命令添加到你的 .zshrc:
source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh
```

## git问题大全
### 执行git push错误时的解决方法
默认情况下在执行 `git push` 时，系统会提示error而无法成功推送。这是由于git默认拒绝了push操作，需要进行设置，修改远程仓库目录中的 .git/config 文件.

**解决方法：**
在后面添加如下代码：

```
[receive]
       denyCurrentBranch = ignore
```       

### 执行git push后，服务器端无法查看到推送内容的原因及解决方法
在初始化远程仓库时最好使用：`git --bare init`，而不要使用：`git init`
如果使用了`git init` 初始化，则远程仓库的目录下也包含 work tree。当本地仓库向远程仓库push时，如果远程仓库正在push的分支上（如果当时不在push的分支，就没有问题）, 那么push后的结果不会反应在work tree上，也即在远程仓库的目录下对应的文件还是之前的内容。

**解决方法：**
远程端必须运行命令：`git reset --hard` 才能看到 push 后的内容。
### 解决因git源代码库路径发生变化，导致本地代码库无法同步的问题
如果执行git clone之后，源代码库的路径发生变化，导致无法与源代码保持一致，可以修改本地代码库的.git/config文件以更新源代码路径，例如：
vi .git/config
内容如下：

```
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[remote "origin"]
        fetch = +refs/heads/*:refs/remotes/origin/*
        url = /home/android/gitTestSource
[branch "master"]
        remote = origin
        merge = refs/heads/master
```
**解决方法：**
如果现在/home/android/gitTestSource目录的文件被移动到了/home/android/project/gitTestSource目录，则可将其中的url = /home/android/gitTestSource改为：url = /home/android/project/gitTestSource。

### git  Unable to create temporary file
1）出现 `Unable to create temporary file: Permission denied`
在Windows上使用TortoiseGit执行Push时出现以下错误

```
git.exe push --force --progress  "origin" master:master
Counting objects: 189, done.
Compressing objects: 100% (187/187)
Writing objects:   7% (14/189)
fatal: Unable to create temporary file: Permission denied
fatal: sha1 file '<stdout>' write error: Invalid argument
error: failed to push some refs to 'git@110.73.4.46:channelv.git'
```
 
`git did not exit cleanly (exit code 1)`
原来是服务器上是用root账户建立的库目录，导致git账户无权写入，方法就是修改文件夹的所属用户和所属用户组

```
chown -R git *
chgrp -R git *
```


2）出现 `failed to push some refs to 'git@110.731.41.46:channelv.git'`

在Windows上使用TortoiseGit执行Push时出现以下错误

```
git.exe push --progress  "origin" master:master
Counting objects: 189, done.
Compressing objects: 100% (158/158)
Writing objects: 100% (189/189), 1016.00 KiB | 997 KiB/s
Writing objects: 100% (189/189), 1.12 MiB | 997 KiB/s, done.
remote: error: 'receive.denyCurrentBranch' configuration variable to 'refuse'.
To git@101.713.4.46:channelv.git
! [remote rejected] master -> master (branch is currently checked out)
error: failed to push some refs to 'git@10.73.4.46:channelv.git'
git did not exit cleanly (exit code 1)
```

在服务器对应的库目录下执行以下命令增加配置即可

`git config --bool core.bare true`

