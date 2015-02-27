#导航
-[算法]
	-[快速排序](#快速排序)


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


