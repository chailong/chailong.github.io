---
title: hexo折腾日记
date: 2016-08-13 11:52:13
tags:
---
1.安装git和node.js 安装自行百度
2.如果您的计算机已经有这些，恭喜！只要安装Hexo新公共管理

3.安装hexo
```
	npm install -g hexo-cli
```
4.正式的旅程碑
```
hexo init <folder>
cd <folder>
npm install
```
5.如何发布GitHub上
	1）在source目录学新建一个没有后缀名名为CNAME的文件，在里面写入你的域名（不带www）
	2）解析你的域名到GitHub上的仓库名称xxx.github.io （自行百度）
	3）修改hexo目录下站点配置文件找到deploy
```
		deploy:
  			type: git
  			repository: https://github.com/sonny/sonny.github.io.git
  			branch: master
  ```
 	*注意这里的repository填写你的仓库地址，这里是我的*
 	4）你的项目根目下运行发布命令
```
		hexo g -d 
```
	5）搭建 hexo，在执行 hexo deploy 后,出现 error deployer not found:git 的错误:
解决方法1.
		将第3步的type值改为：type: github
解决方法2。
```
	npm install hexo-deployer-git --save
```

6.其余详细教程请参考https://hexo.io/docs/




