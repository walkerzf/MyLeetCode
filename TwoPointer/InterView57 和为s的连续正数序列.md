## 题干

> 输入一个正整数 target ，输出所有和为 target 的连续正整数序列（至少含有两个数）。
>
> 序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。
>
>  
>
> 示例 1：
>
> 输入：target = 9
> 输出：[[2,3,4],[4,5]]
> 示例 2：
>
> 输入：target = 15
> 输出：[[1,2,3,4,5],[4,5,6],[7,8]]
>
>
> 限制：
>
> 1 <= target <= 10^5
>



## Solution Two Pointer

* 这题要注意的返回的是二位数组，这个二维数组又跟以前的不一样，利用```List```的```Method toArray``` ，得到结果！！

```java
//time 4ms 37.4MB
class Solution {
    public int[][] findContinuousSequence(int target) {
            List<int []> answer=new LinkedList<>();
            if(target<=2) return null;
            int min=1;
            int max=2;
            int sum=min+max;
            while(min<target/2+1){
                if(sum<target){
                    max++;
                    sum+=max;
                }
                else if(sum>target){
                    sum-=min;
                    min++;
                }
                else {
                    int start=0;
                    int tmp[]=new int[max-min+1];
                    for(int i=min;i<=max;i++){
                        tmp[start++]=i;
                    }
                    answer.add(tmp);
                    sum-=min;
                    min++;
                    
                }
            }
            return answer.toArray(new int[][]{});
    }
}
```

## Solution Math

* 该题可以利用等差数列的求和公式，求出```x```和````y````的关系，然后根据在取值范围内的``y``求出合理的```x```。