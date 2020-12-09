## 题干

> Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.
>
> Example:
>
> Input: n = 4, k = 2
> Output:
> [
>   [2,4],
>   [3,4],
>   [2,3],
>   [1,2],
>   [1,3],
>   [1,4],
> ]
>
> 

这是一个非常经典的组合题目，从n个数字里面选k个数字，$C_n^k$种结果。

## Solution

```java
class Solution {
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> res=new LinkedList<>();
        if(n<=0||k<=0) return res;
        recursion(n,k,1,res,new LinkedList<Integer>());
        return res;
    }

    private void recursion(int n, 
                           int k,
                           int index,
                           List<List<Integer>> res,
                           List<Integer> tmp ){
        if(k==0){
            res.add(new LinkedList(tmp));
        }
        for( int i=index;i<=n;i++){
            tmp.add(i);
            recursion(n,k-1,i+1,res,tmp);
            tmp.remove(tmp.size()-1);
        }
    }
}
```

* 由于此类的题目会遍历所有结果，所以往往剪枝会减少很多的计算量

```java
  for (int i=start;i<=n-(k-tmp.size())+1;i++){
            tmp.add(i);
            findCombine(n,k,i+1,tmp);
            tmp.remove(tmp.size()-1);
        }
```

* 可以看过for循环的终止条件相对于前面n变小了，因为每层的循环，需要在数列中留下 一定量的数字，这些数字由n、 当前tmp数组的大小和所需要的元素数量k所决定。