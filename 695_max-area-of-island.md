# 695. Max Area of Island

## 题目链接：

https://leetcode.com/problems/max-area-of-island/

## 问题描述：

Given a non-empty 2D array grid of 0's and 1's, an island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Find the maximum area of an island in the given 2D array. (If there is no island, the maximum area is 0.)

Example 1:

    [[0,0,1,0,0,0,0,1,0,0,0,0,0],  
     [0,0,0,0,0,0,0,1,1,1,0,0,0],  
     [0,1,1,0,1,0,0,0,0,0,0,0,0],  
     [0,1,0,0,1,1,0,0,1,0,1,0,0],  
     [0,1,0,0,1,1,0,0,1,1,1,0,0],  
     [0,0,0,0,0,0,0,0,0,0,1,0,0],  
     [0,0,0,0,0,0,0,1,1,1,0,0,0],  
     [0,0,0,0,0,0,0,1,1,0,0,0,0]]  
Given the above grid, return 6. Note the answer is not 11, because the island must be connected 4-directionally.  

Example 2:

    [[0,0,0,0,0,0,0,0]]
Given the above grid, return 0.  
Note: The length of each dimension in the given grid does not exceed 50.  
  
## 解题思路：

    递归遍历数组元素
    如果元素为0，不处理；
    如果元素为1，将该值置为0，然后计算相邻不为0的island面积相加
    
## 相关代码：

```c
int countArea(int** grid, int gridSize, int* gridColSize, int row, int col) {
	if (NULL == grid || gridSize < 0 || NULL == gridColSize) {
		return 0;
	}

	if ((row >= 0) && (col >= 0) && (row < gridSize) && (col < *gridColSize) && (grid[row][col] == 1)) {
		grid[row][col] = 0;
		return 1 + countArea(grid, gridSize, gridColSize, row + 1, col) +
			countArea(grid, gridSize, gridColSize, row - 1, col) +
			countArea(grid, gridSize, gridColSize, row, col + 1) +
			countArea(grid, gridSize, gridColSize, row, col - 1);
	}

	return 0;
}

int maxAreaOfIsland(int** grid, int gridSize, int* gridColSize) {

	int max = 0;

	if (NULL == grid || gridSize < 0 || NULL == gridColSize) {
		return max;
	}

	int tmp = 0;
	for (int i = 0; i < gridSize; i++) {
		for (int j = 0; j < *gridColSize; j++) {
			if (1 == grid[i][j]) {
				tmp = countArea(grid, gridSize, gridColSize, i, j);
				max = max < tmp ? tmp : max;
			}
		}
	}

	return max;
}
```

## 运行结果
略
