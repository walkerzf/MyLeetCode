## [面试题62. 圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

> 0,1,,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字。求出这个圆圈里剩下的最后一个数字。
>
> 例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。
>
>  
>
> 示例 1：
>
> 输入: n = 5, m = 3
> 输出: 3
> 示例 2：
>
> 输入: n = 10, m = 17
> 输出: 2
>
>
> 限制：
>
> 1 <= n <= 10^5
> 1 <= m <= 10^6
>

## Solution 暴力模拟法

* 这里使用```ArrayList``` 的性能要比```LinkedList``` 好，不会超时

```java
//time 1084ms 41.5MB
class Solution {
    public int lastRemaining(int n, int m) {
        List<Integer> l=new ArrayList<>();
        for(int i=0;i<n;i++){
            l.add(i);
        }
        int p=0;
        while(l.size()!=1){
            p+=m-1;
            int index=p%l.size();
            l.remove(index);
            p=index;
        }
        return l.get(0);
    }
}
```

## Solution 数学解法

```java
// 8ms 36.2MB
class Solution {
    public int lastRemaining(int n, int m) {
        int ans=0;
        for(int i=2;i<n+1;i++){
            ans=(ans+m)%i;
        }
        return ans;
    }
}
```

