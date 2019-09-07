# 977. Squares of a Sorted Array

## 题目链接：

https://leetcode.com/problems/squares-of-a-sorted-array/

## 问题描述：

Given an array of integers A sorted in non-decreasing order, return an array of the squares of each number, also in sorted non-decreasing order.

Example 1:  
      Input: [-4,-1,0,3,10]  
      Output: [0,1,9,16,100]    

Example 2:  
    Input: [-7,-3,2,3,11]  
    Output: [4,9,9,49,121]  
 

Note:  
    1 <= A.length <= 10000  
    -10000 <= A[i] <= 10000  
    A is sorted in non-decreasing order.  

## 解题思路：

    创建数组分配空间，保存原数组的平方值，然后快速排序  

## 相关代码：

```c
void swap(int *x, int *y) {
	int t = *x;
	*x = *y;
	*y = t;
}

void quick_sort_recursive(int arr[], int start, int end) {
	if (start >= end)
		return;
	int mid = arr[end];
	int left = start, right = end - 1;
	while (left < right) {
		while (arr[left] < mid && left < right)
			left++;
		while (arr[right] >= mid && left < right)
			right--;
		swap(&arr[left], &arr[right]);
	}
	if (arr[left] >= arr[end])
		swap(&arr[left], &arr[end]);
	else
		left++;
	if (left)
		quick_sort_recursive(arr, start, left - 1);
	quick_sort_recursive(arr, left + 1, end);
}

/**
* Note: The returned array must be malloced, assume caller calls free().
*/
int* sortedSquares(int* A, int ASize, int* returnSize) {
	if (NULL == A || NULL == returnSize) {
		*returnSize = 0;
		return NULL;
	}

	int* arr = (int*)malloc(ASize * sizeof(int));

	for (int i = 0; i < ASize; i++) {
		arr[i] = A[i] * A[i];
	}

	quick_sort_recursive(arr, 0, ASize - 1);
	*returnSize = ASize;

	return arr;
}
```

## 运行结果
略
