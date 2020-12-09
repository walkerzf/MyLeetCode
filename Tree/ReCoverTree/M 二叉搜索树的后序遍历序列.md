## [二叉搜索树的后序遍历序列](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

> 输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 true，否则返回 false。假设输入的数组的任意两个数字都互不相同。
>
>  
>
> ```
> 参考以下这颗二叉搜索树：
> 
>      5
>     / \
> 
>    2   6
>   / \
>  1   3
> 示例 1：
> 
> 输入: [1,6,3,2,5]
> 输出: false
> 示例 2：
> 
> 输入: [1,3,2,6,5]
> 输出: true
> 
> 
> 提示：
> 
> 数组长度 <= 1000
> ```

## Solution Recursion to valid the Tree

* actually we do not to   construct the inorder Traversal ,because the do not use the inorder information ,we can only use the 

```java
class Solution {
    public boolean verifyPostorder(int[] postorder) {
        int inorder[] = new int[postorder.length];
        for(int i =0;i<inorder.length;i++) inorder[i] = postorder[i];
        Arrays.sort(inorder);
        int len = inorder.length;
        //postorder left -right - root  
        //inorder left  root -right   sort sequence
        return check(postorder,inorder,0,len-1,0,len-1);
    }
    private boolean check(int []p,int []i,int pl,int pr,int il,int ir){
        if(pl>=pr) return true;
        int root = p[pr];
        int index = il;
        for(;index<=ir;index++){
            if(i[index]==root) break;
        }
        //we know the root  index
        int leftLen = index-il;
        int rightLen = ir-index;
        for(int ii = pl;ii<pl+leftLen;ii++){
            if(p[ii]>root) return false;
        }
        for(int ii =pl+leftLen;ii<pl+leftLen+rightLen;ii++){
            if(p[ii]<root) return false;
        }
        
        return check(p,i,pl,pl+leftLen-1,il,index-1)&&check(p,i,pl+leftLen,pr-1,index+1,ir);
        }
}
```



* slighter simple

```java
class Solution {
    public boolean verifyPostorder(int[] postorder) {
        return recur(postorder, 0, postorder.length - 1);
    }
    boolean recur(int[] postorder, int i, int j) {
        if(i >= j) return true;
        int p = i;
        while(postorder[p] < postorder[j]) p++;
        int m = p;
        while(postorder[p] > postorder[j]) p++;
        return p == j && recur(postorder, i, m - 1) && recur(postorder, m, j - 1);
    }
}

```

