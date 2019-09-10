# 442. Find All Duplicates in an Array

## 题目链接：

https://leetcode.com/problems/find-all-duplicates-in-an-array/

## 问题描述：

Given an array of integers, 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

Find all the elements that appear twice in this array.

Could you do it without extra space and in O(n) runtime?

Example:
Input:
[4,3,2,7,8,2,3,1]

Output:
[2,3]
  
## 解题思路：

### 解题思路1：
    1. 创建n长度的数组indexArr，初始化元素值为0。
    2. 遍历数组nums；
    3. indexArr[nums[i] - 1]++;
    4. 遍历数组indexArr，对应元素值为2的，即为重复元素；
    时间复杂度O(n)，空间复杂度O(n)；
    不符合题目要求
    
### 解题思路2：
    既然要求不能另外开辟空间，那么是否可以在原数组上进行处理？
    这样不需要开辟空间，可以达到节省空间的目的。
    
    既然长度为n的数组，每个元素值都小于n,那么是否可以将只出现一次的元素放到对应的位置？
    比如值为K的元素，放到数组第K个位置？
    
    但是如果值为K的元素，第K个位置刚好是K怎么办？
    这说明K即为重复元素，此时，不在将重复的K放过去，而是继续遍历数组，
    查找下一个元素，将其放到对应的位置，遇到重复元素停止即可；
    
    {4,3,2,7,8,2,3,1};    将4放到第4个位置
    {7,3,2,4,8,2,3,1};    将7放到第7个位置
    {3,3,2,4,8,2,7,1};    将3放到第3个位置
    {2,3,3,4,8,2,7,1};    将2放到第2个位置
    {3,2,3,4,8,2,7,1};    将3放到第3个位置，但是第3个位置已经是3，说明出现3两次，不在使用第一个元素
    {3,2,3,4,8,2,7,1};    遍历数组，2,3,4刚好对应位置
    
    {3,2,3,4,8,2,7,1};    将8放到第8个位置
    {3,2,3,4,1,2,7,8};    将1放到第1个位置
    {1,2,3,4,3,2,7,8};    将3放到第3个位置，但是第3个位置已经是3，说明出现3两次
    {1,2,3,4,3,2,7,8};    将2放到第2个位置，但是第2个位置已经是2，继续遍历数组
    
    得到的数组，重新遍历，然后对应位置不是对应值的就是重复值

    时间复杂度O(n)，空间复杂度O(1)；
    
### 注意事项：
    1. 关于时间复杂度
    我们可以看到，解法2的例子中实际上遍历时间已经超过8次，为什么时间复杂度还是O(n)？
    实际上，假设有K对重复出现的数字，则整体所需的时间则为 n+k次
    只出现1次的元素，只会被调整位置不超过1次，如果该元素刚好在自己的位置，则不需要调整；
        时间：n-k
    出现2次的元素，被调整位置不超过2次，分别为遍历到第一次时放到对应位置，以及第二个重复数字占用了别的数字位置被调整；
        时间：2k
    重复出现的数字可能被调整很多次，重复出现的数字调整后可能还是占用了别的位置，但是并不影响我们的结论
    
    所以整体时间复杂度不超过2n
    
    2. 关于传参
    如果传递一个空数组或者非法参数，应将int* returnSize置为0，并返回NULL
    
    3. 关于计算出的结果
    实际上，调整后的数组，对应位置不是对应值，也能知道哪些数字没有出现
    比如{1,2,3,4,3,2,7,8};
    第五个位置是3，不仅说明3出现多次，也说明5没有出现过

## 相关代码：

```c
/**
* Note: The returned array must be malloced, assume caller calls free().
*/
int* findDuplicates(int* nums, int numsSize, int* returnSize) {
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
			arrTwice[index] = nums[k];
			index++;
		}
        
        if(index >= *returnSize)
            break;
	}

	return arrTwice;
}
```

## 运行结果
略
