## Contains Duplicate III

Solution

Given an array of integers, find out whether there are two distinct indices *i* and *j* in the array such that the **absolute** difference between **nums[i]** and **nums[j]** is at most *t* and the **absolute** difference between *i* and *j* is at most *k*.

**Example 1:**

```
Input: nums = [1,2,3,1], k = 3, t = 0
Output: true
```

**Example 2:**

```
Input: nums = [1,0,1,1], k = 1, t = 2
Output: true
```

**Example 3:**

```
Input: nums = [1,5,9,1,5,9], k = 2, t = 3
Output: false
```

  Hide Hint #1 

Time complexity O(n logk) - This will give an indication that sorting is involved for k elements.

  Hide Hint #2 

Use already existing state to evaluate next state - Like, a set of k sorted numbers are only needed to be tracked. When we are processing the next number in array, then we can utilize the existing sorted state and it is not necessary to sort next overlapping set of k numbers again.



* Brute Force TLE 
* so we need to maintain a window includes k numbers ,in this window ,we find the pair numbers that satisfied the situation .So when meet a number , we need to find [ number - t,number +t] , .The bucket comes into my mind .We can design the bucket size according to  ```t ``` ,when we meet a new number ,the corresponding bucket have a number ,so must find the pairs ,If not ,we need to search the adject bucket .When the bucket size > k ,we need to remove the first element 



## Solution Brute Force TLE

## Solution Bucket

* the corner case
  * the minus number 
  *  the INT_MAX or INTMIN overflow condition 

```c++
class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int> &nums, int k, int t) {
        if (k < 1 || t < 0) return false;
        unordered_map<int, int> bucket;
        for (int i = 0; i < nums.size(); i++) {
            int bucket_idx = nums[i] / (long(t) + 1);
            if(nums[i]<0) --bucket_idx;
            if(bucket.find(bucket_idx)!=bucket.end()) return true;
            bucket[bucket_idx] = nums[i];
            if(bucket.find(bucket_idx-1)!=bucket.end()&&(abs(long(nums[i])-bucket[bucket_idx-1])<=t)) return true;
            if(bucket.find(bucket_idx+1)!=bucket.end()&&(abs(long(nums[i])-bucket[bucket_idx+1])<=t)) return true;
            if(bucket.size()>k){
                 int key_to_remove = nums[i-k]/(long(t)+1);
                 if(nums[i-k]<0)  --key_to_remove;
                 bucket.erase(key_to_remove);
            }
        }
        return false;
    }
};
```

## SET

```c++
 bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
    set<int> window; // set is ordered automatically 
    for (int i = 0; i < nums.size(); i++) {
        if (i > k) window.erase(nums[i-k-1]); // keep the set contains nums i j at most k
        // |x - nums[i]| <= t  ==> -t <= x - nums[i] <= t;
        auto pos = window.lower_bound(nums[i] - t); // x-nums[i] >= -t ==> x >= nums[i]-t 
        // x - nums[i] <= t ==> |x - nums[i]| <= t    
        if (pos != window.end() && *pos - nums[i] <= t) return true;
        window.insert(nums[i]);
    }
    return false;
}
```

