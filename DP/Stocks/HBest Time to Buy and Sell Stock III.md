## Best Time to Buy and Sell Stock III

> Say you have an array for which the *i*th element is the price of a given stock on day *i*.
>
> Design an algorithm to find the maximum profit. You may complete at most *two* transactions.
>
> **Note:** You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).
>
> **Example 1:**
>
> ```
> Input: [3,3,5,0,0,3,1,4]
> Output: 6
> Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
>              Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.
> ```
>
> **Example 2:**
>
> ```
> Input: [1,2,3,4,5]
> Output: 4
> Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
>              Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are
>              engaging multiple transactions at the same time. You must sell before buying again.
> ```
>
> **Example 3:**
>
> ```
> Input: [7,6,4,3,1]
> Output: 0
> Explanation: In this case, no transaction is done, i.e. max profit = 0.
> ```

## Solution

```
dp[k, i] = max(dp[k, i-1], prices[i] - prices[j] + dp[k-1, j-1]), j=[0..i-1]
```



>For k transactions, on i-th day,
if we don't trade then the profit is same as previous day dp[k, i-1];
and if we bought the share on j-th day where j=[0..i-1], then sell the share on i-th day then the profit is prices[i] - prices[j] + dp[k-1, j-1] .
Actually j can be i as well. When j is i, the one more extra item prices[i] - prices[j] + dp[k-1, j] = dp[k-1, i] looks like we just lose one chance of transaction.

> I see someone else use the formula dp[k, i] = max(dp[k, i-1], prices[i] - prices[j] + dp[k-1, j]), where the last one is dp[k-1, j] instead of dp[k-1, j-1]. It's not the direct sense, as if the share was bought on j-th day, then the total profit of previous transactions should be done on (j-1)th day. However, the result based on that formula is also correct, because if the share was sold on j-th day and then bought again, it is the same if we didn't trade on that day.



```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length==0) return 0 ;
        int len =prices.length;
        int dp[][] = new int[3][len];
        //dp[k][i] = Math.max(dp[k][i-1],prices[i]-prices[j]+dp[k-1][j-1]);
        for(int k =1;k<=2;k++){
            for(int i =1 ;i < len;i++){
                int min = prices[0];
                for(int j =1;j<=i;j++){
                    min = Math.min(min,prices[j]-dp[k-1][j-1]);
                    dp[k][i] =Math.max(dp[k][i-1],prices[i]-min);
                }
            }
        }
        return dp[2][len-1];
    }
}
```

```=> optimazition for time complexity```

```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length==0) return 0 ;
        int len =prices.length;
        int dp[][] = new int[3][len];
        //dp[k][i] = Math.max(dp[k][i-1],prices[i]-prices[j]+dp[k-1][j-1]);
        for(int k =1;k<=2;k++){
             int min = prices[0];
            //do not need to calculate every min value for each iteration 
            for(int i =1 ;i < len;i++){
                min =Math.min(min,prices[i] - dp[k-1][i-1]);
                dp[k][i] = Math.max(dp[k][i-1],prices[i] - min);
            }
        }
        return dp[2][len-1];
    }
}
```

```=> slightlt swap the loop```

```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length==0) return 0 ;
        int len =prices.length;
        int dp[][] = new int[3][len];
        int min [] =new int[3];
        Arrays.fill(min,prices[0]);
        //dp[k][i] = Math.max(dp[k][i-1],prices[i]-prices[j]+dp[k-1][j-1]);
        for(int i =1 ;i<len;i++){
            for(int  k=1 ;k<=2;k++){
                min[k] = Math.min(min[k],prices[i] - dp[k-1][i-1]);
                dp[k][i] = Math.max(dp[k][i-1] ,prices[i]-min[k]);
            }
        }
        return dp[2][len-1];
    }
}
```

```=> the condition only depend on i-1 condition``

```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length==0) return 0 ;
        int len =prices.length;
        int dp[] = new int[3];
        int min [] =new int[3];
        Arrays.fill(min,prices[0]);
        //dp[k][i] = Math.max(dp[k][i-1],prices[i]-prices[j]+dp[k-1][j-1]);
        for(int i =1 ;i<len;i++){
            for(int  k=1 ;k<=2;k++){
                min[k] = Math.min(min[k],prices[i] - dp[k-1]);
                dp[k] = Math.max(dp[k] ,prices[i]-min[k]);
            }
        }
        return dp[2];
    }
}
```

```=> we change the condition to two variable ```

```java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length==0) return 0 ;
        int len = prices.length;
        int buy1 = Integer.MAX_VALUE;
        int buy2 = Integer.MAX_VALUE;
        int sell1 = 0;
        int sell2 = 0;
        //dp[k][i] = Math.max(dp[k][i-1],prices[i]-prices[j]+dp[k-1][j-1]);
        for(int i = 0 ;i<len;i++){
            buy1 = Math.min(buy1,prices[i]);
            sell1 = Math.max(sell1,prices[i]-buy1);
            buy2 = Math.min(buy2,prices[i]-sell1);
            sell2 = Math.max(sell2,prices[i] - buy2);
            
        }
        return sell2;
    }
}
```

