## 题干

> Given a set of distinct integers, nums, return all possible subsets (the power set).
>
> Note: The solution set must not contain duplicate subsets.
>
> Example:
>
> Input: nums = [1,2,3]
> Output:
> [
>   [3],
>   [1],
>   [2],
>   [1,2,3],
>   [1,3],
>   [2,3],
>   [1,2],
>   []
> ]

这是No.77问题的follow up，相当于从n个数字中取0--n个数数字的集合。

## solution1



```java
class Solution {
    List<List<Integer>> res=new LinkedList<>();
    
    private void findSubset(int []nums,int start,LinkedList<Integer> tmp){
        if(tmp.size()<=nums.length){
            res.add(new LinkedList<>(tmp));
        }
        for(int i=start;i<nums.length;i++){
            tmp.add(nums[i]);
            findSubset(nums,i+1,tmp);
            tmp.removeLast();
        }
        return;
    }
    
    public List<List<Integer>> subsets(int[] nums) {
        if(nums.length==0) return res;
        //res.add(new LinkedList<>());
        findSubset(nums,0,new LinkedList<Integer>());
        return res;
    }
}
```



* 或者用n次77问题的解法，选择剪枝的做法

```java
  for (int i=start;i<=n-(k-tmp.size())+1;i++){
            tmp.add(i);
            findCombine(n,k,i+1,tmp);
            tmp.remove(tmp.size()-1);
        }
```



## Solution2 Bit Operation

```java
class Solution {

    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res =new LinkedList<>();
        if(nums==null) return  res;
        for( int s=0;s<1<<nums.length;s++){     //s<2的n次方-1
            List<Integer> tmp=new LinkedList<>();
            for(int i=0;i<nums.length;i++){
                if((s & (1 << i))>0)   tmp.add(nums[i]); //这里是因为实际运算里大于0即为真，要
               											//将其转化为Boolean值
            }
             res.add(new LinkedList(tmp));
        }
        return res;
    }

}
```

