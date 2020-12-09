## SubArray Sum Equals K

> Given an array of integers and an integer **k**, you need to find the total number of continuous subarrays whose sum equals to **k**.
>
> **Example 1:**
>
> ```
> Input:nums = [1,1,1], k = 2
> Output: 2
> ```
>
> 
>
> **Note:**
>
> 1. The length of the array is in range [1, 20,000].
> 2. The range of numbers in the array is [-1000, 1000] and the range of the integer **k** is [-1e7, 1e7].

## Solution Brute Force

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        int dp[]=new int[nums.length];
        for(int i=0;i<nums.length;i++){
            dp[i]+=((i==0)?0:dp[i-1])+nums[i];
        }
        int res=0;
        for(int i=0;i<nums.length;i++){
            if(dp[i]==k) res++;
            for(int j=0;j<i;j++){
                if(dp[i]-dp[j]==k) res++;
            }
        }
        return res;
    }
}
```

## Solution Map as Cache  O(N)

```java
//time 11ms
class Solution {
    public int subarraySum(int[] nums, int k) {
        Map<Integer,Integer> m=new HashMap<>();
        int sum=0;
        int count=0;
        m.put(0,1);
        for(int i=0;i<nums.length;i++){
            sum+=nums[i];
            if(m.containsKey(sum-k)){
                count+=m.get(sum-k);
            }
            m.put(sum,m.getOrDefault(sum,0)+1);
        }
        
        return count;
    }
}

```

