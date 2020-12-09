# Repeated DNA Sequences

All DNA is composed of a series of nucleotides abbreviated as `'A'`, `'C'`, `'G'`, and `'T'`, for example: `"ACGAATTCCG"`. When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.

Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule

**Example 1:**

```
Input: s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"
Output: ["AAAAACCCCC","CCCCCAAAAA"]
```

**Example 2:**

```
Input: s = "AAAAAAAAAAAAA"
Output: ["AAAAAAAAAA"]
```

**Constraints:**

- `0 <= s.length <= 105`
- `s[i]` is `'A'`, `'C'`, `'G'`, or `'T'`.

## Solution

 Unordered_map

```c++
class Solution {
public:
    vector<string> findRepeatedDnaSequences(string s) {
        vector<string> res;
        if(s.length()<10) return res;
        unordered_map<string,int> dic;
        for(int i = 0;i<=s.length()-10;i++){
            dic[s.substr(i,10)]++;
        }
        for(auto it = dic.begin();it!=dic.end();it++){
            if(it->second>1){
                res.push_back(it->first);
            }
        }
        return res;
    }
};
```

## Solution Bit Level

we need to focus on the `ascii` about A G C T

A is 0x41, C is 0x43, G is 0x47, T is 0x54. Still don't see it? Let me write it in octal.

A is 0101, C is 0103, G is 0107, T is 0124. The last digit in octal are different for all four letters. That's all we need!

We can simply use `s[i] & 7` to get the last digit which are just the last 3 bits

```c++
vector<string> findRepeatedDnaSequences(string s) {
    unordered_map<int, int> m;
    vector<string> r;
    int t = 0, i = 0, ss = s.size();
    while (i < 9)
        t = t << 3 | s[i++] & 7;
    while (i < ss)
        if (m[t = t << 3 & 0x3FFFFFFF | s[i++] & 7]++ == 1)
            r.push_back(s.substr(i - 10, 10));
    return r;
}
```

