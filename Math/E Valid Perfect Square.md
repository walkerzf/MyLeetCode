##  Valid Perfect Square

> Given a positive integer *num*, write a function which returns True if *num* is a perfect square else False.
>
> **Note:** **Do not** use any built-in library function such as `sqrt`.
>
> **Example 1:**
>
> ```
> Input: 16
> Output: true
> ```
>
> **Example 2:**
>
> ```
> Input: 14
> Output: false
> ```

## Solution  Binary Search

```java
class Solution {
    public boolean isPerfectSquare(int num) {
        if(num==0||num==1) return true;
        int l=1;
        int r=num;
        while(l<=r){
            int mid=(r-l)/2+l;
            if((long)mid*mid==num) return true;
            else if((long)mid*mid<num) l=mid+1;
            else r=mid-1;
        }
        return false;
    }
}
```

## Solution Math

* 所有的n^2都是由等差数列得来的

```java
public boolean isPerfectSquare(int num) {
     int i = 1;
     while (num > 0) {
         num -= i;
         i += 2;
     }
     return num == 0;
 }
```

## Solution Newton

```java
public boolean isPerfectSquare(int num) {
        long x = num;
        while (x * x > num) {
            x = (x + num / x) >> 1;
        }
        return x * x == num;
    }
```

