## 题干

> Find all possible combinations of k numbers that add up to a number n, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.
>
> Note:
>
> All numbers will be positive integers.
> The solution set must not contain duplicate combinations.
> Example 1:
>
> Input: k = 3, n = 7
> Output: [[1,2,4]]
> Example 2:
>
> Input: k = 3, n = 9
> Output: [[1,2,6], [1,3,5], [2,3,4]]
>
> 

## Solution 1 usual combination

```java

class Solution {
    List<List<Integer>> res=new LinkedList<>();
    public List<List<Integer>> combinationSum3(int k, int n) {
           
            if(k<=0||n<=0) return res;
            recursion(k,n,0,1,new LinkedList<Integer>());
            return res;
    }
    private void recursion(int k,int n,int count,int start,List<Integer> tmp ){
            if(n==0&&count==k) res.add(new LinkedList(tmp));
            for(int i=start;i<=9-(k-tmp.size())+1;i++){
                if(count>=k||i>n) break;
                tmp.add(i);
                recursion(k,n-i,count+1,i+1,tmp);
                tmp.remove(tmp.size()-1);
            }
    }
}
```

## Solution2 Bit operation

<img src="C:\Users\15524\AppData\Roaming\Typora\typora-user-images\image-20200219234432696.png" alt="image-20200219234432696" style="zoom:67%;" />

<img src="C:\Users\15524\AppData\Roaming\Typora\typora-user-images\image-20200219234714465.png" alt="image-20200219234714465" style="zoom:67%;" />

```java
class Solution {
    
    public List<List<Integer>> combinationSum3(int k, int n) {
            List<List<Integer>> res=new LinkedList<>();
            if(k<=0||n<=0) return res;
            //生成所有的2^9-1次方中可能的组合，subset
            for(int i=0;i<(1<<9);i++){
                List<Integer> tmp=new LinkedList<>();
                int sum=0;
                for(int j=1;j<=9;j++){
                    if((i&(1<<(j-1)))>0){
                        tmp.add(j);
                        sum+=j;
                    }
                   //if(tmp.size()==k) break;    
                   //如果这样写，每次碰到前缀是124都会视作正确答案              
                }
                 if(tmp.size()==k&&sum==n){
                        res.add(new LinkedList(tmp));
                    }
            }
            return res;
    }
}
```

