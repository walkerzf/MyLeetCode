#### [5556. Furthest Building You Can Reach](https://leetcode-cn.com/problems/furthest-building-you-can-reach/)

难度中等1

You are given an integer array `heights` representing the heights of buildings, some `bricks`, and some `ladders`.

You start your journey from building `0` and move to the next building by possibly using bricks or ladders.

While moving from building `i` to building `i+1` (**0-indexed**),

- If the current building's height is **greater than or equal** to the next building's height, you do **not** need a ladder or bricks.
- If the current building's height is **less than** the next building's height, you can either use **one ladder** or `(h[i+1] - h[i])` **bricks**.

*Return the furthest building index (0-indexed) you can reach if you use the given ladders and bricks optimally.*

 

**Example 1:**

![img](https://assets.leetcode.com/uploads/2020/10/27/q4.gif)

```
Input: heights = [4,2,7,6,9,14,12], bricks = 5, ladders = 1
Output: 4
Explanation: Starting at building 0, you can follow these steps:
- Go to building 1 without using ladders nor bricks since 4 >= 2.
- Go to building 2 using 5 bricks. You must use either bricks or ladders because 2 < 7.
- Go to building 3 without using ladders nor bricks since 7 >= 6.
- Go to building 4 using your only ladder. You must use either bricks or ladders because 6 < 9.
It is impossible to go beyond building 4 because you do not have any more bricks or ladders.
```

**Example 2:**

```
Input: heights = [4,12,2,7,3,18,20,3,19], bricks = 10, ladders = 2
Output: 7
```

**Example 3:**

```
Input: heights = [14,3,19,3], bricks = 17, ladders = 0
Output: 3
```

 

**Constraints:**

- `1 <= heights.length <= 105`
- `1 <= heights[i] <= 106`
- `0 <= bricks <= 109`
- `0 <= ladders <= heights.length`

## Brute Force DFS



```c++
class Solution {
public:
    int res = 0;
    int len ;
    int furthestBuilding(vector<int>& heights, int bricks, int ladders) {
        len =  heights.size();
        res = 0;
        if(ladders==len) return len-1;
        dfs(heights,0,bricks,ladders);
        return res;
    }
    void dfs(vector<int>& heights, int index,int b, int l){
        if(res==len-1) return ;
        if(index==len-1){
            res =  len-1;
            return ;
        }
        res = max(res,index);
        if(heights[index+1]<=heights[index]) dfs(heights,index+1,b,l);
        else{
            int d = heights[index+1]-heights[index];
            if(b>=d) dfs(heights,index+1,b-d,l);
            if(l>0) dfs(heights,index+1,b,l-1);
        }
    }
};
```

## Solution Priority queue Greedy

```c++
class Solution {
public:
    int furthestBuilding(vector<int>& heights, int bricks, int ladders) {
        int n = heights.size();
        // 由于我们需要维护最大的 l 个值，因此使用小根堆
        priority_queue<int, vector<int>, greater<int>> q;
        // 需要使用砖块的 delta h 的和
        int sumH = 0;
        for (int i = 1; i < n; ++i) {
            int deltaH = heights[i] - heights[i - 1];
            if (deltaH > 0) {
                q.push(deltaH);
                // 如果优先队列已满，需要拿出一个其中的最小值，改为使用砖块
                if (q.size() > ladders) {
                    sumH += q.top();
                    q.pop();
                }
                if (sumH > bricks) {
                    return i - 1;
                }
            }
        }
        return n - 1;
    }
};

```

