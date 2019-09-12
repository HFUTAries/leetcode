# 238. Product of Array Except Self

## 题目链接：

https://leetcode.com/problems/product-of-array-except-self/

## 问题描述：

Given an array nums of n integers where n > 1,

return an array output such that output[i] is equal to the product of all the elements of nums except nums[i].

Example:  

    Input:  [1,2,3,4]  
    Output: [24,12,8,6]  
Note: Please solve it without division and in O(n).  
  
Follow up:  
Could you solve it with constant space complexity?   
(The output array does not count as extra space for the purpose of space complexity analysis.)  
  
## 解题思路：
    用一个变量product记录数组元素的乘积，然后输出的第i个元素
    output[i] = product / nums[i];
    
    但是如果nums[i] = 0,会出现除以0的情况，而且题目要求不能用除法；

### 解题思路1：

    既然不能用除法，当计算output[i]时，必须计算nums[0]*...*nums[i - 1],和nums[i + 1]*...*nums[n - 1]
    因此我们可以通过分配两个数组pre[]和post[]:
        pre[0] = 1;
        post[n - 1] = 1;
        遍历数组nums[],然后
        数组pre[i] = nums[0]*...*nums[i - 1] = pre[i - 1] * nums[i - 1];
        数组post[i] = nums[i + 1]*...*nums[n - 1] = post[i + 1] * nums[i + 1];
        
    则output[i] = pre[i] * post[i];
    时间复杂度O(n)，空间复杂度O(n)；
    
### 解题思路2：
    题目扩展说明不开辟额外的空间，且O(n)时间复杂度
    
    那么解题思路1中，pre[]和post[]的空间分配能否省略呢？
    
    其实是可以的
    首先我们不开辟pre[],
    执行output[0] = 1;
    然后遍历数组nums[]
        将nums[0]*...*nums[i - 1]的结果存放在output[i]中,节省pre的空间分配
    遍历结束后，output存放的就是思路1中pre的结果
        output[i] = nums[0]*...*nums[i - 1] = output[i - 1] * nums[i - 1]
    
    接着使用临时变量tmp，tmp = 1
    tmp负责记录从后往前的乘积但是不保存到数组中，节省post的空间分配
    
    然后从后往前遍历数组
        此时output[n - 1] == nums[0]*...*nums[n - 2];
        执行output[n - 1] = output[n - 1] * tmp;
        执行tmp = tmp * nums[n];
        然后数组向前移动一位
        此时output[n - 1] = nums[0]*...*nums[n - 2];
        执行output[n - 1] = output[n - 1] * tmp;
        执行tmp = tmp * nums[n];
        然后，数组继续向前遍历
    当执行到数组第一个元素时，tmp = nums[n - 1]*nums[n - 2]...*nums[2] * nums[1]
        output[0] = output[0] * tmp;
    时间复杂度O(n)，空间复杂度O(1)；

## 相关代码：

解题思路2  
```c
/**
* Note: The returned array must be malloced, assume caller calls free().
*/
int* productExceptSelf(int* nums, int numsSize, int* returnSize) {
	if (NULL == nums || numsSize < 2 || NULL == returnSize) {
		*returnSize = 0;
		return NULL;
	}

	*returnSize = numsSize;
	int* arr = (int*)malloc(sizeof(int) * numsSize);
	arr[0] = 1;

	for (int i = 1; i < numsSize; i++) {
		arr[i] = arr[i - 1] * nums[i - 1];
	}

	int tmp = 1;

	for (int j = numsSize - 1; j > 0; j--) {
		arr[j] = arr[j] * tmp;
		tmp = tmp * nums[j];
	}

	arr[0] = tmp;

	return arr;
}
```

## 运行结果
略
