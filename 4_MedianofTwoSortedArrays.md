# 4. Median of Two Sorted Arrays

## 题目链接 

> https://leetcode.com/problems/median-of-two-sorted-arrays/

## 问题描述：

	There are two sorted arrays nums1 and nums2 of size m and n respectively.
	Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).
	You may assume nums1 and nums2 cannot be both empty.

Example 1:
	nums1 = [1, 3]
	nums2 = [2]

	The median is 2.0
	
Example 2:
	nums1 = [1, 2]
	nums2 = [3, 4]

	The median is (2 + 3)/2 = 2.5

## 解题思路：

### 解题思路1：
    该题目为将两个有序数组的中间值找到并返回。
    最基本的做法是，通过排序算法将两个有序的数组重新排序，然后直接输出中间值
        该方法的空间复杂度为O(nums1Size + nums2Size)，时间复杂度由合并有序数组的排序算法决定

### 解题思路2：	
    合并有序数组，可以通过归并排序的方式，两个有序数组在排序的过程中，不停的将最小值存入新开辟的数组，排序结束后新数组的meida值即可算出；
    我们将上述的方法进行改进
    我们并不需要真的将数组的所有元素排序，也不需要将有序数组的最小值记入新数组，只需要通过临时变量index记录当前排序的下标是否已经到达media的位置即可
    
    Example 2:

	nums1 = [1, 2, 3, 4]
	nums2 = [5, 6, 7, 8]
    新数组nums[]
    meida = 4
    index = 0;
    1 < 5
    1存入数组nums[]   index++；// 并不把1存入数组
    2 < 5
    2存入数组nums[]   index++；// 并不把2存入数组
    3 < 5
    3存入数组nums[]   index++；// 并不把3存入数组
    4 < 5
    4存入数组nums[]   index++；// 并不把3存入数组。且index == media，表示排好顺序的元素已经到了media的位置
    此时数组nums[]已经保存有排好序的4个元素而第四个元素就是要用于计算media值的一个
    不再排序，找到下一个元素5，计算平均值
    
    虽然上述步骤和归并排序一样，但是因为不需要存储排序好的1,2,3,4，自然就节省了新数组的空间，而时间只需要(nums1Size + nums2Size)/2
    
    对应空间复杂度O(1), 时间复杂度 O(m+n).
    
### 解题思路3： 
    题目要求的时间复杂度是 O(log(m+n))，实际上，我们可以知道，上面的解题思路2可以进一步优化
    我们不需要从两个有序数组的开头部分找
    通过二分查找的方式，从两个数组的中间部分开始比较大小并记录index值，判断是否到了media位置
    
    所以时间复杂度上，能够优化到O(log(m+n))，空间复杂度O(1)

## 相关代码：

代码语言：C
实现内容：解题思路2
double findMedianSortedArrays(int* nums1, int nums1Size, int* nums2, int nums2Size){
	double result = 0.0;
	int indexNum1 = 0;
	int indexNum2 = 0;
	int medianIndex = 0;
	int mediaValue = 0;
	int nextValue = 0;
	int tmp = 0;

	if (0 != (nums1Size + nums2Size) % 2)
	{
		medianIndex = (nums1Size + nums2Size) / 2;
	}
	else
	{
		medianIndex = (nums1Size + nums2Size) / 2 - 1;
	}

	while (indexNum1 < nums1Size && indexNum2 < nums2Size)
	{
		if (nums1[indexNum1] <= nums2[indexNum2])
		{
			mediaValue = nums1[indexNum1];
			indexNum1++;
		}
		else
		{
			mediaValue = nums2[indexNum2];
			indexNum2++;
		}

		if (tmp == medianIndex)
		{
			result = mediaValue;
			if (0 == (nums1Size + nums2Size) % 2)
			{
				if (indexNum1 < nums1Size && indexNum2 < nums2Size)
				{
					nextValue = (nums1[indexNum1] <= nums2[indexNum2]) ? nums1[indexNum1] : nums2[indexNum2];
					result = (double)(mediaValue + nextValue) / 2;
				}
				else
				{
					nextValue = (indexNum1 >= nums1Size) ? nums2[indexNum2] : nums1[indexNum1];
					result = (double)(mediaValue + nextValue) / 2;
				}
			}
			return result;
		}
		tmp++;

	}

	if (indexNum1 >= nums1Size)
	{
		tmp = medianIndex - nums1Size;
		result = nums2[tmp];
		if (0 == (nums1Size + nums2Size) % 2)
		{
			result = (double)(result + nums2[tmp + 1]) / 2;
		}
		return result;
	}

	if (indexNum2 >= nums2Size)
	{
		tmp = medianIndex - nums2Size;
		result = nums1[tmp];
		if (0 == (nums1Size + nums2Size) % 2)
		{
			result = (double)(result + nums1[tmp + 1]) / 2;
		}
		return result;
	}

	return result;
}

## 运行结果
![image](https://github.com/HFUTAries/img-folder/blob/master/20190907143637.png)
