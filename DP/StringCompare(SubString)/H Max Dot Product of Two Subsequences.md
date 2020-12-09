## [Max Dot Product of Two Subsequences](https://leetcode-cn.com/problems/max-dot-product-of-two-subsequences/)

> Given two arrays nums1 and nums2.
>
> Return the maximum dot product between non-empty subsequences of nums1 and nums2 with the same length.
>
> A subsequence of a array is a new array which is formed from the original array by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, [2,3,5] is a subsequence of [1,2,3,4,5] while [1,5,3] is not).
>
> ```
>  
> 
> Example 1:
> 
> Input: nums1 = [2,1,-2,5], nums2 = [3,0,-6]
> Output: 18
> Explanation: Take subsequence [2,-2] from nums1 and subsequence [3,-6] from nums2.
> Their dot product is (2*3 + (-2)*(-6)) = 18.
> Example 2:
> 
> Input: nums1 = [3,-2], nums2 = [2,-6,7]
> Output: 21
> Explanation: Take subsequence [3] from nums1 and subsequence [7] from nums2.
> Their dot product is (3*7) = 21.
> Example 3:
> 
> Input: nums1 = [-1,-1], nums2 = [1,1]
> Output: -1
> Explanation: Take subsequence [-1] from nums1 and subsequence [1] from nums2.
> Their dot product is -1.
> 
> 
> Constraints:
> 
> 1 <= nums1.length, nums2.length <= 500
> -1000 <= nums1[i], nums2[i] <= 1000
> ```

## Solution 

* similar to the question 

  * H72 Edit Distance
  * M 1143 Longest Common Subsequence

  


```java
//Memory Exceed
class Solution {
    public int maxDotProduct(int[] nums1, int[] nums2) {
        int len1=nums1.length;
        int len2=nums2.length;
        int max=Math.min(len1,len2);
        int res=Integer.MIN_VALUE;
        int dp[][][]=new int[len1+1][len2+1][max+1] ;
        for(int i=1;i<=len1;i++){
            for(int j=1;j<=len2;j++){
                for(int l=1;l<=max;l++){
                dp[i][j][l]=Integer.MIN_VALUE;
                dp[0][j][lx`]=Integer.MIN_VALUE/2;
                dp[i][0][l]=Integer.MIN_VALUE/2;
                }
            }
        }
         
        for(int i=1;i<=max;i++){
            for(int l=1;l<=len1;l++){
                for(int j=1;j<=len2;j++){
                    int tmp=Math.max(dp[l-1][j][i],dp[l][j-1][i]);
                    dp[l][j][i]=Math.max(dp[l-1][j-1][i-1]+nums1[l-1]*nums2[j-1],tmp);
                    res=Math.max(res,dp[l][j][i]);
                }
            }
        }
        return res;
    }
}
```

##   Solution

* we expand the array to avoid the border case ,but we need initalize the arry to MINVALUE/2 to avoid negative OverFlow

  ![image-20200525095354748](C:\Users\15524\AppData\Roaming\Typora\typora-user-images\image-20200525095354748.png)

* we do not expand the array in every loop ,we initailze the dp[i] [j] =nums[i]*nums2[j]*

```java
class Solution {
    public int maxDotProduct(int[] nums1, int[] nums2) {
        int len1=nums1.length;
        int len2=nums2.length;
        int max=Math.min(len1,len2);
        int res=Integer.MIN_VALUE;
        int dp[][]=new int[len1+1][len2+1] ;
        for(int i=0;i<=len1;i++){
            for(int j=0;j<=len2;j++){
                    dp[i][j]=Integer.MIN_VALUE/2;
                
            }
        }
        int tmp=0;
            for(int l=1;l<=len1;l++){
                for(int j=1;j<=len2;j++){
                    //dp[l][j] is the min
                    // dp[l-1][j] dp[l][j-1]
                    //dp[l-1][j-1]+nums1*nums2 
                    //nums1*nums2
                   dp[l][j]=Math.max(dp[l-1][j],Math.max(dp[l][j-1],Math.max(dp[l-1][j-1]+nums1[l-1]*nums2[j-1],nums1[l-1]*nums2[j-1])));
                }
            }
        
        return dp[len1][len2];
    }
}
```

* or we record the end with i and j ‘s max 
* dp array record the  array  i and j max not end with 

```java
class Solution {
    public int maxDotProduct(int[] nums1, int[] nums2) {
        int len1=nums1.length;
        int len2=nums2.length;
        int max=Math.min(len1,len2);
        int res=Integer.MIN_VALUE;
        int dp[][]=new int[len1+1][len2+1] ;
        int tmp=0;
            for(int l=1;l<=len1;l++){
                for(int j=1;j<=len2;j++){
                    tmp=Math.max(dp[l-1][j],dp[l][j-1]);
                    dp[l][j]=Math.max(dp[l-1][j-1]+nums1[l-1]*nums2[j-1],dp[l][j]);
                    dp[l][j]=Math.max(dp[l][j],tmp);
                    res=Math.max(res,dp[l-1][j-1]+nums1[l-1]*nums2[j-1]);   
                }
            }
        return res;
    }
}
```

