## 题干

> Given a collection of distinct integers, return all possible permutations.
>
> Example:
>
> Input: [1,2,3]
> Output:
> [
>   [1,2,3],
>   [1,3,2],
>   [2,1,3],
>   [2,3,1],
>   [3,1,2],
>   [3,2,1]
> ]
>
> 

## Solution1 Permutation

```java
class Solution {
        List<List<Integer>> res=new LinkedList<>();
        boolean []used;
    public List<List<Integer>> permute(int[] nums) {
        used=new boolean[nums.length];
        if(nums==null||nums.length==0) return res;
        recursion(nums,new LinkedList<Integer>(),nums.length);
        return res;
    }

    private void recursion(int []nums,List<Integer> tmp,int count){
        if(count==0){
            res.add(new LinkedList(tmp));
        }
        for(int i=0;i<nums.length;i++){
            if(used[i]==true) {
                continue;
            }
            used[i]=true;
            tmp.add(nums[i]);
            recursion(nums,tmp,count-1);
            tmp.remove(tmp.size()-1);
            used[i]=false;
        }
    }
}
```

```java
 boolean []used;//
 Boolean []used;//这里的大写的Boolean是boolean的封装类，拥有一样的属性和方法，但是初始化数组时，初始值全为null，导致我后面的判断出错
```

