## Binary Tree Maximum Path Sum

> Given a **non-empty** binary tree, find the maximum path sum.
>
> For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain **at least one node** and does not need to go through the root.
>
> **Example 1:**
>
> ```
> Input: [1,2,3]
> 
>        1
>       / \
>      2   3
> 
> Output: 6
> ```
>
> **Example 2:**
>
> ```
> Input: [-10,9,20,null,null,15,7]
> 
>    -10
>    / \
>   9  20
>     /  \
>    15   7
> 
> Output: 42
> ```

## Solution

* 递归函数的定义，经过该节点的路径最大值，
  * 在函数的返回值，是经过该节点的一条路径的最大值，即是左路径+根节点 ，右路径+根节点，其中左路径和右路径可以是0，
  * 递归过程中更新res的值，res是可以往左右延申的

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int res=Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        if(root==null) return 0;
        getPathSum(root);
        if(res==Integer.MIN_VALUE) return 0;
        return res;
    }
    private int getPathSum(TreeNode node){
        if(node==null) return 0;
       // if(node.left==null&&node.right==null) return node.val;
        int left=Math.max(0,getPathSum(node.left));
        int right=Math.max(0,getPathSum(node.right));
        res=Math.max(left+right+node.val,res);
        return Math.max(left,right)+node.val;
    }
}
```

