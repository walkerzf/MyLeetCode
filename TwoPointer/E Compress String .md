## 题干

> Implement a method to perform basic string compression using the counts of repeated characters. For example, the string aabcccccaaa would become a2blc5a3. If the "compressed" string would not become smaller than the original string, your method should return the original string. You can assume the string has only uppercase and lowercase letters (a - z).
>
> Example 1:
>
> Input: "aabcccccaaa"
> Output: "a2b1c5a3"
> Example 2:
>
> Input: "abbccd"
> Output: "abbccd"
> Explanation: 
> The compressed string is "a1b2c2d1", which is longer than the original string.
>
>
> Note:
>
> 0 <= S.length <= 50000

* 题目上的```smaller than`` 值得注意，新生成的长度大于等于他都会返回原串

## Solution 遍历

```java
//time 3ms 39.4MB

class Solution {
    public String compressString(String S) {
        if(S.length()==0) return "";
        StringBuilder res=new StringBuilder();
        char s[]=S.toCharArray();
        int count =1;
        for(int i=1;i<s.length;i++){
            // if(i-1<0){
            //   //  res.append(s[i]);
            //     count++;
            //     continue;
            // }
            if(s[i]==s[i-1]){
                count++;
                continue;
            }
            if(s[i]!=s[i-1]){
                res.append(s[i-1]);
                res.append(count);
                count=1;
                continue;
            }
        }
        res.append(s[S.length()-1]);
        res.append(count);
        if(res.length()>=S.length()) return  S;
        else return  res.toString();
    }
}
```

