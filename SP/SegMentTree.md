## Segment Tree

```c++
void build(int s, int t, int p) {
  // 对 [s,t] 区间建立线段树,当前根的编号为 p
  if (s == t) {
    d[p] = a[s];
    return;
  }
  int m = (s + t) / 2;
  build(s, m, p * 2), build(m + 1, t, p * 2 + 1);
  // 递归对左右区间建树
  d[p] = d[p * 2] + d[(p * 2) + 1];
}
```

```c++
int getsum(int l, int r, int s, int t, int p) {
  // [l,r] 为查询区间,[s,t] 为当前节点包含的区间,p 为当前节点的编号
  if (l <= s && t <= r)
    return d[p];  // 当前区间为询问区间的子集时直接返回当前区间的和
  int m = (s + t) / 2, sum = 0;
  if (l <= m) sum += getsum(l, r, s, m, p * 2);
  // 如果左儿子代表的区间 [l,m] 与询问区间有交集,则递归查询左儿子
  if (r > m) sum += getsum(l, r, m + 1, t, p * 2 + 1);
  // 如果右儿子代表的区间 [m+1,r] 与询问区间有交集,则递归查询右儿子
  return sum;
}
```

## [1649. Create Sorted Array through Instructions](https://leetcode-cn.com/problems/create-sorted-array-through-instructions/)

Given an integer array `instructions`, you are asked to create a sorted array from the elements in `instructions`. You start with an empty container `nums`. For each element from **left to right** in `instructions`, insert it into `nums`. The **cost** of each insertion is the **minimum** of the following:

- The number of elements currently in `nums` that are **strictly less than** `instructions[i]`.
- The number of elements currently in `nums` that are **strictly greater than** `instructions[i]`.

For example, if inserting element `3` into `nums = [1,2,3,5]`, the **cost** of insertion is `min(2, 1)` (elements `1` and `2` are less than `3`, element `5` is greater than `3`) and `nums` will become `[1,2,3,3,5]`.

Return *the **total cost** to insert all elements from* `instructions` *into* `nums`. Since the answer may be large, return it **modulo** `109 + 7`

 

**Example 1:**

```
Input: instructions = [1,5,6,2]
Output: 1
Explanation: Begin with nums = [].
Insert 1 with cost min(0, 0) = 0, now nums = [1].
Insert 5 with cost min(1, 0) = 0, now nums = [1,5].
Insert 6 with cost min(2, 0) = 0, now nums = [1,5,6].
Insert 2 with cost min(1, 2) = 1, now nums = [1,2,5,6].
The total cost is 0 + 0 + 0 + 1 = 1.
```

**Example 2:**

```
Input: instructions = [1,2,3,6,5,4]
Output: 3
Explanation: Begin with nums = [].
Insert 1 with cost min(0, 0) = 0, now nums = [1].
Insert 2 with cost min(1, 0) = 0, now nums = [1,2].
Insert 3 with cost min(2, 0) = 0, now nums = [1,2,3].
Insert 6 with cost min(3, 0) = 0, now nums = [1,2,3,6].
Insert 5 with cost min(3, 1) = 1, now nums = [1,2,3,5,6].
Insert 4 with cost min(3, 2) = 2, now nums = [1,2,3,4,5,6].
The total cost is 0 + 0 + 0 + 0 + 1 + 2 = 3.
```

**Example 3:**

```
Input: instructions = [1,3,3,3,2,4,2,1,2]
Output: 4
Explanation: Begin with nums = [].
Insert 1 with cost min(0, 0) = 0, now nums = [1].
Insert 3 with cost min(1, 0) = 0, now nums = [1,3].
Insert 3 with cost min(1, 0) = 0, now nums = [1,3,3].
Insert 3 with cost min(1, 0) = 0, now nums = [1,3,3,3].
Insert 2 with cost min(1, 3) = 1, now nums = [1,2,3,3,3].
Insert 4 with cost min(5, 0) = 0, now nums = [1,2,3,3,3,4].
Insert 2 with cost min(1, 4) = 1, now nums = [1,2,2,3,3,3,4].
Insert 1 with cost min(0, 6) = 0, now nums = [1,1,2,2,3,3,3,4].
Insert 2 with cost min(2, 4) = 2, now nums = [1,1,2,2,2,3,3,3,4].
The total cost is 0 + 0 + 0 + 0 + 1 + 0 + 1 + 0 + 2 = 4.
```

 

**Constraints:**

- `1 <= instructions.length <= 105`
- `1 <= instructions[i] <= 105`

* Solution
* Ref : [Zerotac](https://leetcode-cn.com/problems/create-sorted-array-through-instructions/solution/tong-guo-zhi-ling-chuang-jian-you-xu-shu-zu-by-zer/)
* [OI WIKI](https://oi-wiki.org/ds/seg/)

## SegMent Tree

```c++
class SegTree{
  public:
    int n;
    vector<int> segnode;
    
    SegTree(int num){
        //the maxvalue 
        n = num;
        // the number of segnode 
        segnode.resize(n*4);
    }
    void update(int index , int l, int r ,int x){
        if(r<x||l>x) return;
        segnode[index]++;
        if(l==r) return ;
        int mid = (r-l)/2+l;
        update(index*2,l,mid,x);
        update(index*2+1,mid+1,r,x);
    }
    int query(int index , int l, int r, int ql,int qr){
        if(ql>r||qr<l) return 0;
        if(ql<=l&&qr>=r) return segnode[index];
        int mid = (r-l)/2+l;
        int left=0,right=0;
        //important weisha xiecheng l  ql qr 
        if(mid>=l)  left = query(index*2,l,mid,ql,qr);
        if(mid<r) right = query(index*2+1,mid+1,r,ql,qr); 
        return left+right;
    }
    void uupdate(int x){
        update(1,1,n,x);
    }
    int uquery(int left,int right){
        return query(1,1,n,left,right);
    }
    
};

class Solution {
public:
    const int mod = 1e9+7;
    int createSortedArray(vector<int>& instructions) {
        long long ret = 0;
        int maxvalue = *max_element(instructions.begin(),instructions.end());
        SegTree seg(maxvalue);
        int len = instructions.size();
        
        for(int i = 0;i<instructions.size();i++){
            int s = seg.uquery(1,instructions[i]-1);
            int b = seg.uquery(instructions[i]+1,maxvalue);
            ret = (ret+(long long)(min(s,b))) %mod;
            seg.uupdate(instructions[i]);
        }
        
        return ret;
    }
};
```

## FenWick Tree

```c++
class FeWick{
  public:
    int n;
    vector<int> dp;
    FeWick(int num){
        n = num;
        dp.resize(n+1,0);
    }
    int lowbit(int index){
        return index&(-index);
    }
    void update(int index,int inc){
        while(index<=n){
            dp[index]+=inc;
            index+=lowbit(index);
        }
    }
    int query(int index){
        int ret = 0;
        while(index>0){
            ret +=dp[index];
            index -=lowbit(index);
        }
        return ret;
    }
};

class Solution {
public:
    const int mod = 1e9+7;
    int createSortedArray(vector<int>& instructions) {
        long long ret = 0;
        int maxvalue = *max_element(instructions.begin(),instructions.end());
        FeWick fe(maxvalue);
        int len = instructions.size();
        for(int i = 0;i<instructions.size();i++){
            int s = fe.query(instructions[i]-1);
            int b = (i) - fe.query(instructions[i]);
            ret = (ret+(long long)(min(s,b))) % mod;
            fe.update(instructions[i],1);
        }
        
        return ret;
    }
};
```

