# 950. Reveal Cards In Increasing Order

## 题目链接 

> https://leetcode.com/problems/reveal-cards-in-increasing-order/

## 问题描述：

In a deck of cards, every card has a unique integer.  You can order the deck in any order you want.

Initially, all the cards start face down (unrevealed) in one deck.

Now, you do the following steps repeatedly, until all cards are revealed:

Take the top card of the deck, reveal it, and take it out of the deck.
If there are still cards in the deck, put the next top card of the deck at the bottom of the deck.
If there are still unrevealed cards, go back to step 1.  Otherwise, stop.
Return an ordering of the deck that would reveal the cards in increasing order.

The first entry in the answer is considered to be the top of the deck.

Example 1:

    Input: [17,13,11,2,3,5,7]
    Output: [2,13,3,11,5,17,7]
Explanation:  
    We get the deck in the order [17,13,11,2,3,5,7] (this order doesn't matter), and reorder it.
    
    After reordering, the deck starts as [2,13,3,11,5,17,7], where 2 is the top of the deck.
    
    We reveal 2, and move 13 to the bottom.  The deck is now [3,11,5,17,7,13].
    
    We reveal 3, and move 11 to the bottom.  The deck is now [5,17,7,13,11].
    
    We reveal 5, and move 17 to the bottom.  The deck is now [7,13,11,17].
    
    We reveal 7, and move 13 to the bottom.  The deck is now [11,17,13].
    
    We reveal 11, and move 17 to the bottom.  The deck is now [13,17].
    
    We reveal 13, and move 17 to the bottom.  The deck is now [17].
    
    We reveal 17.
Since all the cards revealed are in increasing order, the answer is correct.

## 解题思路：

    如果要保证cards能够按照上述方法翻转后是递增的顺序，我们就要按照反过来的步骤放cards  
    也就是  
    放置17  
    后移(等效于反转[17])17，后移[17]，放置13在前面，得到[13,17]  
    反转[13,17]， 后移[17,13]，放置11在前面，得到[11,17,13]  
    反转[11,17,13]，后移[13,11,17]，放置7在前面，得到[7,13,11,17]  
    反转[7,13,11,17]，后移[17,7,13,11]，放置5在前面，得到[5,17,7,13,11]  
    反转[5,17,7,13,11]，后移[11,5,17,7,13]，放置3，得到[3,11,5,17,7,13]  
    反转[3,11,5,17,7,13]，后移[13,3,11,5,17,7]，放置2，得到[2,13,3,11,5,17,7]  
    
    也就是说我们按照上面的步骤放置cards，就能保证题目的步骤执行后是递增的顺序  
    
    同样我们发现，放置cards的顺序就是按照排序后从大到小放置的，也就是17,13,11,7,5,3,2  
    
    所以我们的解题步骤是：  
    1. 排序得到17,13,11,7,5,3,2；  
    2. 放置最大数到新数组arr的第一个位置；  
    3. 反转arr的已有元素，然后后移一位；  
    4. 放置剩余元素的最大数到新数组arr的第一个位置；  
    5. 重复3,4步，直到所有元素全部放入新数组arr；  
    
    反转说明：[7,13,11,17]经过反转后，是[17,7,13,11]。  
    也就是说，反转是将前面的元素向后移动一位，然后将最后一个数放到第一位。  
        
    
## 相关代码：

代码语言：C  
  
```c
inline void swap(int *x, int *y) {
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

void revealArr(int* arr, int deckSize, int nums) {
	if (NULL == arr || deckSize <= 0 || nums <= 0) {
		return;
	}

	int tmp = arr[nums - 1];

	for (int i = 0; i < nums - 1; i++) {
		arr[nums - i - 1] = arr[nums - i - 2];
	}
	arr[0] = tmp;

	for (int i = 0; i < nums; i++) {
		arr[nums - i] = arr[nums - i - 1];
	}

	arr[0] = 0;
	return;
}

/**
* Note: The returned array must be malloced, assume caller calls free().
*/
int* deckRevealedIncreasing(int* deck, int deckSize, int* returnSize) {
	if (NULL == deck || NULL == returnSize || deckSize < 0) {
		*returnSize = 0;
		return NULL;
	}

	quick_sort_recursive(deck, 0, deckSize - 1);

	int* arr = (int*)malloc(sizeof(int) * deckSize);
	*returnSize = deckSize;
	int tmp = 0;

	// Reveal the cards
	for (int i = 0; i < deckSize; i++) {
		revealArr(arr, deckSize, i);
		arr[0] = deck[deckSize - i - 1];
		tmp = 0;
	}

	return arr;
}
```
## 运行结果
略
