## Longest Duplicate Substring

> Given a string `S`, consider all *duplicated substrings*: (contiguous) substrings of S that occur 2 or more times. (The occurrences may overlap.)
>
> Return **any** duplicated substring that has the longest possible length. (If `S` does not have a duplicated substring, the answer is `""`.)
>
>  
>
> **Example 1:**
>
> ```
> Input: "banana"
> Output: "ana"
> ```
>
> **Example 2:**
>
> ```
> Input: "abcd"
> Output: ""
> ```
>
>  
>
> **Note:**
>
> 1. `2 <= S.length <= 10^5`
> 2. `S` consists of lowercase English letters.

## Solution Binary Search + Rabin-Karp 's algorithm.

* the possible answer range [0,length - 1]
* if we find the possible answer is mid , so the answer must be in [mid,length -1],or the answer in the [0,mid -1]
* the range of Binary Search is vert important ,because the improper range will  get **TLE** 

Rabin-Karp 's algorithm.

Ref:

[Template](https://www.geeksforgeeks.org/java-program-for-rabin-karp-algorithm-for-pattern-searching/)

[Leetcode  官方解答](https://leetcode-cn.com/problems/longest-duplicate-substring/solution/zui-chang-zhong-fu-zi-chuan-by-leetcode/)

[Lee’s Solution](https://leetcode.com/problems/longest-duplicate-substring/discuss/290871/Python-Binary-Search)

[https://en.wikipedia.org/wiki/Rabin%E2%80%93Karp_algorithm](https://en.wikipedia.org/wiki/Rabin–Karp_algorithm)

* the``` myHash``` function similar to ```M 面试题 17.13. Re-Space LCCI``` ,the latter is  much simpler 

> The [Naive String Matching](https://www.geeksforgeeks.org/searching-for-patterns-set-1-naive-pattern-searching/) algorithm slides the pattern one by one. After each slide, it one by one checks characters at the current shift and if all characters match then prints the match.
> Like the Naive Algorithm, Rabin-Karp algorithm also slides the pattern one by one. But unlike the Naive algorithm, Rabin Karp algorithm matches the hash value of the pattern with the hash value of current substring of text, and if the hash values match then only it starts matching individual characters. So Rabin Karp algorithm needs to calculate hash values for following strings.

In this Solution ,we calculate the biggest Mod ,to avoid hash collision 

* To avoid hash collision   we can use a hash map to record the index of string. And we interate the index in the map to test the same substring 

```java
class Solution {
    public String longestDupSubstring(String S) {
        //Binary Search
        int l = 1;
        int r = S.length()-1;
        int num [] =new int [S.length()];
        for(int i=0;i<num.length;i++){
            num[i]=S.charAt(i)-'a';
        }
        long modulus = (long)Math.pow(2, 32);
        int base = 26;
        int index =0;
        while(l<=r){
            int mid = (r-l)/2+l;
            int res =searchIndex(S,mid,num,modulus,base);
            if(res<0) r = mid-1;
            else {
                l=mid+1;
                index =res;
            }
        }
        int start =searchIndex(S,l-1,num,modulus,base);
        if(start<0) return "";
        return S.substring(start,start+l-1);
    }
    
    
    //Rabin-karp's Algorithm function 
    
    private  int searchIndex(String str, int len,int []num , long modulus ,int base){
        long window = 0;
        for(int i=0;i<len;i++){
            window =(window*base+num[i])%modulus;
        }
        Set<Long> seen = new HashSet<>();
        seen.add(window);
        long h =1;
        // to avoid overflow 
        for(int i=1;i<=len;i++) h =(h*base)%modulus;
        for(int start =1 ;start<=num.length-len;start++){
            //to avoid overflow 
            window =(window*base -num[start-1]*h%modulus+modulus)%modulus;
            window =(window+num[start+len-1])%modulus;
            if(seen.contains(window)){
                return start;
            }else seen.add(window);
        }
        return -1;
    }
}

```

we can calculate the hash code, but with consideration about the hash ```collision ````,wo create ```Map <Long , List<Index>>```

```java
class Solution {
    public String longestDupSubstring(String S) {
        // edge case
        if (S == null) {
            return null;
        }
        // binary search the max length
        int min = 0;
        int max = S.length() - 1;
        int mid;
        while (min < max - 1) {
            mid = (min + max) / 2;
            if (searchForLength(S, mid) != null) {
                min = mid;
            } else {
                max = mid - 1;
            }
        }
        String str = searchForLength(S, max);
        if (str != null) {
            return str;
        } else {
            return searchForLength(S, min);
        }
    }
    
    String searchForLength(String str, int len) {
        // rolling hash method
        if (len == 0) {
            return "";
        } else if (len >= str.length()) {
            return null;
        }
        Map<Long, List<Integer>> map = new HashMap<>();    // hashcode -> list of all starting idx with identical hash
        long p = (1 << 31) - 1;  // prime number
        long base = 256;
        long hash = 0;
        for (int i = 0; i < len; ++i) {
            hash = (hash * base + str.charAt(i)) % p;
        }
        long multiplier = 1;
        for (int i = 1; i < len; ++i) {
            multiplier = (multiplier * base) % p;
        }
        // first substring
        List<Integer> equalHashIdx = new ArrayList<Integer>();
        equalHashIdx.add(0);
        map.put(hash, equalHashIdx);
        // other substrings
        int from = 0;
        int to = len;
        while (to < str.length()) {
            hash = ((hash + p - multiplier * str.charAt(from++) % p) * base + str.charAt(to++)) % p;
            equalHashIdx = map.get(hash);
            if (equalHashIdx == null) {
                equalHashIdx = new ArrayList<Integer>();
                map.put(hash, equalHashIdx);
            } else {
                for (int i0: equalHashIdx) {
                    if (str.substring(from, to).equals(str.substring(i0, i0 + len))) {
                        return str.substring(i0, i0 + len);
                    }
                }
            }
            equalHashIdx.add(from);
        }
        return null;
    }
}
```

```java
// compute hash for key[0..m-1]
private long hash(String key, int m) {
    long h = 0 ; 
    for ( int j = 0 ; j < m ; j ++ ) {
        h = (R * h + key.charAt(j)) % q ; 
    }
    return h ; 
}

private boolean compare(String s, int i1, int i2, int m) {
    for ( int i = 0 ; i < m ; i ++ ) {
        if ( s.charAt(i1+i) != s.charAt(i2+i) ) return false ; 
    }
    return true ; 
}

private int test(String s, int m) {
    Map<Long, List<Integer>> map = new HashMap<>() ; 
    long h = hash(s, m) ; 
    map.put(h, new ArrayList<>()) ; 
    map.get(h).add(0) ; 
    
    long RM = 1 ; // R^(m-1) % q
    for ( int i = 1 ; i <= m-1 ; i ++ ) {
        RM = (R * RM) % q ; 
    }
   
    // NOTE i is the ending index of current string
    for ( int i = m ; i < s.length() ; i ++ ) {
        // remove leading digit, add trailing digit, check for match
        h = ( h + q - RM*s.charAt(i-m)%q ) % q ;
        h = ( h * R + s.charAt(i) ) % q ;
        int startIndex = i - m + 1 ; 
        if ( map.containsKey(h) ) {
            for ( int prev: map.get(h) ) {
                if ( compare(s, startIndex, prev, m) ) return startIndex ; 
            }
        }
        else {
            map.put(h, new ArrayList<>()) ; 
        }
        map.get(h).add(startIndex) ;             
    }
    
    return -1 ; 
}

public String longestDupSubstring(String S) {
    int lo = 0, hi = S.length() ; 
    // binary search to find SMALLEST length of string that cannot pass test
    while ( lo < hi ) {
        int mid = lo + (hi-lo)/2 ; 
        int index = test(S, mid) ; 
        if ( index < 0 ) hi = mid ;  
        else {
            lo = mid + 1 ;
        }
    }
    // lo-1 is the LARGEST length of string that CAN pass test
    int checkLen = lo-1 ; 
    if ( checkLen <= 0 ) return "" ; 
    int start = test(S, checkLen) ; 
    return S.substring(start, start + checkLen) ; 
}
```