## Add Digits

> Given a non-negative integer `num`, repeatedly add all its digits until the result has only one digit.
>
> **Example:**
>
> ```
> Input: 38
> Output: 2 
> Explanation: The process is like: 3 + 8 = 11, 1 + 1 = 2. 
>              Since 2 has only one digit, return it.
> ```
>
> **Follow up:**
> Could you do it without any loop/recursion in O(1) runtime?

## Solution Loop

```
class Solution {
    public int addDigits(int num) {
        int temp = num;
        while(temp>=10){
            temp = 0;
            while(num>0){
                temp+=num%10;
                num/=10;
            }
            num = temp;
        }
        return temp;
    }
}
```

## Solution Digit Sum

Ref： WiKi https://en.wikipedia.org/wiki/Digital_root

```java
class Solution {
    public int addDigits(int num) {
        if(num==0) return 0;
        if(num%9==0) return 9;
        else return num%9;
    }
}
```

