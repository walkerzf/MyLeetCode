## Permutation in String

> Given two strings **s1** and **s2**, write a function to return true if **s2** contains the permutation of **s1**. In other words, one of the first string's permutations is the **substring** of the second string.
>
>  
>
> **Example 1:**
>
> ```
> Input: s1 = "ab" s2 = "eidbaooo"
> Output: True
> Explanation: s2 contains one permutation of s1 ("ba").
> ```
>
> **Example 2:**
>
> ```
> Input:s1= "ab" s2 = "eidboaoo"
> Output: False
> ```
>
>  
>
> **Note:**
>
> 1. The input strings only contain lower case letters.
> 2. The length of both given strings is in range [1, 10,000].

## Solution

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int cnt []=new int[50];
        int len=s1.length();
        int count=0;
        for(int i=0;i<len;i++){
            cnt[s1.charAt(i)-'a']++;
            count++;
        }
        int l=0;
        int r=0;
        count=len;
        while(r<s2.length()){
            if(cnt[s2.charAt(r)-'a']>0) count--;
            cnt[s2.charAt(r)-'a']--;
            r++;
            if(count==0) return true;
            if(r-l==len){
                if(cnt[s2.charAt(l)-'a']>=0){
                    count++;           
                } 
            cnt[s2.charAt(l)-'a']++;
            l++;
        }
        }
        return false;
    }
}
```

