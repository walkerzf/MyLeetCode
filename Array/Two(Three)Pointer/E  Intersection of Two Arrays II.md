## [ Intersection of Two Arrays II](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/)

> ```
> Given two arrays, write a function to compute their intersection.
> 
> Example 1:
> 
> Input: nums1 = [1,2,2,1], nums2 = [2,2]
> Output: [2,2]
> Example 2:
> 
> Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
> Output: [4,9]
> Note:
> 
> Each element in the result should appear as many times as it shows in both arrays.
> The result can be in any order.
> Follow up:
> 
> What if the given array is already sorted? How would you optimize your algorithm?
> What if nums1's size is small compared to nums2's size? Which algorithm is better?
> What if elements of nums2 are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?
> ```



## Solution Sort TwoPointer

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        List<Integer> res = new ArrayList<>();
        int p1 = 0 ;
        int p2 =  0;
        while(p1<nums1.length&&p2<nums2.length){
            if(nums1[p1]==nums2[p2]){
                res.add(nums1[p1]);
                p1++;p2++;
            }else if(nums1[p1]>nums2[p2]){
                p2++;
            }else{
                p1++;
            }
        }
        int ret []= new int[res.size()];
        int index = 0;
        for(int n:res){
            ret[index++] =n;
        }
        return ret;
    }
}
```



## Solution One Array to Map And Iterative another

```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        Map<Integer,Integer> m =new HashMap<>();
        for(int i =0 ;i<nums1.length;i++){
            m.put(nums1[i],m.getOrDefault(nums1[i],0)+1);
        }
        int ret [] = new int[nums1.length];
        int index = 0;
        for(int i = 0;i<nums2.length;i++){
            if(m.containsKey(nums2[i])&&m.get(nums2[i])>0){
                m.put(nums2[i],m.get(nums2[i])-1);
                ret[index++] = nums2[i];
            }
        }
        return Arrays.copyOfRange(ret,0,index);
    }
}
```

