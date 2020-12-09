# No.497

You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols + and -. For each integer, you should choose one from + and - as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S.

Example 1:
		Input: nums is [1, 1, 1, 1, 1], S is 3. 
		Output: 5
		Explanation: 

-1+1+1+1+1 = 3
		+1-1+1+1+1 = 3
		+1+1-1+1+1 = 3
		+1+1+1-1+1 = 3
		+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
Note:
The length of the given array is positive and will not exceed 20.
The sum of elements in the given array will not exceed 1000.
Your output answer is guaranteed to be fitted in a 32-bit integer.

## Solution1：Search(DFS)

搜索解法即暴力解法，直接枚举放置加减符号的所有可能情况，判断结果是否等于Target，等于Target的时候，count++。

时间复杂度：O（$2^n$）,	n=20 指数量级的时间复杂度，非常恐怖，但是实际上还是可以**AC**的

空间复杂度：O（n）





## Solution2：DP

It is easy to think about dp(dynamic programing) to solve the problem because it only need the number of ways to assign symbols to make sum of integers equal to target S.

时间复杂度：O(n*sum) 

空间复杂度：O(n*sum)->O(n)

二位DP的状态是前i个数字组成j的和的方法个数，

```java
//push
W[i][j-num[i]]+=w[i-1][j];
W[i][j+num[i]]+=W[i-1][j];

//pull
W[i][j]=w[i-1][j-num[i]]
    +	W[i-1][j+num[i]];
```



## Solution3：DP优化,0-1背包问题

Optimization: Subset sum

Let  P denotes a set of nums hava a + sign in front of it

Let N denotes a set of nums have a - sign in front of it

$P\bigcup N={a_1,a_2,...,a_n}$

$P\bigcap N=\emptyset$

sum(P) -sum(N)= Target

sum(P)- ~~sum(N)~~+sum(P)+~~sum(N)~~=Target+sum(P)+sum(N)

2*sum(P)=Target+sum(array)

=> sum(P)=(target +sum(a))/2

这里变成一个0—1背包问题，从数组中能选出多少种组合组成这个和。

**类似No.416问题**

```java
//push
dp[i]=dp[i-1];
dp[i][j+a_i]+=dp[i-1][j];
//pull
dp[i][j]=dp[i-1][j]+dp[i-1][j-ai];


```

