# 4. Median of Two Sorted Arrays

题目链接 

https://leetcode.com/problems/median-of-two-sorted-arrays/


问题描述：
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

解题思路：

相关代码：

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
