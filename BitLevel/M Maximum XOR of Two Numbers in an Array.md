## Maximum XOR of Two Numbers in an Array

Solution

Given an integer array `nums`, return *the maximum result of `nums[i] XOR nums[j]`*, where `0 ≤ i ≤ j < n`.

**Follow up:** Could you do this in `O(n)` runtime?

**Example 1:**

```
Input: nums = [3,10,5,25,2,8]
Output: 28
Explanation: The maximum result is 5 XOR 25 = 28.
```

**Example 2:**

```
Input: nums = [0]
Output: 0
```

**Example 3:**

```
Input: nums = [2,4]
Output: 6
```

**Example 4:**

```
Input: nums = [8,10,2]
Output: 10
```

**Example 5:**

```
Input: nums = [14,70,53,83,49,91,36,80,92,51,66,70]
Output: 127
```

 

**Constraints:**

- `1 <= nums.length <= 2 * 104`

- `0 <= nums[i] <= 231 - 1`

题目要求 O(N)O(N) 时间复杂度，下面会讨论两种典型的 O(N)O(N) 复杂度解法。
* 利用哈希集合存储按位前缀。

* 利用字典树存储按位前缀。

这两种解法背后的思想是一样的，都是先将整数转化成二进制形式，再从最左侧的比特位开始逐一处理来构建最大异或值。两个方法的不同点在于采用了不同的数据结构来存储按位前缀。第一个方法在给定的测试集下执行速度更快，但第二种方法更加普适，更加简单。

## Solution prefix Bits

```c++
class Solution {
public:
    int findMaximumXOR(vector<int> &nums) {
        int len = nums.size();
        int m = 0;
        for (int i = 0; i < len; i++) m = max(m, nums[i]);
        //calculate the longest bits
        int bits = 0;
        int mask = 0;
        while (bits<32&&mask <= m) {
            mask = (1 << bits);
            bits++;
        }
        bits--;
        //maxXor is result
        int maxXor = 0;
        //curXor is cur max
        int curXor = 0;
        for(int i = bits-1;i>=0;i--){
            maxXor<<=1;
            curXor = maxXor|1;
            unordered_set<int> prefixbits;
            //save the possible prefix 
            for(int num:nums) prefixbits .insert(num>>i);
            //check the all prefix  to generate current bits 1 
            for(int p :prefixbits){
                if(prefixbits.find(p^curXor)!=prefixbits.end()){
                    maxXor = curXor;
                    break;
                }
            }
        }
        return maxXor;
    }
};
```

## Solution Trie

