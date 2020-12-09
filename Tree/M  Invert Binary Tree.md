##  Invert Binary Tree

> Invert a binary tree.
>
> **Example:**
>
> Input:
>
> ```
>      4
>    /   \
>   2     7
>  / \   / \
> 1   3 6   9
> ```
>
> Output:
>
> ```
>      4
>    /   \
>   7     2
>  / \   / \
> 9   6 3   1
> ```
>
> **Trivia:**
> This problem was inspired by [this original tweet](https://twitter.com/mxcl/status/608682016205344768) by [Max Howell](https://twitter.com/mxcl):
>
> > Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so f*** off.



## Solution Recursive

```java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root==null) return null;
        return dfs(root);
    }
    private TreeNode dfs(TreeNode root){
        if(root==null) return null;
        //we reserve the left node before change 
        TreeNode left=root.left;
        root.left=dfs(root.right);
        root.right=dfs(left);
        return root;
    }
}
```

## Solution Deque

```java
public class Solution {
    public TreeNode invertTree(TreeNode root) {
        
        if (root == null) {
            return null;
        }

        final Deque<TreeNode> stack = new LinkedList<>();
        stack.push(root);
        
        while(!stack.isEmpty()) {
            final TreeNode node = stack.pop();
            //we bfs the node and poll one 
            //change the one left and right 
            final TreeNode left = node.left;
            node.left = node.right;
            node.right = left;
            
            if(node.left != null) {
                stack.push(node.left);
            }
            if(node.right != null) {
                stack.push(node.right);
            }
        }
        return root;
    }
}
```

