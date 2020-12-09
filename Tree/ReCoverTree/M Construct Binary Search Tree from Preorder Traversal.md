## Construct Binary Search Tree  From PreOrder Traversal

> Return the root node of a binary **search** tree that matches the given `preorder` traversal.
>
> *(Recall that a binary search tree is a binary tree where for every node, any descendant of `node.left` has a value `<` `node.val`, and any descendant of `node.right` has a value `>` `node.val`. Also recall that a preorder traversal displays the value of the `node` first, then traverses `node.left`, then traverses `node.right`.)*
>
>  
>
> **Example 1:**
>
> ```
> Input: [8,5,1,7,10,12]
> Output: [8,5,10,1,7,null,12]
> ```
>
>  
>
> **Note:** 
>
> 1. `1 <= preorder.length <= 100`
> 2. The values of `preorder` are distinct.

**Intuition**

Find the left part and right part,
		then recursively construct the tree.

## Solution 利用前序和中序 来构建树

* 利用中序遍历和前序遍历计算，树的根节点，它的左子树和右子树有多少元素

```java
class Solution {
    public TreeNode bstFromPreorder(int[] preorder) {
        //BST preOrder => inOrder
        int len=preorder.length;
        int inorder[]=new int[preorder.length];
        for(int i=0;i<inorder.length;i++){ inorder[i]=preorder[i];}
        Arrays.sort(inorder);
        return buildTree(preorder,0,len-1,inorder,0,len-1);
    }
    
    private TreeNode buildTree(int [] preorder , int pl ,int pr ,int [] inorder,int il,int ir){
        if(pl>pr) return null;
        if(pl==pr) return new TreeNode(preorder[pl]);
        int root_val=preorder[pl];
        int index=0;
        for(;index<preorder.length;index++){
            if(inorder[index]==root_val) break;
        }
        TreeNode root=new TreeNode(root_val);
        root.left=buildTree(preorder,pl+1,pl+index-il,inorder,il,index-1);
        root.right=buildTree(preorder,pl+1+index-il,pr,inorder,index+1,ir);
        return root;
    }
}
```

* 优化版本
  * 因为它是二叉搜索树，可以利用它的左右子树和根节点的关系，碰到比它大的数，就是右子树的起点

```java
class Solution {
    public TreeNode bstFromPreorder(int[] preorder) {
        return build(preorder, 0, preorder.length);
    }
    
    private TreeNode build(int[] preorder, int l, int r) {
        if (l >= r) return null;
        TreeNode root = new TreeNode(preorder[l]);
        for (int m = l + 1; m <= r; m++) {
            if (m == r || preorder[m] > root.val) {
                //这里是左闭右开区间
                root.left = build(preorder, l + 1, m);
                root.right = build(preorder, m, r);
                break;
            }
        }
        return root;
    }
}
```

```java
class Solution {
    //int i = 0;
    public TreeNode bstFromPreorder(int[] A) {
        return convert(A, 0,A.length-1);
    }
    private TreeNode convert(int []A,int l,int r){
        if(l>r) return null;
        if(l==r) return new TreeNode(A[l]);
        TreeNode root=new TreeNode(A[l]);
        int i=0;
        //this is a close region
        for( i=l+1;i<=r;i++){
            if(A[i]>A[l]) break; 
        }
        root.left=convert(A,l+1,i-1);
        root.right=convert(A,i,r);
        return root;
    }
}
```



## Solution From  Lee

* the way  to solve this problem is similiar to **Valiate the  BST**

Give the function a bound the maximum number it will handle.
		The left recursion will take the elements smaller than `node.val`
		The right recursion will take the remaining elements smaller than `bound`

**Complexity**
		`				bstFromPreorder` is called exactly N times.
		**It's same as a preorder traversal.**
		Time: `O(N)`

```java
	class Solution {
    int i = 0;
    public TreeNode bstFromPreorder(int[] A) {
        return bstFromPreorder(A, Integer.MAX_VALUE);
    }

    public TreeNode bstFromPreorder(int[] A, int bound) {
        if (i == A.length || A[i] > bound) return null;
        TreeNode root = new TreeNode(A[i++]);
        root.left = bstFromPreorder(A, root.val);
        root.right = bstFromPreorder(A, bound);
        return root;
    }
}
```

