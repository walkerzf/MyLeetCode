## Hamming Distance

> The [Hamming distance](https://en.wikipedia.org/wiki/Hamming_distance) between two integers is the number of positions at which the corresponding bits are different.
>
> Given two integers `x` and `y`, calculate the Hamming distance.
>
> **Note:**
> 0 ≤ `x`, `y` < 231.
>
> **Example:**
>
> ```
> Input: x = 1, y = 4
> 
> Output: 2
> 
> Explanation:
> 1   (0 0 0 1)
> 4   (0 1 0 0)
>        ↑   ↑
> 
> The above arrows point to positions where the corresponding bits are different.
> ```

## Solution  bit shift

```java
//time 0ms 
class Solution {
    public int hammingDistance(int x, int y) {
        int count = 0;
        int res = 0;
        for(int i = 1 ; count<=31;i =(i<<1)){
            res +=(((x&i)^(y&i))>>count);
            count++;
        }
        return res;
    }
}
```

## Solution Bit count Function In Java

```java
public class Solution {
    public int hammingDistance(int x, int y) {
        return Integer.bitCount(x ^ y);
    }
}
```

## Solution

``` n&n-1``` the usual way to remove the lowest bit zero , used in  some question ,to remove the  1 bit 

```c++
class Solution {
public:
    int hammingDistance(int x, int y) {
        int dist = 0, n = x ^ y;
        while (n) {
            ++dist;
            n &= n - 1;
        }
        return dist;
    }
};

```

