## [16. 3Sum Closest](https://leetcode-cn.com/problems/3sum-closest/)

> Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.
>
> Example:
>
> Given array nums = [-1, 2, 1, -4], and target = 1.
>
> The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

## Solution 三指针

* 固定一个指针，动其他两个指针
* 注意对几次去重
  * 选```nums[i]```的重复的情况
  * ```l  r```中出现连续重复的数字的情况

```java
//time 4ms 38.8MB
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int closest=Integer.MAX_VALUE;
        int result=0;
        for(int i=0;i<nums.length-2;i++){
            if(i==0||(i>0&&nums[i]!=nums[i-1])){
                int l=i+1;
                int r=nums.length-1;
                int res=target-nums[i];
                while(l<r){
                    int sum=nums[l]+nums[r];
                    if(sum==res) return target;
                    else if(sum<res){
                        if(res-sum<closest){
                            closest=res-sum;
                            result=sum+nums[i];
                        }
                        while(l<r&&nums[l]==nums[l+1]) l++;
                        l++;
                    }else{
                        if(sum>res){
                            if(sum-res<closest){
                                closest=sum-res;
                                result=sum+nums[i];
                            }
                        }
                        while(l<r&&nums[r]==nums[r-1]) r--;
                        r--;
                    }
                }

            }
        }
        return result;
    }
}
```

```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int closest=Integer.MAX_VALUE;
        int result=0;
        for(int i=0;i<nums.length-2;i++){
            if(i==0||(i>0&&nums[i]!=nums[i-1])){
                int l=i+1;
                int r=nums.length-1;
                int res=target-nums[i];
                while(l<r){
                    int sum=nums[l]+nums[r];
                    if(sum==res) return target;
                    else if(sum<res){
                        while(l<r&&nums[l]==nums[l+1])l++;
                        l++;
                    }else{
                        while(l<r&&nums[r]==nums[r-1]) r--;
                        r--;
                    }
                    if(Math.abs(sum-res)<closest){
                        closest=Math.abs(sum-res);
                        result=sum+nums[i];
                    }
                }
            }
        }
        return result;
    }
}
```

