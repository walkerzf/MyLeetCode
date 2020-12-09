## 题干


> Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.
>
> Each number in candidates may only be used once in the combination.
>
> Note:
>
> All numbers (including target) will be positive integers.
> The solution set must not contain duplicate combinations.
> Example 1:
>
> Input: candidates = [10,1,2,7,6,1,5], target = 8,
> A solution set is:
> [
>   [1, 7],
>   [1, 2, 5],
>   [2, 6],
>   [1, 1, 6]
> ]
> Example 2:
>
> Input: candidates = [2,5,2,1,2], target = 5,
> A solution set is:
> [
>   [1,2,2],
>   [5]
> ]
>
> 

这是一个39题组合问题的变形。

How  to remove duplicates? 这里最关键的是因为给出的candidate数组中存在重复元素，所以可能存在重复的list能达到target sum。

39题中，不存在重复元素，但是元素却可以使用多次，所以只要从前往后，就不会出现重复的list。

* **Use Set.** 比较自然的做法
* **Disallow same number in same depth.** 在同一递归深度禁用同样的元素。

## Solution1 Use Set

```java
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> res= new LinkedList<>();
        HashSet<List<Integer>> dic =new HashSet<>();
        if(target==0||candidates==null) return res;
        Arrays.sort(candidates);
        recursion(candidates,target,0,res,new LinkedList<>(),dic);
        return res;    
    }

    private void recursion(int [] candidates,
                             int target,
                              int index,
                              List<List<Integer>> res,
                              List<Integer> tmp,
                              HashSet<List<Integer>> dic){
        
        if(target==0){
            if(dic.contains(tmp)) return;
            else {
                dic.add(new LinkedList(tmp));
                res.add(new LinkedList(tmp));
            }
        }
            for(int i=index;i<candidates.length;i++){
                //不加剪枝的步骤，会超时
                if(candidates[i]>target) return;
                tmp.add(candidates[i]);
                recursion(candidates,target-candidates[i],i+1,res,tmp,dic);
                tmp.remove(tmp.size()-1);
            }
        }
    }

```



## Solution 2 Disallow same number in same depth

```java
	class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> res= new LinkedList<>();
        
        if(target==0||candidates==null) return res;
        Arrays.sort(candidates);
        recursion(candidates,target,0,res,new LinkedList<>());
        return res;    
    }

    private void recursion(int [] candidates,
                             int target,
                              int index,
                              List<List<Integer>> res,
                              List<Integer> tmp){
        
        if(target==0){
            
                res.add(new LinkedList(tmp));
            }
            for(int i=index;i<candidates.length;i++){
                //这里还是剪枝
                if(candidates[i]>target) return;
                //这里在同一递归深度禁用同样的元素，可以
                if(i>index&&candidates[i]==candidates[i-1]) continue;
                tmp.add(candidates[i]);
                recursion(candidates,target-candidates[i],i+1,res,tmp);
                tmp.remove(tmp.size()-1);
            }
        }
    }

```



