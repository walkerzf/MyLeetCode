## [LCP 12. 小张刷题计划](https://leetcode-cn.com/problems/xiao-zhang-shua-ti-ji-hua/)

> 为了提高自己的代码能力，小张制定了 LeetCode 刷题计划，他选中了 LeetCode 题库中的 n 道题，编号从 0 到 n-1，并计划在 m 天内按照题目编号顺序刷完所有的题目（注意，小张不能用多天完成同一题）。
>
> 在小张刷题计划中，小张需要用 time[i] 的时间完成编号 i 的题目。此外，小张还可以使用场外求助功能，通过询问他的好朋友小杨题目的解法，可以省去该题的做题时间。为了防止“小张刷题计划”变成“小杨刷题计划”，小张每天最多使用一次求助。
>
> 我们定义 m 天中做题时间最多的一天耗时为 T（小杨完成的题目不计入做题总时间）。请你帮小张求出最小的 T是多少。
>
> 示例 1：
>
> 输入：time = [1,2,3,3], m = 2
>
> 输出：3
>
> 解释：第一天小张完成前三题，其中第三题找小杨帮忙；第二天完成第四题，并且找小杨帮忙。这样做题时间最多的一天花费了 3 的时间，并且这个值是最小的。
>
> 示例 2：
>
> 输入：time = [999,999,999], m = 4
>
> 输出：0
>
> 解释：在前三天中，小张每天求助小杨一次，这样他可以在三天内完成所有的题目并不花任何时间。
>
>  
>
> 限制：
>
> 1 <= time.length <= 10^5
> 1 <= time[i] <= 10000
> 1 <= m <= 1000

## Solution  二分

Ref:https://leetcode-cn.com/problems/split-array-largest-sum/solution/er-fen-cha-zhao-by-liweiwei1419-4/

我在网上查了一下资料，这道题属于「最大化最小值」，这一类问题通常是竞赛类的问题。事实上在今年（2020 年）的「力扣」春季团队赛中就出现过这样的问题，这道题是 LCP 12. 小张刷题计划，这道题与「力扣」第 410 题的区别就只在于这个问题的场景，要求我们用贪心算法，**贪心的点是「每一天把耗时最多的问题交给小杨去做」。**

* 当我们在计算天数的时候，运用了贪心算法，很关键

```java
//time  11ms

class Solution {
    public int minTime(int[] time, int m) {
        int l=0;
        int r=0;
        for(int i=0;i<time.length;i++){
            r+=time[i];
        }
        while(l<r){
            int mid=(r-l)/2+l;
            int cnt=calDay(time,mid);
            if(cnt>m){
                l=mid+1;
            }else{
                r=mid;
            }
        }
        return l;
    }
    private int calDay(int []time,int maxCost){
        int cnt=1;
        int curSum=time[0];
        int curMax=time[0];
        for(int i=1;i<time.length;i++){
            curMax=Math.max(curMax,time[i]);
            if(curSum+time[i]-curMax>maxCost){
                curSum=time[i];
                cnt++;
                curMax=time[i];
            }else{
                curSum+=time[i];
            }
        }
        return cnt;
    }
}
```

