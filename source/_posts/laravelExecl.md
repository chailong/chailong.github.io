---
title: Laravel中的Excel
date: 2016-09-08 01:57:23
tags:
    - Laravel
    - Excel
categories: Laravel奇淫技巧
---


Laravel Excel 在 Laravel 5 中集成 PHPOffice 套件中的 PHPExcel ，从而方便我们以优雅的、富有表现力的代码实现Excel/CSV文件的导入和 导出 。
该项目的GitHub地址是： https://github.com/Maatwebsite/Laravel-Excel 。
本文我们将在Laravel中使用Laravel Excel简单实现Excel文件的导入和导出
<!--more-->
# 2、安装&配置
使用Composer安装依赖 
首先在Laravel项目根目录下使用Composer安装依赖：

```
composer require maatwebsite/excel ~2.0.0
```

安装后的设置
在 config/app.php 中注册服务提供者到 providers 数组：

```
Maatwebsite\Excel\ExcelServiceProvider::class,
```

同样在 config/app.php 中注册门面到 aliases 数组：

```
'Excel' => Maatwebsite\Excel\Facades\Excel::class,
```

如果想要对Laravel Excel进行更多的自定义配置，执行如下Artisan命令，执行成功后会在 config 目录下生成一个配置文件 excel.php 。

```
php artisan vendor:publish
```

# 3、导出Excel文件
为了演示Laravel Excel相关功能，我们为本测试创建一个干净的控制器 ExcelController.php ：

```
php artisan make:controller ExcelController --plain
```

然后在 web.php 中定义相关路由：

```
$router->get('excel/export','ExcelController@export');
$router->get('excel/import','ExcelController@import');
```


```
<?php
namespace App\Http\Controllers\Api;
use DB;
use Excel;
use App\Http\Controllers\Controller;
use Illuminate\Http\Request;
class ExcelController extends Controller
{
	Excel文件导出功能
	public function export()
	{
	$info = DB::table('dtsz')
		->select('id', 'name', 'phone', 'yiyuan', 'dlsname', 'pname', 'addtime', 'logintime')
		->get();
	$cellData = [
		['共'.count($info).'条'],
		[' ','姓名', '电话', '所在医院', '推荐人', '所在地区', '注册日期', '最后登录日期', '总计登录天数', '转诊病人数(含新转诊)', '总计提现金额(含未审核)', '剩余积分(含未审核)'],
	];
	foreach ($info as $k => $v) {
		$count  = DB::table('patients')->where('uid', $v->id)->count();
		$get    = DB::table('jifenlog')->where('uid', $v->id)->where('sztype', 0)->select(DB::raw('SUM(jifen) as total_sales'))->first();
		$out    = DB::table('jifenlog')->where('uid', $v->id)->where('sztype', 1)->select(DB::raw('SUM(jifen) as total_sales'))->first();
		$cellData[$k + 2][] = $k + 1;
		$cellData[$k + 2][] = $v->name ? $v->name : '';
		$cellData[$k + 2][] = $v->phone ? $v->phone : '';
		$cellData[$k + 2][] = $v->yiyuan ? $v->yiyuan : '';
		$cellData[$k + 2][] = $v->dlsname ? $v->dlsname : '';
		$cellData[$k + 2][] = $v->pname ? $v->pname : '';
		$cellData[$k + 2][] = date('Y-m-d H:i:s', $v->addtime);
		$cellData[$k + 2][] = date('Y-m-d H:i:s', $v->logintime);
		$cellData[$k + 2][] = $v->day = 0;
		$cellData[$k + 2][] = $v->count_human = $count;
		$cellData[$k + 2][] = $v->count_sum = $get->total_sales ? $get->total_sales : 0;
		$cellData[$k + 2][] = $out->total_sales ? $out->total_sales : 0;
	}
	Excel::create('召回用户黑名单', function ($excel) use ($cellData) {
		$excel->sheet('score', function ($sheet) use ($cellData) {
			$sheet->rows($cellData);
		});
	})->export('xls');
	}
}
```

我们在浏览器中访问 http://localhost:8000/excel/export ，会导出一个名为 召回用户黑名单.xls 的Excel文件：

![111](http://oe2qf8as1.bkt.clouddn.com/854F.tm.png)

如果你要导出csv或者xlsx文件，只需将 export 方法中的参数改成csv或xlsx即可。
如果还要将该Excel文件保存到服务器上，可以使用 store 方法：

```
Excel::create('召回用户黑名单',function($excel) use ($cellData){
     $excel->sheet('score', function($sheet) use ($cellData){
         $sheet->rows($cellData);
     });
})->store('xls')->export('xls');
```

文件默认保存到 storage/exports 目录下，如果出现文件名中文乱码，将上述代码文件名做如下修改即可：

```
iconv('UTF-8', 'GBK', '召回用户黑名单')
```

# 4、导入Excel文件
我们将刚才保存到服务器上的Excel文件导入进来，导入很简单，使用 Excel 门面上的 load 方法即可：

```
Excel文件导入功能  
public function import(){
    $filePath = 'storage/exports/'.iconv('UTF-8', 'GBK', '召回用户黑名单').'.xls';
    Excel::load($filePath, function($reader) {
        $data = $reader->all();
        dd($data);
    });
}
```

load 方法基于项目根路径作为根目录，同样我们对中文进行了转码，否则会提示文件不存在。
在浏览器中访问 http://localhost:8000/excel/import ，页面显示如下：


当然，Laravel Excel还有很多其它功能，比如将Blade视图导出为Excel或CSV，以及对导入/导出更加细粒度的控制，具体可参考其官方文档： http://www.maatwebsite.nl/laravel-excel/docs

