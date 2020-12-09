## Build Array Where You Can Find The Maximum Exactly K Comparisons

> Given three integers n, m and k. Consider the following algorithm to find the maximum element of an array of positive integers:
>
> ![img](https://assets.leetcode.com/uploads/2020/04/02/e.png)
>
>
> You should build the array arr which has the following properties:
>
> arr has exactly n integers.
> 1 <= arr[i] <= m where (0 <= i < n).
> After applying the mentioned algorithm to arr, the value search_cost is equal to k.
> Return the number of ways to build the array arr under the mentioned conditions. As the answer may grow large, the answer must be computed modulo 10^9 + 7.
>
>  
>
> ```
> Example 1:
> 
> Input: n = 2, m = 3, k = 1
> Output: 6
> Explanation: The possible arrays are [1, 1], [2, 1], [2, 2], [3, 1], [3, 2] [3, 3]
> ```
>
> ```
> Example 2:
> 
> Input: n = 5, m = 2, k = 3
> Output: 0
> Explanation: There are no possible arrays that satisify the mentioned conditions.
> ```
>
> ```
> Example 3:
> 
> Input: n = 9, m = 1, k = 1
> Output: 1
> Explanation: The only possible array is [1, 1, 1, 1, 1, 1, 1, 1, 1]
> ```
>
> ```
> Example 4:
> 
> Input: n = 50, m = 100, k = 25
> Output: 34549172
> Explanation: Don't forget to compute the answer modulo 1000000007
> ```
>
> ```
> Example 5:
> 
> Input: n = 37, m = 17, k = 7
> Output: 418930126
> ```
>
> ```
> Constraints:
> 
> 1 <= n <= 50
> 1 <= m <= 100
> 0 <= k <= n
> ```
>
> 





## Solution Top Down

* 记忆化数组

```java
//time  123ms 39.4MB
class Solution {
    int modulo=1000000007;
    Integer dp[][][];
    public int numOfArrays(int n, int m, int k) {
        dp=new Integer[n+1][m+1][k+1];
        return  dfs(n,m,k,0,0,0);
    }
    private int dfs(int n,int m, int k,int len,int curMax,int curCost){
        if(len==n){
            if(curCost==k) return 1;
            else return 0;
        }
        if(dp[len][curMax][curCost]!=null) return dp[len][curMax][curCost];
        int ans=0;
        for(int num=1;num<=m;num++){
            int newMax=curMax;
            int newCost=curCost;
            if(num>newMax){
                newMax=num;
                newCost++;
            }
            if(newCost>k) break;
            ans=(ans+dfs(n,m,k,len+1,newMax,newCost))%modulo;
        }
        dp[len][curMax][curCost]=ans;
        return ans;
    }
}
```

* 优化版本 类似于 Bottom up

```java
class Solution {
    public int numOfArrays(int n, int m, int k) {
        Integer[][][] dp = new Integer[n + 1][m + 1][k + 1];
        return dfs(n, m, k, 0, 0, 0, dp);
    }
    // dfs(... i, currMax, currCost) the number of ways to build the valid array `arr[i:]`
    //     keeping in mind that current maximum element is `currMax` and current search cost is `currCost`
    int dfs(int n, int m, int k, int i, int currMax, int currCost, Integer[][][] dp) {
        if (i == n) {
            if (k == currCost) return 1; // valid if the value search cost is equal to k
            return 0;
        }
        if (dp[i][currMax][currCost] != null) return dp[i][currMax][currCost];
        int ans = 0;
        // Case 1: num in range [1, currMax], newMax = currMax, newCost = currCost
        ans += (long) currMax * dfs(n, m, k, i + 1, currMax, currCost, dp) % 1_000_000_007;

        // Case 2: num in range [currMax+1, m], newMax = num, newCost = currCost + 1
        if (currCost + 1 <= k) {
            for (int num = currMax + 1; num <= m; num++) {
                ans += dfs(n, m, k, i + 1, num, currCost + 1, dp);
                ans %= 1_000_000_007;
            }
        }
        return dp[i][currMax][currCost] = ans;
    }
}
```



## Solution Bottom Top

* 或者用long 的数组

```java
//time 200ms 38.9MB
class Solution {
    public int numOfArrays(int n, int m, int k) {
        int modulo=1000000007;
        // param 1: length  param2: maxValue  param3: Cost
        int  dp[][][]=new int[n+1][m+1][k+1];
        for(int i=1;i<=m;i++){
            dp[1][i][1]=1;
        }
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
                for(int l=1;l<=k;l++){
                    int ans=0;
                    //apend <j to the end of the array
                    for(int y=1;y<=j;y++){
                        ans=(ans+dp[i-1][j][l])%modulo;
                    }
                    //apend j to end of the array
                    for(int x=1;x<j;x++){
                        ans+=dp[i-1][x][l-1]%modulo;
                        ans%=modulo;
                    }
                    dp[i][j][l]+=ans%modulo;
                }
            }
        }
        int res=0;
        for(int i=1;i<=m;i++){
            res=(res+dp[n][i][k])%modulo;
        }
        return (int)res;
    }
}

```

