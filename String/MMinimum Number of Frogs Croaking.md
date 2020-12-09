## Minimum Number of Frogs Croaking

> Given the string `croakOfFrogs`, which represents a combination of the string "croak" from different frogs, that is, multiple frogs can croak at the same time, so multiple “croak” are mixed. *Return the minimum number of* different *frogs to finish all the croak in the given string.*
>
> A valid "croak" means a frog is printing 5 letters ‘c’, ’r’, ’o’, ’a’, ’k’ **sequentially**. The frogs have to print all five letters to finish a croak. If the given string is not a combination of valid "croak" return -1.
>
>  
>
> **Example 1:**
>
> ```
> Input: croakOfFrogs = "croakcroak"
> Output: 1 
> Explanation: One frog yelling "croak" twice.
> ```
>
> **Example 2:**
>
> ```
> Input: croakOfFrogs = "crcoakroak"
> Output: 2 
> Explanation: The minimum number of frogs is two. 
> The first frog could yell "crcoakroak".
> The second frog could yell later "crcoakroak".
> ```
>
> **Example 3:**
>
> ```
> Input: croakOfFrogs = "croakcrook"
> Output: -1
> Explanation: The given string is an invalid combination of "croak" from different frogs.
> ```
>
> **Example 4:**
>
> ```
> Input: croakOfFrogs = "croakcroa"
> Output: -1
> ```
>
>  
>
> **Constraints:**
>
> - `1 <= croakOfFrogs.length <= 10^5`
> - All characters in the string are: `'c'`, `'r'`, `'o'`, `'a'` or `'k'`.

## Solution

* 这个办法也太笨了

```java
//tiem 400ms 
        class Solution {
            char c[]=new char[]{'c','r','o','a','k'};
            public int minNumberOfFrogs(String croakOfFrogs) {
                Map<Character ,Integer> m=new HashMap<>();
                m.put('c',0);m.put('r',0);m.put('o',0);m.put('a',0);m.put('k',0);
                int count=0;
                for(int i=0;i<croakOfFrogs.length();i++){
                    char c=croakOfFrogs.charAt(i);
                    m.put(c,m.get(c)+1);
                    if(isVaild(m)==false) return -1;
                    int n=min(m);
                    delete(n,m);
                    int mm=max(m);
                    count=Math.max(count,mm);
                }
                boolean vaild=true;
                for(Character c:m.keySet()){
                    if(m.get(c)!=0) return -1;
                }
                return count;
            }
            private boolean isVaild(Map<Character ,Integer>m ){
                int pre=m.get('c');
                for(int i=1;i<5;i++){
                        if(pre<m.get(c[i])) return false;
                        pre=m.get(c[i]);
                }
                return true;
            }
            private  void delete(int  common,Map<Character ,Integer> m){
                for(Character c:m.keySet()){
                    m.put(c,m.get(c)-common);
                }
            }
            private int max(Map<Character,Integer> m){
                int da=Integer.MIN_VALUE;
                for(Character c:m.keySet()){
                    if(da<m.get(c)){
                        da=m.get(c);
                    }
                }
                return da;
            }
            private int min(Map<Character, Integer> m){
                int xiao=Integer.MAX_VALUE;
                for(Character c:m.keySet()){
                    if(xiao>m.get(c)){
                        xiao=m.get(c);
                    }
                }
                return xiao;
            }
        }
```

* 因为先来的一定是c ，如果能判断合法的话，最大值一定是c字符，最小的一定是k字符 ，很明显

```c++
	class Solution {
public:
    int minNumberOfFrogs(string croakOfFrogs) {
        int c = 0, r = 0, o = 0, a = 0, k = 0, ans = 0;
        for(char x : croakOfFrogs){
            if(x == 'c') c += 1;
            else if(x == 'r') r += 1;
            else if(x == 'o') o += 1;
            else if(x == 'a') a += 1;
            else if(x == 'k') k += 1;
            else return -1;
            if(r > c or o > r or a > o or k > a) return -1;
            ans = max(c, ans);
            if(k == 1){
                c -= 1;
                r -= 1;
                o -= 1;
                a -= 1;
                k -= 1;
            }
        }
        if(c) return -1;
        return ans;
    }
};
```

