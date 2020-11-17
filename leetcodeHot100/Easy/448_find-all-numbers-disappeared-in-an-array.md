# 448. 找到所有数组中消失的数字

# 题目链接：

 <https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/>

## 问题描述：

给定一个范围在 1 ≤ a[i] ≤ n ( n = 数组大小 ) 的
整型数组，数组中的元素一些出现了两次，另一些只出现一次。

找到所有在 [1, n] 范围之间没有出现在数组中的数字。

您能在不使用额外空间且时间复杂度为O(n)的情况下完成这个任务吗?
你可以假定返回的数组不算在额外空间内。

示例:

输入:

[4,3,2,7,8,2,3,1]

输出:

[5,6]

# 解题思路：

略

# 相关代码：

```c
/**
* Note: The returned array must be malloced, assume caller calls free().
*/
int* findDisappearedNumbers(int* nums, int numsSize, int* returnSize) {

	int tmp = 0;
	*returnSize = 0;

	if (NULL == nums || numsSize <= 0 || NULL == returnSize) {
		return NULL;
	}

	for (int i = 0; i < numsSize; ) {
		if (nums[nums[i] - 1] != nums[i]) {
			tmp = nums[nums[i] - 1];
			nums[nums[i] - 1] = nums[i];
			nums[i] = tmp;
		}
		else {
			i++;
		}
		tmp = 0;
	}

	for (int j = 0; j < numsSize; j++) {
		if (j != nums[j] - 1) {
			*returnSize = *returnSize + 1;
		}
	}

	int* arrTwice = (int*)malloc(sizeof(int) * (*returnSize));
	int index = 0;

	for (int k = 0; k < numsSize; k++) {
		if (k != nums[k] - 1) {
			arrTwice[index] = k + 1;
			index++;
		}
	}

	return arrTwice;
}
```
