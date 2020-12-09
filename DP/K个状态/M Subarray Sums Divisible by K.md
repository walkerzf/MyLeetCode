## [Subarray Sums Divisible by K](https://leetcode-cn.com/problems/subarray-sums-divisible-by-k/)

> Given an array A of integers, return the number of (contiguous, non-empty) subarrays that have a sum divisible by K.
>
>  
>
> ```
> Example 1:
> 
> Input: A = [4,5,0,-2,-3,1], K = 5
> Output: 7
> Explanation: There are 7 subarrays with a sum divisible by K = 5:
> [4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]
> 
> 
> Note:
> 
> 1 <= A.length <= 30000
> -10000 <= A[i] <= 10000
> 2 <= K <= 10000
> 
> 
> ```

* the problem is focus on the mod operation 
* i notice that the minus mod opearation in java will get a minus too 

##  Solution  no clear ideas about mod operation

```java
//timee 45ms
class Solution {
    public int subarraysDivByK(int[] A, int K) {
        Map<Integer,Integer> m=new HashMap<>();
        m.put(0,1);
        int sum=0;
        int res=0;
        for(int i=0;i<A.length;i++){
            sum+=A[i];
            if(m.containsKey(sum%K)){
                res+=m.get(sum%K);
            }
            if(sum%K>0){
                if(m.containsKey((sum%K)-K)){
                    res+=m.get(sum%K-K);
                   }
             }else {
                   if(m.containsKey((sum%K)+K)){
                       res+=m.get((sum%K)+K);
                   }
               }
                      
            
            if(m.containsKey(sum%K)){
                m.put(sum%K,m.get(sum%K)+1);
            }else{
                m.put(sum%K,1);
            }
        }
        return res;
    }
}
```

## elegant Mod

```java
class Solution {
   public int subarraysDivByK(int[] A, int K) {
        Map<Integer, Integer> record = new HashMap<>();
        record.put(0, 1);
        int sum = 0, ans = 0;
        for (int elem: A) {
            sum += elem;
            // 注意 Java 取模的特殊性，当被除数为负数时取模结果为负数，需要纠正
            int modulus = (sum % K + K) % K;
            int same = record.getOrDefault(modulus, 0);
            ans += same;
            record.put(modulus, same + 1);
        }
        return ans;
    }
}
```



## Solution DP

```java
class Solution {

    public int subarraysDivByK3(int[] A, int K) {
        Map<Integer, Integer> record = new HashMap<>();
        record.put(0, 1);

        int sum = 0;
		
        for (int num: A) {
            sum += num;
			//think about the java mod operation 
            // 保证 tmp 是正数
            int tmp = (sum % K + K) % K;
            record.put(tmp, record.getOrDefault(tmp, 0)+1);
        }

        int ans = 0;
        // for (Map.Entry<Integer, Integer> entry: record.entrySet()) {
        //     int value = entry.getValue();
        //     ans += value*(value-1)/2;
        // }

        for (Integer value: record.values()) {
            ans += value * (value - 1) / 2;
        }

        return ans;
    }
		//dp Solution ,we create an array ' length == modulus 
    public int subarraysDivByK(int[] A, int K) {
        
        int[] dp = new int[K];  // 要么除 K = 0, 只有当两个都是在里面的时候才进行加入
        int count = 0;
        int ans = 0;
        
        for (int i = 0; i < A.length; i++) {
            count += A[i];

            int t = (count % K + K) % K;

            if (t == 0) {
                dp[0] += 1;
                ans += dp[0];
            } else if (dp[t] == 0) {
                dp[t] = 1;
            }  else {
                ans += dp[t];
                dp[t] += 1;
            }
        }

        return ans;
    }
}
```

