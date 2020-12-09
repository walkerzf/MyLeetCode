## [Maximum Length of Subarray With Positive Product](https://leetcode-cn.com/problems/maximum-length-of-subarray-with-positive-product/)

> Given an array of integers nums, find the maximum length of a subarray where the product of all its elements is positive.
>
> A subarray of an array is a consecutive sequence of zero or more values taken out of that array.
>
> Return the maximum length of a subarray with positive product.
>
> ```
>  
> 
> Example 1:
> 
> Input: nums = [1,-2,-3,4]
> Output: 4
> Explanation: The array nums already has a positive product of 24.
> Example 2:
> 
> Input: nums = [0,1,-2,-3,-4]
> Output: 3
> Explanation: The longest subarray with positive product is [1,-2,-3] which has a product of 6.
> Notice that we cannot include 0 in the subarray since that'll make the product 0 which is not positive.
> Example 3:
> 
> Input: nums = [-1,-2,-3,0,1]
> Output: 2
> Explanation: The longest subarray with positive product is [-1,-2] or [-2,-3].
> Example 4:
> 
> Input: nums = [-1,2]
> Output: 1
> Example 5:
> 
> Input: nums = [1,2,3,5,-6,4,0,10]
> Output: 4
> 
> 
> Constraints:
> 
> 1 <= nums.length <= 10^5
> -10^9 <= nums[i] <= 10^9
> ```
>
> 



## Solution DP

```
设 POS[i] 是以 nums[i] 结尾，乘积为正的最长子数组的长度。

设 NEG[i] 是以 nums[i] 结尾，乘积为负的最长子数组的长度。

为了编写代码方便，设 nums 下标从 1 开始。那么 POS[0] = NEG[0] = 0。

接下来讨论一下 nums[i] 的值如何影响 POS[i] 及 NEG[i] 的计算。

当 nums[i] 为 0 时，显然有 POS[i] = NEG[i] = 0，即这样的子数组不存在。
当 nums[i] 为正时：
POS[i] = POS[i-1] + 1。
NEG[i] = NEG[i-1] ? (NEG[i-1] + 1) : 0。
为何计算NEG[i]时要判断 NEG[i-1] 不为 0 ？因为 nums[i] 自己没法成为一个乘积为负的数组。
当 nums[i] 为负时：
POS[i] = NEG[i-1] ? (NEG[i-1] + 1) : 0。 判断 NEG[i-1] 是否为 0 的原因同上。
NEG[i] = POS[i-1] + 1;
```



```c++
int dp[100001][2]; // dp[i][0] 即 POS，dp[i][1] 即 NEG。
class Solution {
public:
    int getMaxLen(vector<int>& nums) {
        dp[0][0] = dp[0][1] = 0;
        int anw = 0;
        for(int i = 1; i <= nums.size(); i++) {
            if(nums[i-1] == 0) {
                dp[i][0] = 0;
                dp[i][1] = 0;
            } else if(nums[i-1] > 0) {
                dp[i][0] = dp[i-1][0] + 1;
                dp[i][1] = dp[i-1][1] ? (dp[i-1][1] + 1) : 0;
            } else {
                dp[i][0] = dp[i-1][1] ? (dp[i-1][1] + 1) : 0;
                dp[i][1] = dp[i-1][0] + 1;
            }
            
            anw = max(anw, dp[i][0]);
        }
        return anw;
    }
};

```



## Solution

```java
class Solution {
    public int getMaxLen(int[] nums) {
        int max = 0;
        int len = nums.length;
        int firstN = -1;
        int lastN = -1;
        int countN = 0;
        int preZero = -1;
        for(int i = 0 ;i< len;i++){
            if(nums[i]==0){
                if(countN%2==0){
                    max = Math.max(max,i-1-preZero);
                }else{
                    int leftLen = firstN - preZero;
                    int rightLen = i -lastN;
                    max = Math.max(max,i-1-preZero- Math.min(leftLen,rightLen));
                }
                firstN =-1; lastN = -1; 
                preZero = i;
                countN = 0 ;
            }else if(nums[i]<0) {
                countN++;
                if(firstN==-1) firstN = i;
                lastN = i;
            }
        }

        if(countN%2==0) max = Math.max(max,len-preZero-1);
        else{
            int leftLen = firstN - preZero;
            int rightLen = len -lastN;
            max = Math.max(max , len- 1 - preZero- Math.min(leftLen,rightLen));
        }
        return max;
    }
}
```

