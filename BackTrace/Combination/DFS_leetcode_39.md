## 题干


> Given a set of candidate numbers (candidates) (**without duplicates**) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.
>
> The same repeated number may be chosen from candidates unlimited number of times.
>
> Note:
>
> All numbers (including target) will be positive integers.
>The solution set must not contain duplicate combinations.
> Example 1:
> 
> Input: candidates = [2,3,6,7], target = 7,
>A solution set is:
> [
> [7],
> [2,2,3]
> ]
> Example 2:
> 
> Input: candidates = [2,3,5], target = 8,
>A solution set is:
> [
> [2,2,2,2],
> [2,3,3],
> [3,5]
> ]

这是一个典型的组合问题。

## Solution1 

```java
class Solution {
    List<List<Integer>> res=new LinkedList<>();

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
            if(target==0||candidates==null) return res;
            recursion(candidates,target,0,new LinkedList<Integer>());     
            return res;
    }

    private void recursion(int[] candidates,int target,int index,List<Integer> tmp){
        if(target==0) {
            res.add(new LinkedList(tmp));
            return ;
        }
        else if(target<0) return;
        for(int i=index;i<candidates.length;i++){
            tmp.add(candidates[i]);
            recursion(candidates,target-candidates[i],i,tmp);
            tmp.remove(tmp.size()-1);
        }

    }
}
```



## Solution 2 优化版本

* 将数组排序
* 在递归的循环中利用排序的数组对递归树剪枝，提高运行效率

```java
class Solution {
    List<List<Integer>> res=new LinkedList<>();

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
            if(target==0||candidates==null) return res;
            Arrays.sort(candidates);
            recursion(candidates,target,0,new LinkedList<Integer>());
        
            return res;
    }

    private void recursion(int[] candidates,int target,int index,List<Integer> tmp){
        if(target==0) {
            res.add(new LinkedList(tmp));
            return ;
        }
        else if(target<0) return;
        for(int i=index;i<candidates.length;i++){
            //剪枝
            if(candidates[i]>target) break;
            tmp.add(candidates[i]);
            recursion(candidates,target-candidates[i],i,tmp);
            tmp.remove(tmp.size()-1);
        }

    }
}
```



## Follow up

* 题目变形：
  * 要求输出的数列顺序为从小到大

> Example：
>
> ​	[
>
> ​	[3,5]，
>
> ​	[2,3,3],
> ​			[2,2,2,2],
>
> ​	]

```java
class Solution {
    List<List<Integer>> res=new LinkedList<>();

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
            if(target==0||candidates==null) return res;
            Arrays.sort(candidates);
        	/*
        	按List的长度进行递归，递归的传递参数变多
        	*/
            for(int length=1;length<=target/candidates[0];length++){
            recursion(candidates,target,0,0,length,new LinkedList<Integer>());
            }
            return res;
    }

    private void recursion(int[] candidates,int target,int index,int n,int length,List<Integer> tmp){
        /*
        出递归的条件为两个，满足递归深度
        */
        if(length==n){
            if(target==0) {
                res.add(new LinkedList(tmp));
                return ;
            }
        }
        else return；
      
        for(int i=index;i<candidates.length;i++){
         	//剪枝
            if(candidates[i]>target||n>length) break;
            tmp.add(candidates[i]);
            recursion(candidates,target-candidates[i],i,n+1,length,tmp);
            tmp.remove(tmp.size()-1);
        }

    }
}
```

