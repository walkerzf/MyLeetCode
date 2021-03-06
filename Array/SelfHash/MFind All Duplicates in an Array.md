## Find All Duplicates in an Array

> Given an array of integers, 1 ≤ a[i] ≤ *n* (*n* = size of array), some elements appear **twice** and others appear **once**.
>
> Find all the elements that appear **twice** in this array.
>
> Could you do it without extra space and in O(*n*) runtime?
>
> 
>
> **Example:**
>
> ```
> Input:
> [4,3,2,7,8,2,3,1]
> 
> Output:
> [2,3]
> ```

## SelfHash

```c++
class Solution {
public:
    vector<int> findDuplicates(vector<int>& nums) {
       int len = nums.size();
        vector<int> res;  
        if(len==0) return res;
         for(int i=0 ; i<len;i++){
             int index = abs(nums[i])-1;
             if(nums[index]<0) res.push_back(index+1);
             else nums[index] = -nums[index];
        }
        return res;
    }
};
```

