#### [480. Sliding Window Median](https://leetcode-cn.com/problems/sliding-window-median/)

Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

Examples:

```
[2,3,4]` , the median is `3
[2,3]`, the median is `(2 + 3) / 2 = 2.5
```

Given an array *nums*, there is a sliding window of size *k* which is moving from the very left of the array to the very right. You can only see the *k* numbers in the window. Each time the sliding window moves right by one position. Your job is to output the median array for each window in the original array.

For example,
Given *nums* = `[1,3,-1,-3,5,3,6,7]`, and *k* = 3.

```
Window position                Median
---------------               -----
[1  3  -1] -3  5  3  6  7       1
 1 [3  -1  -3] 5  3  6  7       -1
 1  3 [-1  -3  5] 3  6  7       -1
 1  3  -1 [-3  5  3] 6  7       3
 1  3  -1  -3 [5  3  6] 7       5
 1  3  -1  -3  5 [3  6  7]      6
```

Therefore, return the median sliding window as `[1,-1,-1,3,5,6]`.

**Note:**
You may assume `k` is always valid, ie: `k` is always smaller than input array's size for non-empty array.
Answers within `10^-5` of the actual value will be accepted as correct.

## Solution STL Solution

* the special ds is `multiset` ,can have duplications and sorted  
* we maintain the `mid` iteration ,so we can easily get the median

```c++
class Solution {
public:
    vector<double> medianSlidingWindow(vector<int> &nums, int k) {
        vector<double> res;
        multiset<int> window(nums.begin(), nums.begin() + k);
        auto mid = next(window.begin(), k / 2);
        for (int i = k; i <= nums.size(); i++) {
            double m = ((*mid) * 1.0 + (*prev(mid, 1 - k % 2)) * 1.0) / 2.0;
            res.push_back(m);
            if( i ==nums.size()) break;
            //add element
            window.insert(nums[i]);
            if(nums[i]<*mid){
                mid--;
            }
            if(nums[i-k]<=*mid){
                mid++;
            }
            //remove element
            window.erase(window.lower_bound(nums[i-k]));
        }
        return res;
    }
};
```

## Solution Two priorityQueue

* we maintain two `priority queue` to have the former part and later part of the window ,and we let the former have k/2 +1 elements ,the later have k/2 elements
* we maintain a `balance ` variable , when the former part have one more element balance ++,  or the balance –,After we insert  the element , we rebalance two `priority queue` 
* we delay the deletion of the element ,when the deletion element on the top of the queue ,we `pop` it.

```c++
class Solution {
public:
    vector<double> medianSlidingWindow(vector<int> &nums, int k) {
        vector<double> medians;                                 // record res
        unordered_map<int, int> label;                     // record the numbers need to delete
        priority_queue<int> lo;                                 // max heap , record the former part
        priority_queue<int, vector<int>, greater<int> > hi;     // min heap , record the later part
        for (int i = 0; i < k; i++) {
            lo.push(nums[i]);
        }
        for (int i = 0; i < k / 2; i++) {
            int n = lo.top();
            lo.pop();
            hi.push(n);
        }
        for (int i = k; i <= nums.size(); i++) {
            if (k % 2 == 0) {
               // cout << lo.top() << hi.top() << endl;
                medians.push_back((lo.top() * 1.0 + hi.top() * 1.0) / 2.0);
            } else medians.push_back(lo.top());
            if (i == nums.size()) return medians;
            int deletenum = nums[i-k];
            int innum = nums[i];
            int balance = 0;
            label[deletenum]++;
            if(deletenum<=lo.top()){
                balance--;
            }else balance++;
            
            if(innum<=lo.top()){
                balance++;
                lo.push(innum);
            }else{
                balance--;
                hi.push(innum);
            }

            if(balance>0) {
                int nn = lo.top();
                lo.pop();
                hi.push(nn);
                balance --;
            } else if(balance<0){
                int nn = hi.top();
                lo.push(nn);
                hi.pop();
                balance++;
            }
            while(!lo.empty()&&label[lo.top()]>0){
                label[lo.top()]--;
                lo.pop();
            }
            while(!hi.empty()&&label[hi.top()]>0){
                label[hi.top()]--;
                hi.pop();
            }

        }
        return medians;
    }
};
```

