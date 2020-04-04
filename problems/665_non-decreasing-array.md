# 665. Non-decreasing Array

## 题目链接：

https://leetcode.com/problems/non-decreasing-array/

## 问题描述：

Given an array nums with n integers, your task is to check if it could become non-decreasing by modifying at most 1 element.
We define an array is non-decreasing if nums[i] <= nums[i + 1] holds for every i (0-based) such that (0 <= i <= n - 2).

Example 1: 
    Input: nums = [4,2,3] 
    Output: true 
    Explanation: You could modify the first 4 to 1 to get a non-decreasing array. 
 
Example 2: 
    Input: nums = [4,2,1] 
    Output: false 
    Explanation: You can't get a non-decreasing array by modify at most one element. 
 
Constraints: 
    1 <= n <= 10 ^ 4 
    - 10 ^ 5 <= nums[i] <= 10 ^ 5 
  
## 解题思路：

    题目为判断数组是否可以只通过一次修改，将数组变成非递增序列
    
    比如[1,2,3,5,4,7,9]，我们遍历数组，发现元素4不符合要求
    可以直接将4变成5[1,2,3,5,5,7,9]
    或者直接将4变成7[1,2,3,5,7,7,9]
    都可以达到题目要求
    
    具体情况比上述稍微复杂一些
    我们初始化修改次数count = 0；
    遍历数组，当遍历到第i个元素时发现：nums[i] < nums[i - 1];
    
    我们在判断是修改当前数字nums[i]还是之前数字nums[i - 1]时，比较一下
    1. 如果当前数字nums[i] >= nums[i - 2]时，修改之前数字nums[i - 1]；
                   nums[i - 1] = nums[i]
       如果之前数字nums[i - 1]是第一个数组元素，修改之前数字nums[i - 1]；
                   nums[i - 1] = nums[i]
    
    但是如果当前数字nums[i] < nums[i - 2]时,只修改一次之前数字不行了，此时需要向后判断；
    2. 如果当前数字nums[i]是最后一个数组元素，修改当前数字nums[i]；
                   nums[i] = nums[i - 1]
       如果之后数字nums[i + 1] >= nums[i - 1],修改当前数字nums[i]；
                   nums[i] = nums[i - 1]
    
    以上情况只需要修改一次，使count = 1，然后继续遍历数组即可，后续如果继续含有变小元素，直接返回false；
    
    3.但是之后数字nums[i + 1] < nums[i - 1],只修改一次之前数字nums[i - 1]或者只修改当前数字nums[i]达不到要求
                   直接返回false
    
    
    
## 相关代码：

```c
bool checkPossibility(int* nums, int numsSize) {
	if (NULL == nums || numsSize <= 0)
	{
		return false;
	}

	int flag = 0;
	if (numsSize <= 2) return true;

	for (int i = 1; i < numsSize && flag < 2; i++)
	{
		if (nums[i] >= nums[i - 1])
		{
			continue;
		}

		if ( i < 2 || (i >= 2 && nums[i] >= nums[i - 2]) )
		{
			nums[i - 1] = nums[i];
			flag++;
			continue;
		}

		if (i >= numsSize - 1 || (i < numsSize - 1 && nums[i + 1] > nums[i - 1]) )
		{
			nums[i] = nums[i - 1];
			flag++;
			continue;
		}
		else
		{
			return false;
		}
	}

	return flag > 1 ? false : true;
}
```

## 运行结果
略
