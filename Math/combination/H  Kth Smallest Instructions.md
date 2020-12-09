## [5600. Kth Smallest Instructions](https://leetcode-cn.com/problems/kth-smallest-instructions/)

Bob is standing at cell `(0, 0)`, and he wants to reach `destination`: `(row, column)`. He can only travel **right** and **down**. You are going to help Bob by providing **instructions** for him to reach `destination`.

The **instructions** are represented as a string, where each character is either:

- `'H'`, meaning move horizontally (go **right**), or
- `'V'`, meaning move vertically (go **down**).

Multiple **instructions** will lead Bob to `destination`. For example, if `destination` is `(2, 3)`, both `"HHHVV"` and `"HVHVH"` are valid **instructions**.

However, Bob is very picky. Bob has a lucky number `k`, and he wants the `kth` **lexicographically smallest instructions** that will lead him to `destination`. `k` is **1-indexed**.

Given an integer array `destination` and an integer `k`, return *the* `kth` ***lexicographically smallest instructions** that will take Bob to* `destination`.

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/12/ex1.png)

```
Input: destination = [2,3], k = 1
Output: "HHHVV"
Explanation: All the instructions that reach (2, 3) in lexicographic order are as follows:
["HHHVV", "HHVHV", "HHVVH", "HVHHV", "HVHVH", "HVVHH", "VHHHV", "VHHVH", "VHVHH", "VVHHH"].
```

**Example 2:**

**![img](https://assets.leetcode.com/uploads/2020/10/12/ex2.png)**

```
Input: destination = [2,3], k = 2
Output: "HHVHV"
```

**Example 3:**

**![img](https://assets.leetcode.com/uploads/2020/10/12/ex3.png)**

```
Input: destination = [2,3], k = 3
Output: "HHVVH"
```



**Constraints:**

- `destination.length == 2`
- `1 <= row, column <= 15`
- `1 <= k <= nCr(row + column, row)`, where `nCr(a, b)` denotes `a` choose `b`.

## Solution  Combination

**组合数的递推公式**
`C [n] [k] = C[n-1] [k-1] + C[n-1] [k]`

```c++
class Solution {
public:
    string kthSmallestPath(vector<int>& d, int k) {
        int h = d[1];
        int v = d[0];
        //store the combination number
        vector<vector<int>> comb(h+v,vector<int>(h+v,0));
        comb[0][0] = 1;
        for(int i =1 ;i<h+v;i++){
            comb[i][0] = 1 ;
            for(int j =1 ;j<=i;j++){
                comb[i][j] = comb[i-1][j-1]+ comb[i-1][j];
            }
        }
        string ans;
        int s = h+v;
        for(int i = 1;i<= s;i++){
            if(h>0){
            if(k>comb[s-i][h-1]){
                k-=comb[s-i][h-1];
                ans+="V";
                v--;
            }else{
                ans+="H";
                h--;
            }
            }else{
                ans+="V";
                v--;
            }
        }
        return ans;
    }
};
```

