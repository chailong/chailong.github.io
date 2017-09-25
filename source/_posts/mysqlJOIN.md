---
title: Mysql的左链接和右链接区别
date: 2016-10-03 02:35:44
tags:
    - mysql
categories: 数据库
---

很多人在学习活面试的时候会听到这样的问题：Mysql的左链接和右链接有什么区别？

简单的理解：
    左连接: 左表所有与右表满足条件的
    右连接: 右表所有与左表满足条件的
<!--more-->

例子，相信你一看就明白，不需要多说 

```
A表(a1,b1,c1)        B表(a2,b2) 
a1 b1 c1               a2 b2 
01 数学 95              01 张三 
02 语文 90              02 李四 
03 英语 80              04 王五
``` 

左链接
`select A.*,B.* from A left outer join B on(A.a1=B.a2)`
结果是: 

```
a1 b1 c1 a2 b2 
01 数学 95 01 张三 
02 语文 90 02 李四 
03 英语 80 NULL NULL 
```

右链接
`select A.*,B.* from A right outer join B on(A.a1=B.a2)`
结果是: 

```
a1 b1 c1 a2 b2 
01 数学 95 01 张三 
02 语文 90 02 李四 
NULL NULL NULL 04 王五
```


