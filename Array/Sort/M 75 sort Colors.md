## Sort Colors

> Given an array with *n* objects colored red, white or blue, sort them **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** so that objects of the same color are adjacent, with the colors in the order red, white and blue.
>
> Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.
>
> **Note:** You are not suppose to use the library's sort function for this problem.
>
> **Example:**
>
> ```
> Input: 	[2,0,2,1,1,0]
> Output: [0,0,1,1,2,2]
> ```
>
> **Follow up:**
>
> - A rather straight forward solution is a two-pass algorithm using counting sort.
>   First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
> - Could you come up with a one-pass algorithm using only constant space?



* **The point is in-place**

## Solution Count sort

The intuition is Count sort

```java
//time 0ms 
class Solution {
    public void sortColors(int[] nums) {
        int count [] =new int [3];
        for(int i = 0 ;i< nums.length ;i++){
            count[nums[i]]++;
        }
        int index =0;
        for(int i=0;i<count[0];i++){
            nums[index++]=0;
        }
        for(int i=0;i<count[1];i++){
            nums[index++]=1;
        }
        for(int i=0;i<count[2];i++){
            nums[index++]=2;
        }
    }
}

```

## Solution Three Partition

Ref: https://en.wikipedia.org/wiki/Dutch_national_flag_problem \

> This problem can also be viewed in terms of rearranging elements of an [array](https://en.wikipedia.org/wiki/Array_data_structure). Suppose each of the possible elements could be classified into exactly one of three categories (bottom, middle, and top). For example, if all the elements are in 0 ... 1, the bottom could be defined as elements in 0 ... 0.1 (not including 0.1), the middle as 0.1 ... 0.3 (not including 0.3) and the top as 0.3 and greater. (The choice of these values illustrates that the categories need not be equal ranges). The problem is then to produce an array such that all "bottom" elements come before (have an index less than the index of) all "middle" elements, which come before all "top" elements.
>
> One [algorithm](https://en.wikipedia.org/wiki/Algorithm) is to have the top group grow down from the top of the array, the bottom group grow up from the bottom, and keep the middle group just above the bottom. The algorithm indexes three locations, the bottom of the top group, the top of the bottom group, and the top of the middle group. Elements that are yet to be sorted fall between the middle and the top group.[[4\]](https://en.wikipedia.org/wiki/Dutch_national_flag_problem#cite_note-NIST-4) At each step, examine the element just above the middle. If it belongs to the top group, swap it with the element just below the top. If it belongs in the bottom, swap it with the element just above the bottom. If it is in the middle, leave it. Update the appropriate index. Complexity is Θ(n) moves and examinations.[[1\]](https://en.wikipedia.org/wiki/Dutch_national_flag_problem#cite_note-monash-1)

* **Pseudocode**: 
  * Entries from 0 up to (but not including) ```i```are values less than *mid*,
  * entries from *i* up to (but not including) *j* are values equal to *mid*,
  * entries from *j* up to (but not including) *k* are values not yet sorted, and
  * entries from *k* to the end of the array are values greater than *mid*. 

```python
rocedure three-way-partition(A : array of values, mid : value):
    i ← 0
    j ← 0
    k ← size of A

    while j < k:
        if A[j] < mid:
            swap A[i] and A[j]
            i ← i + 1
            j ← j + 1
        else if A[j] > mid:
            k ← k - 1
            swap A[j] and A[k]
        else:
            j ← j + 1
```



```java
//time  0ms
class Solution {
    public void sortColors(int[] nums) {
        int left = 0 ;
        int right = nums.length ;
        int index =0;
        int tmp =0;
        while(index <right){
            if(nums[index] < 1){
                //swap
                tmp =nums[left];
                nums[left++] =nums[index];
                nums[index++] =tmp;
            }else if(nums[index] > 1){
                //we need to -1
                right --;
                //swap ,we do not increase index
                //because the nums[index] may be 0 or 2
                tmp =nums[right];
                nums[right] =nums[index];
                nums[index] =tmp;
            }// we meet 1 ,we do not change anything
            else index ++;
        }
    }
}
```

* Similar Question ： https://en.wikipedia.org/wiki/American_flag_sort
* The American flag sort is similar bucket question , it is very useful sort large objects such as strings, for **which comparison is not a unit-time operation** 