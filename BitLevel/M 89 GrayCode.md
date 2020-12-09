## Gray Code

> The gray code is a binary numeral system where two successive values differ in only one bit.
>
> Given a non-negative integer n representing the total number of bits in the code, print the sequence of gray code. A gray code sequence must begin with 0.
>
> ```
> Example 1:
> 
> Input: 2
> Output: [0,1,3,2]
> Explanation:
> 00 - 0
> 01 - 1
> 11 - 3
> 10 - 2
> 
> For a given n, a gray code sequence may not be uniquely defined.
> For example, [0,2,3,1] is also a valid gray code sequence.
> 
> 00 - 0
> 10 - 2
> 11 - 3
> 01 - 1
> Example 2:
> 
> Input: 0
> Output: [0]
> Explanation: We define the gray code sequence to begin with 0.
>  A gray code sequence of n has size = 2n, which for n = 0 the size is 20 = 1.
> Therefore, for n = 0 the gray code sequence is [0].
> ```

## Solution BitLevel

Time O(2^N)

Space O(2^N)

```java
//time 2ms

class Solution {
    public List<Integer> grayCode(int n) {
    List<Integer> res=new LinkedList<>();
    res.add(0);
    for(int i=0;i<n;i++){
        int size=res.size();
        for(int j=size-1;j>=0;j--){
            res.add(res.get(j)+(1<<i));
        }
    }
    return res;
    }
}
```

## Solution  DP

```c++

// Author: Huahua
class Solution {
public:
  vector<int> grayCode(int n) {
    vector<vector<int>> dp(n + 1);
    dp[0] = {0};    
    for (int i = 1; i <= n; ++i) {
      dp[i] = dp[i - 1];
      for (int j = dp[i - 1].size() - 1; j >= 0; --j)
        dp[i].push_back(dp[i - 1][j] | (1 << (i - 1)));
    }
    return dp[n];
  }
};
```

## Solution OneLine

The idea is simple. G(i) = i^ (i/2).

```java
public List<Integer> grayCode(int n) {
    List<Integer> result = new LinkedList<>();
    for (int i = 0; i < 1<<n; i++) result.add(i ^ i>>1);
    return result;
}
```

