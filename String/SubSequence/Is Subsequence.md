##  Is Subsequence

> Given a string **s** and a string **t**, check if **s** is subsequence of **t**.
>
> A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).
>
> **Follow up:**
> If there are lots of incoming S, say S1, S2, ... , Sk where k >= 1B, and you want to check one by one to see if T has its subsequence. In this scenario, how would you change your code?
>
> **Credits:**
> Special thanks to [@pbrother](https://leetcode.com/pbrother/) for adding this problem and creating all test cases.
>
>  
>
> **Example 1:**
>
> ```
> Input: s = "abc", t = "ahbgdc"
> Output: true
> ```
>
> **Example 2:**
>
> ```
> Input: s = "axc", t = "ahbgdc"
> Output: false
> ```
>
>  
>
> **Constraints:**
>
> - `0 <= s.length <= 100`
> - `0 <= t.length <= 10^4`
> - Both strings consists only of lowercase characters.

* the general idea for the easy mode is use Two pointers to check is or not Subsequence 
* But when we meet many ```S``` String ,but T is the only one , if we use Two pointers ,we will waste many time on iterate the T String .So we need to record the index Character in the T String  .To record the index, we have some ways
  * use Map 
  * use Array because the Character in T is Lower Case Character

### Solution One S

```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        // check s is t's subsequence or not 
        int pS = 0 ;
        int pT = 0 ;
        while( pS < s.length() && pT< t.length()){
            if(s.charAt(pS)==t.charAt(pT)){
                pS++;
                pT++;
            }else{
                pT++;
            }
        }
        return pS== s.length();
    }
}
```

## Solution 

* In the second condition ,we need to Search the greater index in the Character
* so we need to record the pre index and  use binary Search 

```java
 // Follow-up: O(N) time for pre-processing, O(Mlog?) for each S.
    // Eg-1. s="abc", t="bahbgdca"
    // idx=[a={1,7}, b={0,3}, c={6}]
    //  i=0 ('a'): prev=1
    //  i=1 ('b'): prev=3
    //  i=2 ('c'): prev=6 (return true)
    // Eg-2. s="abc", t="bahgdcb"
    // idx=[a={1}, b={0,6}, c={5}]
    //  i=0 ('a'): prev=1
    //  i=1 ('b'): prev=6
    //  i=2 ('c'): prev=? (return false)
    public boolean isSubsequence(String s, String t) {
        List<Integer>[] idx = new List[256]; // Just for clarity
        for (int i = 0; i < t.length(); i++) {
            if (idx[t.charAt(i)] == null)
                idx[t.charAt(i)] = new ArrayList<>();
            idx[t.charAt(i)].add(i);
        }
        
        int prev = 0;
        for (int i = 0; i < s.length(); i++) {
            // Note: char of S does NOT exist in T causing NPE
            if (idx[s.charAt(i)] == null) return false; 
            int j = Collections.binarySearch(idx[s.charAt(i)], prev);
            if (j < 0) j = -j - 1;
            if (j == idx[s.charAt(i)].size()) return false;
            //prev++
            prev = idx[s.charAt(i)].get(j) + 1;
        }
        return true;
    }
```

