## Reverse Bits

> Reverse bits of a given 32 bits unsigned integer.
>
>  
>
> **Example 1:**
>
> ```
> Input: 00000010100101000001111010011100
> Output: 00111001011110000010100101000000
> Explanation: The input binary string 00000010100101000001111010011100 represents the unsigned integer 43261596, so return 964176192 which its binary representation is 00111001011110000010100101000000.
> ```
>
> **Example 2:**
>
> ```
> Input: 11111111111111111111111111111101
> Output: 10111111111111111111111111111111
> Explanation: The input binary string 11111111111111111111111111111101 represents the unsigned integer 4294967293, so return 3221225471 which its binary representation is 10111111111111111111111111111111.
> ```
>
>  
>
> **Note:**
>
> - Note that in some languages such as Java, there is no unsigned integer type. In this case, both input and output will be given as signed integer type and should not affect your implementation, as the internal binary representation of the integer is the same whether it is signed or unsigned.
> - In Java, the compiler represents the signed integers using [2's complement notation](https://en.wikipedia.org/wiki/Two's_complement). Therefore, in **Example 2** above the input represents the signed integer `-3` and the output represents the signed integer `-1073741825`.
>
>  
>
> **Follow up**:
>
> If this function is called many times, how would you optimize it?\



## Solution one bit by one bit 

```java
public class Solution {
    // you need treat n as an unsigned value
    //in Java there is not a  unsigned value 2's complement 
    public int reverseBits(int n) {
        //int bit [] = new int[32];
        int ret = 0;
        for(int i = 0; i<32 ;i++){
            ret |=(((n&(1<<i))>>>i)<<(31-i));
        }
        return ret;
    }
}
```

## Solution  we divide and conqure

```c++
class Solution {
public:
    uint32_t reverseBits(uint32_t n) {
        //reverse the 16 bis
        n = (n >> 16) | (n << 16);
        //reverse the 8 bits  11111111 00000000 
        n = ((n & 0xff00ff00) >> 8) | ((n & 0x00ff00ff) << 8);
        //reverse the 4 bits 11110000 00001111
        n = ((n & 0xf0f0f0f0) >> 4) | ((n & 0x0f0f0f0f) << 4);
        //reverse the 2 bits 1100 0011
        n = ((n & 0xcccccccc) >> 2) | ((n & 0x33333333) << 2);
        //reverse the 1 bit 1010  0101
        n = ((n & 0xaaaaaaaa) >> 1) | ((n & 0x55555555) << 1);
        return n;
    }
};
```

