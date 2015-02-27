#导航
- [算法]
	- [快速排序](#快速排序)


## 快速排序
快速排序是一种不稳定的排序算法，时间复杂度一般为O(nlogn)
一般使用了分治法+挖坑填数法(有时候划分不一定适用挖坑填数)

### 分治法
[分治法](http://baike.baidu.com/link?url=0KLfXSDK6Nb4M3HKoVW0MIayqhoGzQ5-Bc8oVOEC7dUvp-BXfXf6WVZ6lYDnVItthTqJVbkeFoVwS19-08eixK)：
	分--将问题分解为规模更小的子问题；
	治--将这些更小的子问题逐个击破；
	合--将已解决的子问题合并，最终得出"母"问题解。

### 划分算法
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

###代码示例(默认都是升序)

php版：
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
