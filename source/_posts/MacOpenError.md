---
title: macOS Sierra 如何打开任何来源
date: 2016-09-14 06:42:46
tags:
    - Mac
    - App_Errpr
    - 软件打不开
categories: MAC-DIY
---

一定有很多小朋友遇到过这样的问题，升级到了macOS Sierra，随之而来的是第三方应用都无法打开了，提示无法打开或者扔进废纸篓。浪费了1个小时却得到下图一样的回报，绝望不绝望 意不意外 惊不惊喜 刺不刺激 感觉整个人都崩溃了，下面我会告诉你如何解决。

![](http://opxoapzhs.bkt.clouddn.com/7A3218F3-EB10-4C64-91BA-66517F743FAA.png)
<!--more-->
首先我们来了解下
大家都知道，macOS Sierra之前的系统也是需要手动去打开应用程序-系统偏好设置-安全性和隐私-通用里勾选任何来源，这样操作之后才能打开第三方应用。而到了macOS Sierra同样如此，但是默认是不显示的。「电脑都不让玩了，还怎么活」

# 不想看废话？点我啊 
1.打开终端工具  (往下看教你如何打开终端)
2.输入命令 `sudo spctl --master-disable` 回车执行
3.输入用户名密码 
4.打开设置-隐私 查看
由刚开始的这样
    ![](http://opxoapzhs.bkt.clouddn.com/qq.jpg)
变成这样
    ![](http://opxoapzhs.bkt.clouddn.com/EBE3DD2C-204C-4618-A37B-7EF9D9810A0E.png)
5.完毕 

⚠️ 如果将「隐私权限」改回系统默认，需要重新执行上述命令
## 终端的打开姿势
1. 点击桌面空白处-选择菜单栏「前往」- 实用工具 找到终端双击打开
2. 点击桌面空白处 使用快捷键:command+shift+u  找到终端双击打开
3. 使用command+空格 输入 Terminal 或 终端 回车打开
4. 打开应用程序-实用工具-终端 双击打开
    



