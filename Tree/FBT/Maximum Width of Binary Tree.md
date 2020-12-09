## Maximum Width of Binary Tree

> Given a binary tree, write a function to get the maximum width of the given tree. The width of a tree is the maximum width among all levels. The binary tree has the same structure as a **full binary tree**, but some nodes are null.
>
> The width of one level is defined as the length between the end-nodes (the leftmost and right most non-null nodes in the level, where the `null` nodes between the end-nodes are also counted into the length calculation.
>
> **Example 1:**
>
> ```
> Input: 
> 
>            1
>          /   \
>         3     2
>        / \     \  
>       5   3     9 
> 
> Output: 4
> Explanation: The maximum width existing in the third level with the length 4 (5,3,null,9).
> ```
>
> **Example 2:**
>
> ```
> Input: 
> 
>           1
>          /  
>         3    
>        / \       
>       5   3     
> 
> Output: 2
> Explanation: The maximum width existing in the third level with the length 2 (5,3).
> ```
>
> **Example 3:**
>
> ```
> Input: 
> 
>           1
>          / \
>         3   2 
>        /        
>       5      
> 
> Output: 2
> Explanation: The maximum width existing in the second level with the length 2 (3,2).
> ```
>
> **Example 4:**
>
> ```
> Input: 
> 
>           1
>          / \
>         3   2
>        /     \  
>       5       9 
>      /         \
>     6           7
> Output: 8
> Explanation:The maximum width existing in the fourth level with the length 8 (6,null,null,null,null,null,null,7).
> ```
>
> **Note:** Answer will in the range of 32-bit signed integer.

* The idea is pointed in the context
* use the full binary Tree and Index

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
    //full binary Tree 
    public int widthOfBinaryTree(TreeNode root) {
        int max = 1 ;
        Deque<TreeNode> q = new LinkedList<>();
        Deque<Integer> index = new LinkedList<>();
        if(root==null) return 0 ;
        q.add(root);
        index.add(0);
        while(!q.isEmpty()){
            int size = q.size();
            max = Math.max(max,index.peekLast()-index.peekFirst()+1);
            while(size>0){
                size--;
                TreeNode t =q.pollFirst();
                int  n = index.pollFirst();
                if(t.left!=null){
                    q.add(t.left);
                    index.add(n*2);
                }
                if(t.right!=null){
                    q.add(t.right);
                    index.add(n*2+1);
                }
                
            }
        }
        return max;
    }
}
```

## Solution DFS

* we only store the first index of the every level ,the first index is the index we first meet 
* and ``` dfs ```calculate the possible index 

```java
class Solution {
    int max = 1; 
    public int widthOfBinaryTree(TreeNode root) {
        if(root==null) return 0;
        dfs(root,0,0,new ArrayList<>());
        return max ;
    }
    private void dfs(TreeNode  root,int index,int level,List<Integer> tmp){
        if(root==null) return ;
        if(tmp.size()==level){
            tmp.add(index);
        }
        max = Math.max(max,index-tmp.get(level)+1);
        dfs(root.left,index*2,level+1,tmp);
        dfs(root.right,index*2+1,level+1,tmp);
    }
}
```

