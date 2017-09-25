---
title: 排序法之最快方法
date: 2016-10-06 00:59:21
tags:
    -- 排序
categories: php奇淫技巧
---
按照惯例我感觉我还的介绍下冒泡排序(这里介绍来子网络)。  
冒泡排序（Bubble Sort，台湾译为：泡沫排序或气泡排序）是一  种简单的排序算法。它重复地走访过要排序的数列，依次比较两个元素，如果他们的顺序错误就把他们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端。
![](http://opxoapzhs.bkt.clouddn.com//sort/BubbleSort.gif)
<!--more-->

## 思路
比较相邻的元素。如果第一个比第二个大，就交换他们两个。对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数。针对所有的元素重复以上的步骤，除了最后一个。持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

## 呆萌--demo：
```
$arrs = array(1,43,2,3,4,5,6);   // 6+5+4+3+2+1  循环次数=(数组总数-1)的累加和
function BubbleSort ($arr)
{
	$two = 0;
	$len = count($arr);
	//该层循环控制 需要冒泡的轮数
	for ($i=1; $i<$len; $i++) {
		//该层循环用来控制每轮 冒出一个数 需要比较的次数
		for ($k=0; $k<$len-$i; $k++) {
			$two++;
			if($arr[$k] > $arr[$k+1]) {
				$tmp = $arr[$k+1]; //声明一个临时变量暂时存放
				$arr[$k+1] = $arr[$k];
				$arr[$k] = $tmp;
			}
		}
	}
	echo $two.'==two<br>';
	return $arr;
}
var_dump(BubbleSort($arrs));
```
## 测试欢迎投稿
下面是笔者做的一个测试的一个循环次数，如果你有比笔者更快方法请发送到 [http://www.llaravel.com](http://www.llaravel.com) 进行留言
![](http://opxoapzhs.bkt.clouddn.com//sort/BubbleSort.png)



