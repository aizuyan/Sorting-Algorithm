#导航
- [算法]
    	-[交换排序](#交换排序)
		- [快速排序](#快速排序)
			- [快速排序介绍](#快速排序)
			- [分治法介绍](#分治法)
			- [划分算法介绍](#划分方法)
			- [代码示例](#代码示例_快速排序)
				- [php版](#php_快速排序)
				- [javascript版](#javascript_快速排序)
    	-[插入排序](#插入排序)
		- [插入排序](#直接插入排序)
			- [插入排序介绍](#插入排序介绍)
			- [in-place算法](#in-place_algorithm)
			- [代码示例](#代码示例_插入排序)
				- [php版](#php_插入排序)
				- [javascript版](#javascript_插入排序)


## 快速排序
快速排序是一种不稳定的排序算法，时间复杂度一般为O(nlogn)
一般使用了分治法+挖坑填数法(有时候划分不一定适用挖坑填数)

###分治法
[分治法](http://baike.baidu.com/link?url=0KLfXSDK6Nb4M3HKoVW0MIayqhoGzQ5-Bc8oVOEC7dUvp-BXfXf6WVZ6lYDnVItthTqJVbkeFoVwS19-08eixK)：
	分--将问题分解为规模更小的子问题；
	治--将这些更小的子问题逐个击破；
	合--将已解决的子问题合并，最终得出"母"问题解。

###划分方法
划分算法比较常用的是挖坑填数法，节省空间，不用开辟专门的空间去保存额外的数组
快速排序中挖坑填数法的伪算法：
``` bash
BEGIN
	i=0,j=N-1;  //i,j为数组下标,N为数组长度
	X=arr[i];
	while(i<j)
		while(i<j && arr[j]>=X)
			j--;
		if(i<j)
			arr[i] = arr[j];
			while(i<j && arr[i]<=X) //从前向后找到一个比X大的数来填后面的坑
				i++;
			if(i<j)
				arr[j] = arr[i];
	end while;
	if(i!=0)
		arr[i] = X;
END
```

###代码示例_快速排序

####php_快速排序
```php
function qSort(&$arr, $i=null, $j=null)
{
	if(!is_array($arr) || count($arr)<=1) return; //不合法，或者不需要排序
	if($i===null && $j === null)
	{
		$i = 0;
		$j = count($arr)-1;
	}
	if($i>=$j) return; //该部分已经排序完毕
	$begin 	= $i;
	$end 	= $j;
	$x 		= $arr[$i];
	while($i<$j)
	{
		while($i<$j && $arr[$j]>=$x) $j--; //从后往前找一个小于$x的值
		if($i<$j)
		{
			$arr[$i] = $arr[$j]; //后面挖坑填前面
			while($i<$j && $arr[$i]<=$x) $i++;  //从前往后找一个大于$x的值
			if($i<$j) $arr[$j] = $arr[$i]; //前面挖坑补后面
		}
	}
	if($i != $begin) $arr[$i] = $x;  //将临时变量中的值填入该趟快速排序的最后一个坑
	qSort($arr, $begin, $i-1);
	qSort($arr, $i+1, $end);
}

$arr = array(89,7,5,2,1,2,9);
qSort($arr);
print_r($arr);

//还有一种是比较简单的没有用到挖坑填数法，直接将大于和小于的数分别放在不同的数组中
```
结果：
```javascript
Array
(
    [0] => 1
    [1] => 2
    [2] => 2
    [3] => 5
    [4] => 7
    [5] => 9
    [6] => 89
)
```

####javascript_快速排序
```javascript
Array.prototype.qSort=function(i,j){
	if(this.length <= 1) return; //只剩一个元素，该子项排序ok
	if(typeof(i)=="undefined" && typeof(j)=="undefined"){ //未设置下标的开始和结尾，初始赋值
		i = 0;
		j = this.length-1;
	}
	i 	= parseInt(i);
	j	= parseInt(j);
	if(i >= j) return;
	var begin 	= i;
	var end 	= j;
	var x = this[i];
	while(i<j){
		while(i<j && this[j]>=x) j--; //从后往前寻找小于x的值
		if(i<j){
			this[i] = this[j]; //从后面挖数前面的坑
			i++; //前面的下表后移动
			while(i<j && this[i]<=x) i++; //从前往后寻找大于x的值
			if(i<j) this[j] = this[i]; //从前面挖数天后面的值
		}
	}
	if(i != begin) this[i] = x;
	this.qSort(begin, i-1);
	this.qSort(i+1, end);
}

var arr = [5,14,5,6,7,7,2,1];
arr.qSort();
```
输出：
```javascript
[1, 2, 5, 5, 6, 7, 7, 14]
```

##插入排序

###插入排序介绍
插入排序是一种稳定的排序算法，平均时间复杂度为O(n2)
插入排序算法的工作原理是通过构建有序数列，对于未排序的数据，在已排序的数列中从后向前扫描，找到相应位置并插入，这里可以看看[维基百科——插入排序](#http://zh.wikipedia.org/wiki/%E6%8F%92%E5%85%A5%E6%8E%92%E5%BA%8F)。插入排序的实现上，通常采用[in-place算法](http://en.wikipedia.org/wiki/In-place_algorithm)(及使用很少的常数级别的额外空间的排序)，因而排序从后向前的扫描过程中，需要反复的弄懂已排好序的元素，为最新的元素提供插入空间

###in-place_algorithm
在计算机科学中，in-place算法指的是通过一个很小的,常数级别的,额外空间的数据结构去改变输入数据，输入数据通常被程序执行的输出数据重写。详细内容可以看看[维基百科——in-place算法](#http://en.wikipedia.org/wiki/In-place_algorithm)

###代码示例_插入排序

####php_插入排序
```php
function insertSort(&$arr){
	if(!is_array($arr) || ($len = count($arr))<=1)
		return (array)$arr;
	for($i=1; $i<$len; $i++)
		for($j=$i; $j>0; $j--)
			if($arr[$j] < $arr[$j-1]){
				$tmp 		= $arr[$j];
				$arr[$j] 	= $arr[$j-1];
				$arr[$j-1] 	= $tmp;
			}
}

$arr = array(89,7,5,2,1,2,9);
print_r(insertSort($arr));
```
输出
```javascript
Array
(
    [0] => 1
    [1] => 2
    [2] => 2
    [3] => 5
    [4] => 7
    [5] => 9
    [6] => 89
)
```

####javascript_插入排序
```javascript
Array.prototype.insertSort=function(){
	var len,i,j,tmp;
	if((len=this.length) <= 1) return;
	for(i=1; i<len; i++)
		for(j=i; j>0; j--)
			if(this[j]<this[j-1]){
				tmp = this[j];
				this[j] = this[j-1];
				this[j-1] = tmp;
			}
}

var arr = [89,7,5,2,1,2,9];
console.log(arr);
arr.insertSort();
console.log(arr);
```
输出
```javascript
[ 89, 7, 5, 2, 1, 2, 9 ]
[ 1, 2, 2, 5, 7, 9, 89 ]
```
