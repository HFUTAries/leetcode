# 581. Shortest Unsorted Continuous Subarray

## 题目链接：

https://leetcode.com/problems/shortest-unsorted-continuous-subarray/

## 问题描述：

Given an integer array, you need to find one continuous subarray that if you only sort this subarray in ascending order, then the whole array will be sorted in ascending order, too.  
  
You need to find the shortest such subarray and output its length.  
  
&emsp;&emsp;Example 1:  
&emsp;&emsp;&emsp;&emsp;Input: [2, 6, 4, 8, 10, 9, 15]  
&emsp;&emsp;&emsp;&emsp;Output: 5  
&emsp;&emsp;Explanation: You need to sort [6, 4, 8, 10, 9] in ascending order to make the whole array sorted in ascending order.  

Note:  
&emsp;&emsp;Then length of the input array is in range [1, 10,000].  
&emsp;&emsp;The input array may contain duplicates, so ascending order here means <=.    
  
## 解题思路：
    排序得到新数组arry[]，然后查找nums和array两端第一个不同的元素
    根据元素下标计算子数组长度


## 相关代码：

解题思路  
```c
void quickSort(int* a, int left, int right) {
	if (left >= right) {
		return;
	}
	int i = left;
	int j = right;
	int key = a[left];

	while (i < j) {
		while (i < j && key <= a[j]) {
			j--;
		}

		a[i] = a[j];

		while (i < j && key >= a[i]) {
			i++;
		}

		a[j] = a[i];
	}

	a[i] = key;
	quickSort(a, left, i - 1);
	quickSort(a, i + 1, right);

}

int findUnsortedSubarray(int* nums, int numsSize) {
	if(NULL == nums ||numsSize <= 1)
	{
		return 0;
	}

	int start = -1, end = -1;

	int* arr = malloc(sizeof(int) * numsSize);
	memcpy(arr, nums, sizeof(int) * numsSize);
	quickSort(arr, 0, numsSize - 1);

	for (int i = 0; i < numsSize; i++) {
		if(start == -1) {
			if (arr[i] != nums[i]) {
				start = i;
			}
		}

		if (end == -1) {
			if (arr[numsSize - 1 - i] != nums[numsSize - 1 - i]) {
				end = numsSize - 1 - i;
			}
		}

		if (start != -1 && end != -1)
			break;
	}

	if (start == -1 && end == -1) return 0;
	return end - start + 1;
}
```

## 运行结果
略
