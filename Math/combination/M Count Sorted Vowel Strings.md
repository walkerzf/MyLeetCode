## [5555. Count Sorted Vowel Strings](https://leetcode-cn.com/problems/count-sorted-vowel-strings/)

Given an integer `n`, return *the number of strings of length* `n` *that consist only of vowels (*`a`*,* `e`*,* `i`*,* `o`*,* `u`*) and are **lexicographically sorted**.*

A string `s` is **lexicographically sorted** if for all valid `i`, `s[i]` is the same as or comes before `s[i+1]` in the alphabet.

**Example 1:**

```
Input: n = 1
Output: 5
Explanation: The 5 sorted strings that consist of vowels only are ["a","e","i","o","u"].
```

**Example 2:**

```
Input: n = 2
Output: 15
Explanation: The 15 sorted strings that consist of vowels only are
["aa","ae","ai","ao","au","ee","ei","eo","eu","ii","io","iu","oo","ou","uu"].
Note that "ea" is not a valid string since 'e' comes after 'a' in the alphabet.
```

**Example 3:**

```
Input: n = 33
Output: 66045
```

**Constraints:**

- `1 <= n <= 50` 

## Solution Combination

先假设给aeiou五个字母分配一个“虚”的位置（这样就相当于有n+5个小球需要分配），然后应用隔板法时保证每个字母至少分配到一个位置（“虚”的位置保证每个字母必须分配到一个），插完隔板后再把“虚的位置”去掉。那就是从n+4个放置隔板的位置中选择4个

```c++
class Solution {
public:
    int countVowelStrings(int n) {
        return (n + 4) * (n + 3) * (n + 2) * (n + 1) / 24;
    }
};


```



## Solution DFS

```c++
class Solution {
public:
    int countVowelStrings(int n) {
        return dfs(n , 5);
    }
    int dfs(int n ,int leftn){
        if(n==0) return 1;
        if(leftn==1) return 1;
        int res = 0;
        for(int i = 1;i<=leftn;i++){
            res+=dfs(n-1,leftn-i+1);
        }
        return res;
    }
};
```

## Solution DP

```c++
class Solution {
    int f[55][5];
public:
    int countVowelStrings(int n) {
        n++;
        int i,j,k;
        memset(f,0,sizeof(f));
        for(i=0;i<5;i++)f[1][i]=1;
        for(i=2;i<=n;i++)for(j=0;j<5;j++)for(k=0;k<=j;k++)f[i][j]+=f[i-1][k];
        return f[n][4];
    }
};
```

