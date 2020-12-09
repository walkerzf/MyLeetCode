##  Maximum Sum Circular Subarray

* 53题的Follow Up

> Given a **circular array** **C** of integers represented by `A`, find the maximum possible sum of a non-empty subarray of **C**.
>
> Here, a *circular array* means the end of the array connects to the beginning of the array. (Formally, `C[i] = A[i]` when `0 <= i < A.length`, and `C[i+A.length] = C[i]` when `i >= 0`.)
>
> Also, a subarray may only include each element of the fixed buffer `A` at most once. (Formally, for a subarray `C[i], C[i+1], ..., C[j]`, there does not exist `i <= k1, k2 <= j` with `k1 % A.length = k2 % A.length`.)
>
>  
>
> **Example 1:**
>
> ```
> Input: [1,-2,3,-2]
> Output: 3
> Explanation: Subarray [3] has maximum sum 3
> ```
>
> **Example 2:**
>
> ```
> Input: [5,-3,5]
> Output: 10
> Explanation: Subarray [5,5] has maximum sum 5 + 5 = 10
> ```
>
> **Example 3:**
>
> ```
> Input: [3,-1,2,-1]
> Output: 4
> Explanation: Subarray [2,-1,3] has maximum sum 2 + (-1) + 3 = 4
> ```
>
> **Example 4:**
>
> ```
> Input: [3,-2,2,-3]
> Output: 3
> Explanation: Subarray [3] and [3,-2,2] both have maximum sum 3
> ```
>
> **Example 5:**
>
> ```
> Input: [-2,-3,-1]
> Output: -1
> Explanation: Subarray [-1] has maximum sum -1
> ```
>
>  
>
> **Note:**
>
> 1. `-30000 <= A[i] <= 30000`
> 2. `1 <= A.length <= 30000`

## Solution One pass

Ref：https://en.wikipedia.org/wiki/Maximum_subarray_problem

https://leetcode.com/explore/challenge/card/may-leetcoding-challenge/536/week-3-may-15th-may-21st/3330/discuss/178422/One-Pass?page=2

![image](https://assets.leetcode.com/users/motorix/image_1538888300.png)

```java
class Solution {
    public int maxSubarraySumCircular(int[] A) {
        int curMax=Integer.MIN_VALUE;
        int sum=0;
        int curMin=Integer.MAX_VALUE;
        int min=Integer.MAX_VALUE;
        int max=Integer.MIN_VALUE;
        for(int i=0;i<A.length;i++){
            sum+=A[i];
            if(curMax>0) curMax=curMax+A[i];
            else curMax=A[i];
            max=Math.max(max,curMax);
            if(curMin<0) curMin=curMin+A[i];
            else curMin=A[i];
            min=Math.min(curMin,min);
        }
        if(max<0) return max;
        return Math.max(sum-min,max);
    }
}
```

