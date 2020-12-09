## Count Complete Tree Nodes

> Given a **complete** binary tree, count the number of nodes.
>
> **Note:**
>
> **Definition of a complete binary tree from [Wikipedia](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees):**
> In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.
>
> **Example:**
>
> ```
> Input: 
>     1
>    / \
>   2   3
>  / \  /
> 4  5 6
> 
> Output: 6
> ```

## Solution  Intuition

```O(H)  O(N)```

```java
class Solution {
    public int countNodes(TreeNode root) {
        
        if(root==null) return  0;
        int maxdepth =1 ;
        TreeNode t =root;
        while(root.left !=null){
            root =root.left ;
            maxdepth++;
        }
        int count =getLastLevelNode(t,1,maxdepth);
        return count+(int)Math.pow(2,maxdepth-1)-1;
    }
    private int getLastLevelNode(TreeNode root,int depth ,int maxDepth){
        if(root!=null&&depth==maxDepth){
            return 1;
        }else if(root==null) return 0;
        int left = getLastLevelNode(root.left,depth+1,maxDepth);
        int right = getLastLevelNode(root.right,depth+1,maxDepth);
        return left+right;
    }
}
```

```O(N)```

```java
class Solution {
    public int countNodes(TreeNode root) {
             if(root==null) return 0;
        return countNodes(root.left)+countNodes(root.right)+1;
    }
}
```

## Solution 

```O(log N ^2)```

```java
class Solution {
    public int countNodes(TreeNode root) {
        int h = getHeight(root);
        if(h==0) return 0;
        if(getHeight(root.right)==h-1){
            return (1<<h-1)+countNodes(root.right);
        } else return (1<<h-2)+countNodes(root.left);
    }
    private int getHeight(TreeNode node){
        if(node==null) return 0;
        return 1 + getHeight(node.left);
    }
}
```

**Iteration**

```java
class Solution {
    int height(TreeNode root) {
        return root == null ? -1 : 1 + height(root.left);
    }
    public int countNodes(TreeNode root) {
        int nodes = 0, h = height(root);
        while (root != null) {
            if (height(root.right) == h - 1) {
                nodes += 1 << h;
                root = root.right;
            } else {
                nodes += 1 << h-1;
                root = root.left;
            }
            h--;
        }
        return nodes;
    }
}
```

## Solution Binary Search

```java
class Solution {
  // Return tree depth in O(d) time.
  public int computeDepth(TreeNode node) {
    int d = 0;
    while (node.left != null) {
      node = node.left;
      ++d;
    }
    return d;
  }

  // Last level nodes are enumerated from 0 to 2**d - 1 (left -> right).
  // Return True if last level node idx exists. 
  // Binary search with O(d) complexity.
  public boolean exists(int idx, int d, TreeNode node) {
    int left = 0, right = (int)Math.pow(2, d) - 1;
    int pivot;
    for(int i = 0; i < d; ++i) {
      pivot = left + (right - left) / 2;
      if (idx <= pivot) {
        node = node.left;
        right = pivot;
      }
      else {
        node = node.right;
        left = pivot + 1;
      }
    }
    return node != null;
  }

  public int countNodes(TreeNode root) {
    // if the tree is empty
    if (root == null) return 0;

    int d = computeDepth(root);
    // if the tree contains 1 node
    if (d == 0) return 1;

    // Last level nodes are enumerated from 0 to 2**d - 1 (left -> right).
    // Perform binary search to check how many nodes exist.
    int left = 1, right = (int)Math.pow(2, d) - 1;
    int pivot;
    while (left <= right) {
      pivot = left + (right - left) / 2;
      if (exists(pivot, d, root)) left = pivot + 1;
      else right = pivot - 1;
    }

    // The tree contains 2**d - 1 nodes on the first (d - 1) levels
    // and left nodes on the last level.
    return (int)Math.pow(2, d) - 1 + left;
  }
}

```

**mine** use two times Binary Search 

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int countNodes(TreeNode root) {
        if(root==null) return 0;
        int depth =getDepth(root);
        if(depth ==0) return 1;
        int left = 1; 
        int right = (1<<depth) ;
        while(left<=right){
            int mid =(right - left) /2 +left ;
            if(exist(mid,depth,root)) left =mid +1;
            //mid is not possible 
            else right = mid -1;
        }
        return (1<<depth)-1+left-1;
    }
    
    private boolean exist(int p ,int depth,TreeNode node){
        int left = 1;
        int right = 1<<depth;
        for(int i =0;i<depth ;i++){
            int mid =(right -left )/2 +left;
            if(p<=mid){
                node=node.left;
                right =mid;
            }else{
                node =node.right; 
                left =mid +1;
            }
        }
        return node!=null;
    }
    private int getDepth(TreeNode root){
        int depth =0;
        while(root.left!=null){
            depth++;
            root=root.left;
        }
        return depth;
    }
}
```

