## [526. Beautiful Arrangement](https://leetcode-cn.com/problems/beautiful-arrangement/)

Suppose you have **N** integers from 1 to N. We define a beautiful arrangement as an array that is constructed by these **N** numbers successfully if one of the following is true for the ith position (1 <= i <= N) in this array:

1. The number at the ith position is divisible by **i**.
2. **i** is divisible by the number at the ith position.

 

Now given N, how many beautiful arrangements can you construct?

**Example 1:**

```
Input: 2
Output: 2
Explanation: 

The first beautiful arrangement is [1, 2]:

Number at the 1st position (i=1) is 1, and 1 is divisible by i (i=1).

Number at the 2nd position (i=2) is 2, and 2 is divisible by i (i=2).

The second beautiful arrangement is [2, 1]:

Number at the 1st position (i=1) is 2, and 2 is divisible by i (i=1).

Number at the 2nd position (i=2) is 1, and i (i=2) is divisible by 1.
```

 

**Note:**

1. **N** is a positive integer and will not exceed 15.

 

## Solution BackTraceing

```c++
//time 588ms
class Solution {
public:
    int res = 0;
    int countArrangement(int N) {
        vector<bool>  used(N+1,false);
        dfs(used,1,N);
        return res;
    }
    void dfs(vector<bool> &used,int count,int N){
        if(count>N) {
            res++;
            return;
        }
        for(int i =1;i<=N;i++){
            if(used[i]) continue;
            if(i%count==0||count%i==0){
                used[i] = true;
                dfs(used,count+1,N);
                used[i] = false;
            }
        }
    }
};
```



## Solution Condition Compression

```c++
//time 16ms
class Solution {
public:
    int countArrangement(int N) {
        int dp[1 << N];
        memset(dp,0,sizeof(dp));
        dp[0] = 1;
        for (int i = 0; i < (1 << N); i++) {
            int s = 1;
            for (int j = 0; j < N; j++) if ((i >> j) & 1) s++;
            for (int j = 1; j <= N; j++) {
                if ((!((i >> (j - 1))&1)) && (s % j == 0 || j % s == 0)) {
                    dp[i | (1 << (j - 1))] += dp[i];
                }
            }
        }
        return dp[(1 << N) - 1];
    }
};
```

