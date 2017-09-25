---
title: MAC开发工具大杂烩
date: 2017-06-24 01:51:58
tags: 
    - MAC
categories: 开发工具
---
开发必备部分软件安装配置教程

文件打开提示放入垃圾桶打开终端键入：`sudo spctl --master-disable` 输入密码即可
<!--more-->
本人命令放在 `~/bin` 中 在 .zshrc 中 引入 `export PATH='/Users/baby/bin':$PATH` 

# brew：

## 安装：
安装前需要下载Xcode
官网自行安装：[https://brew.sh](https://brew.sh)
## 插件
 ```
 brew install tig  #高亮美化
 
 brew install ccat #美化cat命令
 alias cat=ccat    #替换系统cat命令
 
 brew install tree  #树形目录
 ```
 
## 切换PHP版本
一、使用brew安装php多版本方法

```
brew install php56
brew install php70
```

二、安装切换工具

```
brew install php-version
source $(brew --prefix php-version)/php-version.sh
```

三、查看当前安装的所有版本

```
php-version
```

四、切换版本

```
php-version 5.6.5
```

```
brew list
brew services start homebrew/php/php71
brew services restart php71
brew unlink php71
```

搞定

# Itrem2


## 下载&安装：Itrem2
下载地址:[http://www.iterm2.com/downloads.html](http://www.iterm2.com/downloads.html)
下载完成后直接拖到应用程序中

设置选项：
去除![](http://opxoapzhs.bkt.clouddn.com/15046316197660/15046381900701.jpg)

```
    cd ~
    touch .hushlogin 
```

效果如下：  是不是干净很清爽
![](http://opxoapzhs.bkt.clouddn.com/15046316197660/15046383296331.jpg)
## 自动提示：zsh-autosuggestions
下载&安装
依然是在github中：[zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
手动安装：

```
git clone git://github.com/zsh-users/zsh-autosuggestions ~/.zsh/zsh-autosuggestions
```

打开 `~/.zshrc` 文件 在做后一行换行添加

```
source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh
```

退出iterm2 重新打开即可



![](http://opxoapzhs.bkt.clouddn.com/15046316197660/15046377401155.jpg)
1:com+q 直接退出  2: cond+end 在当前窗口全屏

![](http://opxoapzhs.bkt.clouddn.com/15046316197660/15046378769341.jpg)
快捷键：在任何地方呼出iterm2 . 这里设施的是英文状态下的～ 数字数字1傍边那个


-------
材料：
Oh-My-zsh:[Oh-My-zsh](https://github.com/robbyrussell/oh-my-zsh) || github上搜索
使用via curl下载
输入密码即可
配置文件存在于 `~/.zshrc` 打开它 大概第10行 `ZSH_THEME="robbyrussell"` 我这里是 `ZSH_THEME="cloud"`

打开 [https://draculatheme.com](https://draculatheme.com) 找到iterm2 进行下载
引入选择刚刚下载的主题配置：
![](http://opxoapzhs.bkt.clouddn.com/15046316197660/15046441733710.jpg)


字体设置：
![](http://opxoapzhs.bkt.clouddn.com/15046316197660/15046443304233.jpg)

最终系效果图-
![oic](http://opxoapzhs.bkt.clouddn.com/15046316197660/oicc.png)


### 加在缓慢：

```
sudo rm -rf /private/var/log/asl/*.asl
```

清理了终端 log 就好了 干净无用

### shell 语法高亮
先上图 2333：
![](http://opxoapzhs.bkt.clouddn.com/15046316197660/15050353264309.jpg)

炫不炫 绿色代表存在这个命令 一眼就看出来了，如果打错了，它就是红色的

下载方式有很多，请自行安装，本人使用的手动安装 走你-->
插件git地址: [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

安装起来非常简单，clone到$ZSH_CUSTOM/plugins目录，然后在.zshrc文件正配置一下即可。




# Sublime
下载&安装：
打开sublime text 菜单栏找到 Tools -> install Packge... 稍等片刻即好，编辑器左下角安装状态
![](http://opxoapzhs.bkt.clouddn.com/15046316197660/15046386227539.jpg)

主题：command+shit+p --> install Package --> Material Theme;
地址：[https://github.com/equinusocio/material-theme](https://github.com/equinusocio/material-theme)

配置：
command + ,
```
{
	"color_scheme": "Packages/Material Theme/schemes/Material-Theme.tmTheme",
	"font_face": "Fira Code",
	"font_size": 14,
	"ignored_packages":
	[
		"Vintage"
	],
	"line_padding_bottom": 2,
	"line_padding_top": 2,
	"open_files_in_new_window": false,
	"theme": "Material-Theme.sublime-theme",
	"update_check": false
}
```
对应：配色、字体、字号、【 】、行与行上间距、行与行下间距、禁止打开文件新建窗口、启动时创建窗口、主题、关闭自动更新提示
字体：链接: https://pan.baidu.com/s/1nuCW4Lb 密码: mxvv



## 通过命令行启动
官网地址：[https://www.sublimetext.com/docs/3/osx_command_line.html](https://www.sublimetext.com/docs/3/osx_command_line.html)
```
mkdir ~/bin
ln -s '/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl' ~/bin/subl

cd ~/bin
vi .bashrc
export PATH="/Users/baby/bin:$PATH"
source .bashrc
```
## 效果图
![ite](http://opxoapzhs.bkt.clouddn.com/15046316197660/iter.png)

## 编码问题
针对subl3 

1、按Control + ~打开命令行,然后输入下面这一行代码 代码报错[点我](https://packagecontrol.io/installation) 复制

```
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```

执行之后，必须重启Subl3，才能继续

2、安装ConvertToUTF8
按Command + Shift + P打开万能搜索框
然后输入install package回车，这时候会加载所有的packges列表。看到列表之后再输入ConvertToUTF8回车，就会下载安装这个包。装好之后会看到个包的说明文件，

3.重复2步骤 看到列表之后输入Codecs33回车。
> 原因: 由于 Sublime Text 3 内嵌的 Python 限制，ConvertToUTF8 可能无法正常工作,所以你懂的


# Phpstrome

## 主题安装：
cmd+shift + a 搜索 plugins ->点击 brow reposit... 搜索 Material Theme UI 选中 然后install ，安装完毕重启。
![](http://opxoapzhs.bkt.clouddn.com/15046316197660/15047456737706.jpg)

### 生效
cmd+,Editor-->Color & Fonts 选择主题生效 点击右下角完成
![](http://opxoapzhs.bkt.clouddn.com/15046316197660/15047461885857.jpg)


comd+1 左侧

面包屑：
![](http://opxoapzhs.bkt.clouddn.com/15046316197660/15046512637733.jpg)
tabs placement :

### 讨厌的波浪线
command shift a 搜，warning 去设置一下，或者是 typo
![](http://opxoapzhs.bkt.clouddn.com/15046316197660/15047436875078.jpg)


### Xdebug-phpstome

1.首先下载Debug

2.配置php.ini

```
[xdebug]
xdebug.remote_autostart=1
xdebug.default_enable=1
xdebug.remote_port=9001
xdebug.remote_host=127.0.0.1
xdebug.remote_connect_back=1
xdebug.remote_enable=1
xdebug.idekey=PHPSTORM
```

3.重启下php-fpm

4.配置PHPstorm
    * phpstorm -> preferences 打开设置页面：
    * 然后，再选择 Languages -> php -> Debug 下，将Xdebug 下的Debug port 改成9001，和php.ini中设置的一致

//OK。xdebug端口配置好了

debug也只是针对某一个页面在做的。我们以index.php页面为例，先打开index.php，菜单栏点击 run -->Edit Configurations 打开设置页面：
![](http://opxoapzhs.bkt.clouddn.com/15046316197660/15047153533249.jpg)

设置完成control+d 运行
Cli interpreter 错误 comd+shift+a 输入 Cli interpreter ，enter 
![](http://opxoapzhs.bkt.clouddn.com/15046316197660/15047156552449.jpg)

参考：`http://blog.csdn.net/think2me/article/details/45344489`


# 其他
export = '':$PATH
source ./bashrc 
       .bash_profile
alias filename =''

# Git 

git config --global user.name 'username'
git config --global user.emali 'You-Emali'

ssh-keygen -t rsa -C 'You-Emali'

id_rsa 私钥
id_rsa.pub 弓腰
known_hosts 记录
authorized_keys 和做人弓腰

## 优化git
使用 `git add . ` 、 `git commit -m ` 很繁琐索性优化下
首先在 `~/bin/` touch 一个 .alias 文件 写入：

```
alias gaa="git add ."

alias gc="git commit -m "

alias gp="git push"

alias gs="git status"

#alias gl="git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

#alias gll="git log --graph --abbrev-commit --decorate --all --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(dim white) - %an%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n %C(white)%s%C(reset)'"
```

编辑 `~/.zshrc` 最后加上 `source ~/bin/.alias` 即可。


## 代码规范插件phpcode

### 安装&更新
官网[http://cs.sensiolabs.org](http://cs.sensiolabs.org)
需要使用 PHP 5.3.6 以上的版本

curl 下载：

```
curl -L http://cs.sensiolabs.org/download/php-cs-fixer-v2.phar -o php-cs-fixer
```

wget 下载：

```
wget http://cs.sensiolabs.org/download/php-cs-fixer-v2.phar -O php-cs-fixer

or

wget https://github.com/FriendsOfPHP/PHP-CS-Fixer/releases/download/v2.4.0/php-cs-fixer.phar -O php-cs-fixer
```

下载完成后给可执行的权限，然后移动到 bin 目录下面即可：

```
sudo chmod a+x php-cs-fixer
sudo mv php-cs-fixer /usr/local/bin/php-cs-fixer
```

### 用法
用法也很简单，最基本的命令参数就是 fix，直接执行时会尽可能多的根据默认标准格式化代码

```
# 格式化目录 如果是当前目录的话可以省略目录
php-cs-fixer fix /path/to/dir
# 格式化文件
php-cs-fixer.phar fix /path/to/file
```


# 课外阅读
# 优酷免费去广告
打开 Chrome 的开发者工具，在 console 面板粘贴以下代码然后执行：

```
window.sessionStorage.setItem("P_l_h5", true); 
```

然后刷新，或者你使用其他浏览器的话，也是打开开发者工具，通常快捷键就是 F12，使用上面的代码。

# 释放你的F5(comd+R)
这是官网[browsersync](http://www.browsersync.cn/)
基于node.js
npm install -g browser-sync

启动 BrowserSync
    需要进入项目目录！！
    需要进入项目目录！！
    需要进入项目目录！！

静态网站
```
// --files 路径是相对于运行该命令的项目（目录） 
browser-sync start --server --files "css/*.css"
```

如果您需要监听多个类型的文件，您只需要用逗号隔开。


动态网站
如果您已经有其他本地服务器环境PHP或类似的，您需要使用代理模式。 BrowserSync将通过代理URL(localhost:3000)来查看您的网站。

```
// 主机名可以是ip或域名 这是官网给出的本人MAC无法使用
browser-sync start --proxy "主机名" "css/*.css"
```

在本地创建了一个PHP服务器环境，并通过绑定Browsersync.cn来访问本地服务器，使用以下命令方式，Browsersync将提供一个新的地址localhost:3000来访问Browsersync.cn，并监听其css目录下的所有css文件。

```
browser-sync start --proxy "Browsersync.cn" "css/*.css"
```

无法使用怎么办？

```
browser-sync start --proxy "主机名" --files "css/*.css"
```
ok 可以啦

