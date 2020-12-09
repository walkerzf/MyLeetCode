## [202. Happy Number](https://leetcode-cn.com/problems/happy-number/)

> Write an algorithm to determine if a number n is "happy".
>
> A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.
>
> Return True if n is a happy number, and False if not.
>
> Example: 
>
> Input: 19
> Output: true
> Explanation: 
> 12 + 92 = 82
> 82 + 22 = 68
> 62 + 82 = 100
> 12 + 02 + 02 = 1
>



## Solution

```java
// class Solution {
//     public boolean isHappy(int n) {
//         int sum=n;
//         int count=0;
//         while(sum!=1&&count<20){
//             int tmp=0;
//             while(sum>0){
//                 tmp+=(sum%10)*(sum%10);
//                 sum/=10;
//             }
//             sum=tmp;
//             count++;
//         }
//         if(sum==1) return true;
//         else return false;
//     }
// }
class Solution {
    public boolean isHappy(int n) {
        int fast=n;
        int slow=n;
        do{
            slow=squareSum(slow);
            fast=squareSum(fast);
            fast=squareSum(fast);
        }while(slow!=fast);
        if(fast==1)
            return true;
        else return false;
    }
    
    private int squareSum(int m){
        int squaresum=0;
        while(m!=0){
           squaresum+=(m%10)*(m%10);
            m/=10;
        }
        return squaresum;
    }
}
```

## Solution 用HashSet

```java
class Solution {
    public boolean isHappy(int n) {
        HashSet<Integer> set = new HashSet<Integer>();
        while(n!=1&&!set.contains(n)){
            set.add(n);
            n=getNext(n);
        }
        return n==1;
    }

    public int getNext(int n){
        int total = 0;
        while(n>0){
            int temp = n%10;
            n/=10;
            total+=temp*temp;
        }
        return total;
    }
}
```

