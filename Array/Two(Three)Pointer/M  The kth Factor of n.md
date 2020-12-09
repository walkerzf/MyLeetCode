## [ The kth Factor of n](https://leetcode-cn.com/problems/the-kth-factor-of-n/)

> Given two positive integers n and k.
>
> A factor of an integer n is defined as an integer i where n % i == 0.
>
> Consider a list of all factors of n sorted in ascending order, return the kth factor in this list or return -1 if n has less than k factors.
>
> ```
> Example 1:
> 
> Input: n = 12, k = 3
> Output: 3
> Explanation: Factors list is [1, 2, 3, 4, 6, 12], the 3rd factor is 3.
> Example 2:
> 
> Input: n = 7, k = 2
> Output: 7
> Explanation: Factors list is [1, 7], the 2nd factor is 7.
> Example 3:
> 
> Input: n = 4, k = 4
> Output: -1
> Explanation: Factors list is [1, 2, 4], there is only 3 factors. We should return -1.
> Example 4:
> 
> Input: n = 1, k = 1
> Output: 1
> Explanation: Factors list is [1], the 1st factor is 1.
> Example 5:
> 
> Input: n = 1000, k = 3
> Output: 4
> Explanation: Factors list is [1, 2, 4, 5, 8, 10, 20, 25, 40, 50, 100, 125, 200, 250, 500, 1000].
> 
> 
> Constraints:
> 
> 1 <= k <= n <= 100
> ```

## Solution Brute Force 

```java
class Solution {
    public int kthFactor(int n, int k) {
        int count = 0;
        for(int i =1 ; i<=n;i++){
            if(n%i==0) count++;
            if(count==k) return i;
        }   
        return -1;
    }
}
```

## Solution



Ref:https://leetcode-cn.com/problems/the-kth-factor-of-n/solution/cong-bao-li-sou-suo-dao-shi-jian-osqrtnkong-jian-o/

**sqrt(N) time  O(N) space** 

* store the  i and n/i
* but the square number need to notice the repeat ones
* but we need the sort quesion 

**sqrt(N) time  sqrt(N) space** 

* we can use the loop ``` ( i=1 ;i*i<=n；i++)```
* use sqrt(N) time ,in loop we check the k  and store the``` n/i```
* if(k>0) so we check the list that stores the ```n/i``` ,in reverse order



* **use O(1) space** 
* so we iteration two times 
* first time ``` 1->i*i<=n``` second time  ``` i *i>n - >1``` check the K 

```c++
class Solution {
public:
    int kthFactor(int n, int k) {

        int i;
        for(i = 1; i * i <= n; i ++)
            if(n % i == 0){
                k --;
                if(!k) return i;
            }

        i--; // 注意，此时 i * i > n，所以要 i --

        // 第二趟反向遍历，对 i 的初始值，还需要根据是否 i * i == n 做判断，避免重复
        for(i = (i * i == n ? i - 1 : i); i > 0; i --)
            if(n % i == 0){
                k --;
                if(!k) return n / i; // 看 n / i
            }

        return -1;
    }
};
```

