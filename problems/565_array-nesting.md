# 565. Array Nesting

## 题目链接：

https://leetcode.com/problems/array-nesting/

## 问题描述：

A zero-indexed array A of length N contains all integers from 0 to N-1. 
Find and return the longest length of set S, where S[i] = {A[i], A[A[i]], A[A[A[i]]], ... } subjected to the rule below.

Suppose the first element in S starts with the selection of element A[i] of index = i, the next element in S should be A[A[i]], and then A[A[A[i]]]… 
By that analogy, we stop adding right before a duplicate element occurs in S.

 

Example 1:

    Input: A = [5,4,0,3,1,6,2]  
    Output: 4  
  Explanation:   
    A[0] = 5, A[1] = 4, A[2] = 0, A[3] = 3, A[4] = 1, A[5] = 6, A[6] = 2.  

One of the longest S[K]:
    S[0] = {A[0], A[5], A[6], A[2]} = {5, 6, 2, 0}
 
Note:

N is an integer within the range [1, 20,000].
The elements of A are all distinct.
Each element of A is an integer within the range [0, N-1].

## 解题思路：

    遍历数组第i个元素；
    如果A[i] != i;则寻找A[A[i]]，且将A[i]置为-1；
    如果找到了A[...A[A[i]]] = i,则将长度和之前记录的最大值比较，然后取最大值；

## 相关代码：

```c
int arrayNesting(int* nums, int numsSize) {
	if (NULL == nums || numsSize <= 0) {
		return 0;
	}

	int max = 0;
	int tmp = 0;
	int len = 0;
	int index = 0;

	for (int i = 0; i < numsSize; i++) {
		if (-1 == nums[i]) {
			continue;
		}

		tmp = nums[i];
		len = 1;
		index = i;
		while (tmp != i) {
			tmp = nums[index];
			nums[index] = -1;
			index = tmp;
			tmp = nums[index];
			len++;
		}
		nums[index] = -1;
		max = max < len ? len : max;
	}

	return max;
}
```

## 运行结果
略
