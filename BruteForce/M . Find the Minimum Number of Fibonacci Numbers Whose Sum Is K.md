## [5373. Find the Minimum Number of Fibonacci Numbers Whose Sum Is K](https://leetcode-cn.com/problems/find-the-minimum-number-of-fibonacci-numbers-whose-sum-is-k/)

> Given the number k, return the minimum number of Fibonacci numbers whose sum is equal to k, whether a Fibonacci number could be used multiple times.
>
> The Fibonacci numbers are defined as:
>
> F1 = 1
> F2 = 1
> Fn = Fn-1 + Fn-2 , for n > 2.
> It is guaranteed that for the given constraints we can always find such fibonacci numbers that sum k.
>
>
> Example 1:
>
> Input: k = 7
> Output: 2 
> Explanation: The Fibonacci numbers are: 1, 1, 2, 3, 5, 8, 13, ... 
> For k = 7 we can use 2 + 5 = 7.
> Example 2:
>
> Input: k = 10
> Output: 2 
> Explanation: For k = 10 we can use 2 + 8 = 10.
> Example 3:
>
> Input: k = 19
> Output: 3 
> Explanation: For k = 19 we can use 1 + 5 + 13 = 19.
>
>
> Constraints:
>
> 1 <= k <= 10^9

## Solution

* 从大的往小的试就好了

```c++
class Solution {
public:
    int findMinFibonacciNumbers(int k) {
        long long t[50];
        int i=2;
        t[0]=t[1]=1;
        while((t[i]=t[i-1]+t[i-2])<=k) ++i;
        int c=0;
        while(k)
        {
            if(k>=t[i]) k-=t[i],++c;
            --i;
        }
        return c;
    }
};
```

* 自己的做法 ，还要从第一个开始 太笨了

```java
class Solution {
    public int findMinFibonacciNumbers(int k) {
        if(k==1||k==2) return 1;
        int dp[]=new int[k+1];
        dp[1]=1;
        dp[2]=1;
        int ii=1;
        int ji=2;
        int index=3;
        while(index<=k){
            dp[index]=1;
            ii=ji;
            ji=index;
            index=ii+ji;
        }
        if(dp[k]==1) return 1;
        for(int i=3;i<=k;i++){
            if(dp[i]==1) continue;
            int j=i;
            while(j>0){
                if(dp[j]==1) break;
                j--;
            }
            dp[i]=dp[j]+dp[i-j];
        }
        return dp[k];
    }
}
```

