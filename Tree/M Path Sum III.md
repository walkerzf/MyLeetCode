## Path Sum III

> You are given a binary tree in which each node contains an integer value.
>
> Find the number of paths that sum to a given value.
>
> The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).
>
> The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.
>
> **Example:**
>
> ```
> root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8
> 
>       10
>      /  \
>     5   -3
>    / \    \
>   3   2   11
>  / \   \
> 3  -2   1
> 
> Return 3. The paths that sum to 8 are:
> 
> 1.  5 -> 3
> 2.  5 -> 2 -> 1
> 3. -3 -> 11
> ```

## Solution Intuition

* we dfs the every Node ,the return value is a map whose key is sum the value is happen  times
* for everyNode ,we need to create a new HashMap , it costs ,or we can use the return map from the leaf Node 



* Better
  * for initialization  ,we record the (0,1) ,we backTrace the Tree,record the prefixSum .(Key:prefix Sum ; Value : how many ways to get the prefixSum) ,so whenever we reach a node ,we check if prefix Sum (1. target exists or not 2. we add the prefixSum to res),  ,whenever we reach the node second time which means we would leave the node and its subtree 
  * because the sum can only go downwards , so we use prefixSum ,single direction 

```java
    public int pathSum(TreeNode root, int sum) {
        HashMap<Integer, Integer> preSum = new HashMap();
        preSum.put(0,1);
        return helper(root, 0, sum, preSum);
    }
    
    public int helper(TreeNode root, int currSum, int target, HashMap<Integer, Integer> preSum) 		{
        if (root == null) {
            return 0;
        }
        
        currSum += root.val;
        int res = preSum.getOrDefault(currSum - target, 0);
        preSum.put(currSum, preSum.getOrDefault(currSum, 0) + 1);
        
        res += helper(root.left, currSum, target, preSum) + helper(root.right, currSum, target, preSum);
        preSum.put(currSum, preSum.get(currSum) - 1);
        return res;
    }
```



```java
class Solution {
    public int pathSum(TreeNode root, int sum) {
        Map<Integer,Integer> m = new HashMap<>();
        m.put(0,1);
        return  dfs(root,sum,0,m);
    }
    private int  dfs(TreeNode root, int sum , int cur , Map<Integer , Integer> m){
        if(root==null) return 0;
        int res = 0;
        cur+=root.val;
        if(m.containsKey(cur-sum)){
            res+=m.get(cur-sum);
        }
        if(!m.containsKey(cur)){
            m.put(cur,1);
        }else m.put(cur,m.get(cur)+1);
        res+=dfs(root.left,sum,cur,m);
        res+=dfs(root.right,sum,cur,m);
        m.put(cur,m.get(cur)-1);
        return res;
    }
}
```



## Solution Recursive

* for everyNode ,we want to calculate the sum included the node ‘s val is target or not 
* so we can implement it in typical recursive

```java
	class Solution {
        int ans = 0;
    public int pathSum(TreeNode root, int sum) {
        if(root==null) return 0;
        dfs(root,sum,0);
        pathSum(root.left,sum);
        pathSum(root.right,sum);
        return ans;
    }
    private void dfs(TreeNode root, int sum,int tmp){
        if(root==null) return;
        //we need  or we get the duplication 
        if(tmp==sum-root.val) ans++;
       dfs(root.left,sum,tmp+root.val);
        dfs(root.right,sum,tmp+root.val);
    }
}
```

Or

```java
class Solution {
    public int pathSum(TreeNode root, int sum) {
    if(root==null) return 0;
            int res=findSum(root,sum);
            res+=pathSum(root.left,sum);
            res+=pathSum(root.right,sum);
            return  res;
            
    }
    public int findSum(TreeNode node,int num){
        int count=0;
        if(node==null) return 0;
        if(num==node.val)   count+=1;
        count+=findSum(node.left,num-node.val);
        count+=findSum(node.right,num-node.val);
        return count;
    }
}
```

