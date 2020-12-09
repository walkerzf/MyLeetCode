##  面试题56 - I. 数组中数字出现的次数 Single Number Ⅲ



> 一个整型数组 nums 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是O(n)，空间复杂度是O(1)。
>
>  
>
> 示例 1：
>
> 输入：nums = [4,1,4,6]
> 输出：[1,6] 或 [6,1]
> 示例 2：
>
> 输入：nums = [1,2,10,4,1,4,3,3]
> 输出：[2,10] 或 [10,2]
>
>
> 限制：
>
> 2 <= nums <= 10000
>



## Solution 暴力

```java
//time  14ms 
class Solution {
    public int[] singleNumbers(int[] nums) {
        int res[]=new int[2];
        Set<Integer> s=new HashSet<>();
        for(int i=0;i<nums.length;i++){
            if(!s.contains(nums[i])){
                s.add(nums[i]);
            }else{
                s.remove(nums[i]);
            }
        }
        int index=0;
        for(int n:s){
            res[index++]=n;
        }
        return res;
    }
}
```

## Solution BitLevel

* 获得所有数的异或值，即两个出现一次的异或值
* 取他bit上为1 的一个数，跟其他所有的数，与
  * 重复的数，肯定会出现一边
  * 出现一次的数肯定会出现在两边

```java
//time 2ms
class Solution {
    public int[] singleNumbers(int[] nums) {
        int res[]=new int[2];
        int sum=0;
        for(int i=0;i<nums.length;i++){
            sum^=nums[i];
        }
        //获取某个1
        sum=(-sum)&sum;
        for(int i=0;i<nums.length;i++){
            if((sum&nums[i])==0){
                res[0]^=nums[i];
            }else{
                res[1]^=nums[i];
            }
        }
        return res;
    }
}
```

