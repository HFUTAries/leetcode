# 53. Maximum Subarray

## 题目链接：

https://leetcode.com/problems/maximum-subarray/

## 问题描述：

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

Example:  
    Input: [-2,1,-3,4,-1,2,1,-5,4],  
    Output: 6  
Explanation: [4,-1,2,1] has the largest sum = 6.  
Follow up:  

If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.
  
## 解题思路：

    计算最大和子串，实际上就是将数组元素相加的过程  
    首先将数组处理下，相邻的正数，或者相邻的负数加到一起。  
    得到的数组肯定是    ...... 正数   负数   正数   负数......  
    比如[-2,1,-3,4,-1,2,1,-5,4]，实际上可以看成是[-2,1,-3,4,-1,3,-5,4]  
    
    如果一个正数，加上相邻的负数大于0，且负数加上后面的正数也大于0  
    那么这相邻的三个数加一起肯定比任何一个都大，比如 6 -2 4  
    
    如果一个正数，加上相邻的负数小于等于0，那么最大和肯定是，负数之前的最大和，与负数之后的最大和其中之一
    这时候重新计算负数后面的最大和即可。
    
    所以按照上面的步骤，我们可以简化一点
    我们可以通过如下方式遍历数组  
    
    max赋值为数组第一个元素；  
    从前往后加数组元素，并赋值给sum；  
    如果sum < 0,则说明一个正数加上负数时，只会变小，那么最大和子串只可能是这个负数之前的最大和，或者这个元素后面的最大和；
        此时，因为max已经记录了负数之前的最大和，只需要计算这个元素后面的最大和，然后和max对比即可
    如果sum > 0,则说明还可以向后相加，此时如果sum > max,那么说明出现新的最大值，则继续向后遍历数组；  
    
## 相关代码：

```c
int maxSubArray(int* nums, int numsSize) {
	if (NULL == nums || numsSize <= 0) {
		return 0;
	}

	int max = nums[0];
	int tmpSum = 0;

	for (int i = 0; i < numsSize; i++) {
		tmpSum += nums[i];

		if (tmpSum <= 0) {
			// 小于0，则也有可能数组都是负数，还需要找出最大的负数值
			// 比如[-2, -1, -3, -6]，最大和是-1，需要考虑
			max = tmpSum > max ? tmpSum : max;
			tmpSum = 0;
			continue;
		}
		max = tmpSum > max ? tmpSum : max;
	}

	return max;
}
```

## 运行结果
略
