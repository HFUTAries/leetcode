# 617. 合并二叉树

# 题目链接：

https://leetcode-cn.com/problems/merge-two-binary-trees/

## 问题描述：

给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。

你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为
NULL 的节点将直接作为新二叉树的节点。

示例 1:

输入:

Tree 1 Tree 2

1 2

/ \\ / \\

3 2 1 3

/ \\ \\

5 4 7

输出:

合并后的树:

3

/ \\

4 5

/ \\ \\

5 4 7

注意: 合并必须从两个树的根节点开始。

# 解题思路：

递归调用mergeTrees函数，合并两个二叉树节点，

# 相关代码：
```c
struct TreeNode\* mergeTrees(struct TreeNode\* t1, struct TreeNode\* t2) {

if (NULL == t1 && NULL == t2)

{

return NULL;

}

struct TreeNode \*result;

result = (struct TreeNode \*)malloc(sizeof(struct TreeNode));

if (NULL == t1)

{

result-\>val = t2-\>val;

result-\>left = t2-\>left;

result-\>right = t2-\>right;

return result;

}

if (NULL == t2)

{

result-\>val = t1-\>val;

result-\>left = t1-\>left;

result-\>right = t1-\>right;

return result;

}

result-\>val = t1-\>val + t2-\>val;

result-\>left = mergeTrees(t1-\>left, t2-\>left);

result-\>right = mergeTrees(t1-\>right, t2-\>right);

return result;

}
```c
