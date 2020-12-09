## [Number of Ways to Reorder Array to Get Same BST](https://leetcode-cn.com/problems/number-of-ways-to-reorder-array-to-get-same-bst/)

> Given an array nums that represents a permutation of integers from 1 to n. We are going to construct a binary search tree (BST) by inserting the elements of nums in order into an initially empty BST. Find the number of different ways to reorder nums so that the constructed BST is identical to that formed from the original array nums.
>
> For example, given nums = [2,1,3], we will have 2 as the root, 1 as a left child, and 3 as a right child. The array [2,3,1] also yields the same BST but [3,2,1] yields a different BST.
>
> Return the number of ways to reorder nums such that the BST formed is identical to the original BST formed from nums.
>
> Since the answer may be very large, return it modulo 10^9 + 7.
>
>  ![img](https://assets.leetcode.com/uploads/2020/08/12/bb.png)
>
> ```
> Example 1:
> 
> 
> 
> Input: nums = [2,1,3]
> Output: 1
> Explanation: We can reorder nums to be [2,3,1] which will yield the same BST. There are no other ways to reorder nums which will yield the same BST.
> Example 2:
> 
> 
> ```
>
> ![img](https://assets.leetcode.com/uploads/2020/08/12/ex1.png)
>
> ```
> Input: nums = [3,4,5,1,2]
> Output: 5
> Explanation: The following 5 arrays will yield the same BST: 
> [3,1,2,4,5]
> [3,1,4,2,5]
> [3,1,4,5,2]
> [3,4,1,2,5]
> [3,4,1,5,2]
> Example 3:
> 
> 
> 
> Input: nums = [1,2,3]
> Output: 0
> Explanation: There are no other orderings of nums that will yield the same BST.
> Example 4:
> 
> 
> 
> Input: nums = [3,1,2,5,4,6]
> Output: 19
> Example 5:
> 
> Input: nums = [9,4,2,1,3,6,5,7,8,14,11,10,12,13,16,15,17,18]
> Output: 216212978
> Explanation: The number of ways to reorder nums to get the same BST is 3216212999. Taking this number modulo 10^9 + 7 gives 216212978.
> 
> 
> Constraints:
> 
> 1 <= nums.length <= 1000
> 1 <= nums[i] <= nums.length
> All integers in nums are distinct.
> ```

## Solution

* Point ： 
  * use long long to avoid overflow
  * when we know the root , the left Tree will be compose of the value < root val . So does the right tree.And the left tree value position with the right value will not impact the result . So we need to pick  ``` n ``` from ```m```  (Permutation) , and the n will have same permutations using the same function 
  * care about the    mulitply mod opearation 
  * need pre calculation about the permutations 

```c++
vector<vector<long long >> C;
const long long MOD = 1000000007;

class Solution {
public:
    int numOfWays(vector<int> &nums) {
        int len = nums.size();
        C = vector<vector<long long >>(len + 1, vector<long long>(len + 1, 0));
        for (int i = 0; i <= len; i++) {
            for (int j = 0; j <= i; j++) {
                if (j == 0) C[i][j] = 1;
                else C[i][j] = (C[i - 1][j - 1] + C[i - 1][j]) % MOD;
            }
        }
        return (int) ((dfs(nums) + MOD - 1) % MOD);
    }

    long long dfs(vector<int> &n) {
        int len = n.size();
        if(len==0) return  1 ;
        if (len == 1) return 1;
        int root = n[0];
        vector<int> left;
        vector<int> right;
        for (int number : n) {
            if (number > root) right.push_back(number);
            else if (number < root) left.push_back(number);
        }
        return ((C[len - 1][left.size()]) % MOD * ((dfs(left) % MOD * dfs(right) % MOD) % MOD)%MOD) % MOD;
    }
};
```

