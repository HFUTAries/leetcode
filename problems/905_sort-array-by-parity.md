# 905. Sort Array By Parity

## 题目链接：

https://leetcode.com/problems/sort-array-by-parity/

## 问题描述：

Given an array A of non-negative integers, return an array consisting of all the even elements of A, followed by all the odd elements of A.  
You may return any answer array that satisfies this condition.  

Example 1:
    Input: [3,1,2,4]  
    Output: [2,4,3,1]  
    The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.  

Note:

    1 <= A.length <= 5000  
    0 <= A[i] <= 5000  

## 解题思路：

    遍历数组，如为偶数则数组从前向后放置元素；如果为奇数，则从后向前放置元素  

## 相关代码：

```c
/**
* Note: The returned array must be malloced, assume caller calls free().
*/
int* sortArrayByParity(int* A, int ASize, int* returnSize) {
	if (NULL == A || ASize <= 0) {
		*returnSize = ASize;
		return NULL;
	}

	int* arr = (int*)malloc(sizeof(int) * ASize);
	if (NULL == arr) {
		printf("malloc fail and return\n");
		*returnSize = 0;
		return NULL;
	}

	int evenIndex = 0;
	int oddIndex = ASize - 1;
	for (int i = 0; i < ASize; i++) {
		if (0 == A[i] % 2) {
			arr[evenIndex] = A[i];
			evenIndex++;
		}
		else {
			arr[oddIndex] = A[i];
			oddIndex--;
		}
	}

	*returnSize = ASize;
	return arr;
}
```

## 运行结果
略
