## Single Number II

> Given a **non-empty** array of integers, every element appears *three* times except for one, which appears exactly once. Find that single one.
>
> **Note:**
>
> Your algorithm should have a linear runtime complexity. **Could you implement it without using extra memory?**
>
> **Example 1:**
>
> ```
> Input: [2,2,3,2]
> Output: 3
> ```
>
> **Example 2:**
>
> ```
> Input: [0,1,0,1,0,1,99]
> Output: 99
> ```

## Solution Hash Set

* record all the data and sum
* 3 times data -sum /2



## Solution Hash Map

get the key ‘s’ value equals one

## Solution Bitwise

O(N) this bit operation is too diffculty ,and use the 数电的知识

```java
class Solution {
  public int singleNumber(int[] nums) {
    int seenOnce = 0, seenTwice = 0;

    for (int num : nums) {
      // first appearence: 
      // add num to seen_once 
      // don't add to seen_twice because of presence in seen_once

      // second appearance: 
      // remove num from seen_once 
      // add num to seen_twice

      // third appearance: 
      // don't add to seen_once because of presence in seen_twice
      // remove num from seen_twice
      seenOnce = ~seenTwice & (seenOnce ^ num);
      seenTwice = ~seenOnce & (seenTwice ^ num);
    }

    return seenOnce;
  }
}

```

## Solution record the bit 

```java
class Solution {
    public int singleNumber(int[] nums) {
        //O（32） -> O（1）
        int[] counts = new int[32];
        for(int num : nums) {
            for(int j = 0; j < 32; j++) {
                counts[j] += num & 1;
                num >>>= 1;
            }
        }
        int res = 0, m = 3;
        for(int i = 0; i < 32; i++) {
            res <<= 1;
            res |= counts[31 - i] % m;
        }
        return res;
    }
}

```

