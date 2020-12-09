## Power of Two

> Given an integer, write a function to determine if it is a power of two.
>
> **Example 1:**
>
> ```
> Input: 1
> Output: true 
> Explanation: 20 = 1
> ```
>
> **Example 2:**
>
> ```
> Input: 16
> Output: true
> Explanation: 24 = 16
> ```
>
> **Example 3:**
>
> ```
> Input: 218
> Output: false
> ```

## Solution Binary Search

```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        int l = 0 ;
        int r = n /2 ;
        while( l < r){
            int mid = l + ( r - l ) /2 ;
            if((1 <<mid) == n ){
                return true ;
            }else if((1<<mid) > n){
                r = mid -1;
            }else{
                 l = mid+1 ;
            }
        }
        return (1<<l) ==n;
    }
}
```

## Solution

* power of Two means only one bit in the number is ```1```

Ref:https://leetcode.com/explore/challenge/card/june-leetcoding-challenge/540/week-2-june-8th-june-14th/3354/discuss/63966/4-different-ways-to-solve-Iterative-Recursive-Bit-operation-Math

```c++
class Solution {
public:
    bool isPowerOfTwo(int n) {
        if(n<=0) return false;
        return !(n&(n-1));
    }
};
```

