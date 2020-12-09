##  Bitwise AND of Numbers Range

> Given a range [m, n] where 0 <= m <= n <= 2147483647, return the bitwise AND of all numbers in this range, inclusive.
>
> **Example 1:**
>
> ```
> Input: [5,7]
> Output: 4
> ```
>
> **Example 2:**
>
> ```
> Input: [0,1]
> Output: 0
> ```

* 这题主要是找到两个的重复的部分

## Solution

```java
class Solution {
    public int rangeBitwiseAnd(int m, int n) {
        int bitnum1=calBit(m);
        int bitnum2=calBit(n);
        if(bitnum1*bitnum2==0) return 0;
        if(bitnum2>bitnum1) return 0;
        else{
            int res=1<<(bitnum2-1);
            bitnum1--;
            while(bitnum1>0){                
                bitnum1--;
                int num1=m&(1<<bitnum1);
                int num2=n&(1<<bitnum1);
                if((num1^num2)==0) res=res+(m&(1<<bitnum1));
                else break;
            }
            return res;
        }
        
    }
    private int calBit(int x){
        int count=0;
        while(x>0){
            x>>=1;
            count++;
        }
        return count;
    }
}
```

## Solution

* 如果两个bit位数不同，那么m为0，结果并为0
* 如果两个位数相同，且m==n ，返回m
* 如果m！=n，那么必定有相同的部分，m剩下的是相同的部分，然后再左移曾经右移的长度

```java
class Solution {
    public int rangeBitwiseAnd(int m, int n) {
        int shift = 0;
        while(m < n){ // when equals 0, stop
            m >>= 1;
            n >>= 1;
            shift++;
        }
        return m << shift;
    }
}
```

