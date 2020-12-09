## 题干

> You are given two sorted arrays, A and B, where A has a large enough buffer at the end to hold B. Write a method to merge B into A in sorted order.
>
> Initially the number of elements in A and B are m and n respectively.
>
> Example:
>
> Input:
> A = [1,2,3,0,0,0], m = 3
> B = [2,5,6],       n = 3
>
> Output: [1,2,2,3,5,6]

## Solution Two pointer

```java
//time：0ms 38MB
class Solution {
    public void merge(int[] A, int m, int[] B, int n) {
        while(m>0&&n>0){
        	//比较较大的数字插在A的尾部，当一个数组遍历完之后出循环
            if(A[m-1]>B[n-1]){
                A[m+n-1]=A[m-1];
                m--;
            }
            else {
                A[m+n-1]=B[n-1];
                n--; 
            }
        }
        //如果是B数组没有遍历完，则需要继续插入，A数组没有遍历完的话，保持原样，
        while(n>0){
            A[n-1]=B[n-1];
            n--;
        }
    }
}
```

* 双指针还有另外一种做法，创建一个m+n的数组，从头到尾遍历AB数组
* 或者是直接将B数组复制到A中，然后```sort```