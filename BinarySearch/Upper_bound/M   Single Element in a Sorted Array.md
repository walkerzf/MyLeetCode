##  Single Element in a Sorted Array

> You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once. Find this single element that appears only once.
>
>  
>
> **Example 1:**
>
> ```
> Input: [1,1,2,3,3,4,4,8,8]
> Output: 2
> ```
>
> **Example 2:**
>
> ```
> Input: [3,3,7,7,10,11,11]
> Output: 10
> ```
>
>  
>
> **Note:** Your solution should run in O(log n) time and O(1) space.

* 要利用好其他元素都是出现两次，```unique``` element元素只出现一次

## Solution

```java
//time 0ms
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int l=0;
        int r=nums.length-1;
        while(l<r){
            if((r-l)==1) return nums[l];
            int mid=(r-l)/2+l;
            if(nums[mid]!=nums[mid-1]&&nums[mid]!=nums[mid+1]) return nums[mid];
            if(nums[mid]==nums[mid-1]&&(mid+1)%2==0){
                l=mid+1;
            }else if(nums[mid]==nums[mid+1]&&(mid+2)%2==0){
                l=mid+2;
            }
            else{
                r=mid-1;
            }
        }
        return nums[l];
    }
}
```

* 比较蛋疼的做法，应该先处理```mid```这个值，看它属于哪一个```pair```

```java
   public static int singleNonDuplicate(int[] nums) {
        int start = 0, end = nums.length - 1;

        while (start < end) {
            // We want the first element of the middle pair,
            // which should be at an even index if the left part is sorted.
            // Example:
            // Index: 0 1 2 3 4 5 6
            // Array: 1 1 3 3 4 8 8
            //            ^
            int mid = (end - start) / 2 + start;
            if (mid % 2 == 1) mid--;

            // We didn't find a pair. The single element must be on the left.
            // (pipes mean start & end)
            // Example: |0 1 1 3 3 6 6|
            //               ^ ^
            // Next:    |0 1 1|3 3 6 6
            if (nums[mid] != nums[mid + 1]) end = mid;

            // We found a pair. The single element must be on the right.
            // Example: |1 1 3 3 5 6 6|
            //               ^ ^
            // Next:     1 1 3 3|5 6 6|
            else start = mid + 2;
        }

        // 'start' should always be at the beginning of a pair.
        // When 'start > end', start must be the single element.
        return nums[start];
    }
```



* 或者进行二分的数据就是```pair```的index

```java
class Solution {
    public int singleNonDuplicate(int[] nums) {
        int l=0;
        int n=nums.length;
        int r=n/2;
        while(l<r){
            int mid=(r-l)/2+l;
            if(nums[2*mid]!=nums[2*mid+1]) r=mid;
            else l=mid+1;
        }
        return nums[2*l];       
    }
}
```

* 利用与```0x01``` 异或，表示当为偶数时+1 ，为奇数时-1

```java
public int singleNonDuplicate(int[] nums) {
    int lo = 0, hi = nums.length - 1;
    while (lo < hi) {
        int mid = (lo + hi) >>> 1;
        if (nums[mid] == nums[mid ^ 1])
            lo = mid + 1;
        else
            hi = mid;
    }
    return nums[lo];
}
```

