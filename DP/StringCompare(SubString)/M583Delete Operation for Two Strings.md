## [Delete Operation for Two Strings](https://leetcode-cn.com/problems/delete-operation-for-two-strings/)

> Given two words word1 and word2, find the minimum number of steps required to make word1 and word2 the same, where in each step you can delete one character in either string.
>
> ```
> Example 1:
> Input: "sea", "eat"
> Output: 2
> Explanation: You need one step to make "sea" to "ea" and another step to make "eat" to "ea".
> Note:
> The length of given words won't exceed 500.
> Characters in given words can only be lower-case letters.
> ```
>
> 

## Solution 

* 与编辑距离的题很相似

```java
//time  10ms
class Solution {
    public int minDistance(String word1, String word2) {
        int len1=word1.length();
        int len2=word2.length();
        if(len1==0) return len2;
        if(len2==0) return len1;
        int dp[][]=new int[len1+1][len2+1];
        for(int i=0;i<=len1;i++){
            dp[i][0]=i;
        }
        for(int i=0;i<=len2;i++){
            dp[0][i]=i;
        }
        for(int i=1;i<=len1;i++){
            for(int j=1;j<=len2;j++){
                if(word1.charAt(i-1)==word2.charAt(j-1)) {
                    dp[i][j]=dp[i-1][j-1];
                    continue;
                }
                dp[i][j]=Math.min(dp[i-1][j],dp[i][j-1])+1;
            }
        }
        return dp[len1][len2];
    }
}
```

## [712. Minimum ASCII Delete Sum for Two Strings](https://leetcode-cn.com/problems/minimum-ascii-delete-sum-for-two-strings/)

> Given two strings s1, s2, find the lowest ASCII sum of deleted characters to make two strings equal.
>
> ```
> Example 1:
> Input: s1 = "sea", s2 = "eat"
> Output: 231
> Explanation: Deleting "s" from "sea" adds the ASCII value of "s" (115) to the sum.
> Deleting "t" from "eat" adds 116 to the sum.
> At the end, both strings are equal, and 115 + 116 = 231 is the minimum sum possible to achieve this.
> Example 2:
> Input: s1 = "delete", s2 = "leet"
> Output: 403
> Explanation: Deleting "dee" from "delete" to turn the string into "let",
> adds 100[d]+101[e]+101[e] to the sum.  Deleting "e" from "leet" adds 101[e] to the sum.
> At the end, both strings are equal to "let", and the answer is 100+101+101+101 = 403.
> If instead we turned both strings into "lee" or "eet", we would get answers of 433 or 417, which are higher.
> Note:
> 
> 0 < s1.length, s2.length <= 1000.
> All elements of each string will have an ASCII value in [97, 122].
> ```

## Solution 

```java
//time  28ms
class Solution {
    public int minimumDeleteSum(String s1, String s2) {
        int len1=s1.length();
        int len2=s2.length();
        int dp[][]=new int [len1+1][len2+1];
        for(int i=1;i<=len1;i++){
            dp[i][0]=dp[i-1][0]+s1.charAt(i-1);
        }
        for(int i=1;i<=len2;i++){
            dp[0][i]=dp[0][i-1]+s2.charAt(i-1);
        }
        for(int i=1;i<=len1;i++){
            for(int j=1;j<=len2;j++){
                int a1=dp[i-1][j]+s1.charAt(i-1);
                int a2=dp[i][j-1]+s2.charAt(j-1);
                //int a3=
                if(s1.charAt(i-1)==s2.charAt(j-1)){
                    dp[i][j]=dp[i-1][j-1];
                    continue;
                }
                dp[i][j]=Math.min(a1,a2);

            }
        }
        return dp[len1][len2];
    }
}
```

