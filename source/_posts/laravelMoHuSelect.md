---
title: laravel最优雅的模糊查询
date: 2016-09-18 08:27:48
tags: 
	- laravel 
	- select
	- DB 
categories: Laravel奇淫技巧
---
![分页](https://gss0.baidu.com/-4o3dSag_xI4khGko9WTAnF6hhy/bainuo/crop%3D0%2C2%2C470%2C285%3Bw%3D470%3Bq%3D80/sign=e5ebcb6ea98b87d6440df15f3a38040a/9d82d158ccbf6c81d6fd55b7b43eb13532fa40a2.jpg)
<!--more-->
### Create a new post
``` bash
public function index(Request $request)
{
$page     = 0;
$limit    = 10;
$where[]  = ['id','!=',0];
  
if ($request->page) {
    $page = ($request->page-1)*10;
}
  
if ($request->disease) {
    $where[] = ['disease_lists','like',"%,$request->disease%"];
}
  
if ($request->hosname) {
    $where[] = ['name','like',"%$request->hosname%"];
    $limit   = 15;
}
  
$hos_topten = DB::connection('ezhuanzhen')->table('cb_hos')
      ->where($where)
      ->orderBy('browse_num','desc')
      ->skip($page)
      ->take($limit)
      ->select('id','name','level','address','browse_num as hot','tel_list','disease_list')
      ->get();
}
```


进入hexo博客项目的themes/next目录
用文本编辑器打开_config.yml文件
搜索"auto_excerpt",
把enable改为对应的false改为true，然后hexo d -g，再进主页，问题就解决了！


