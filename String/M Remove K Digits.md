## Remove K Digits

> Given a non-negative integer *num* represented as a string, remove *k* digits from the number so that the new number is the smallest possible.
>
> **Note:**
>
> - The length of *num* is less than 10002 and will be ≥ *k*.
> - The given *num* does not contain any leading zero.
>
> 
>
> **Example 1:**
>
> ```
> Input: num = "1432219", k = 3
> Output: "1219"
> Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.
> ```
>
> 
>
> **Example 2:**
>
> ```
> Input: num = "10200", k = 1
> Output: "200"
> Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.
> ```
>
> 
>
> **Example 3:**
>
> ```
> Input: num = "10", k = 2
> Output: "0"
> Explanation: Remove all the digits from the number and it is left with nothing which is 0.
> ```

## Solution  

* 从头开始遍历，找该数比前一个数小的地方，删除大的数
* 一开始找错了，一直找比前一个数大的，去掉大的

```java
class Solution {
    public String removeKdigits(String num, int k) {
        if(num.length()==k) return "0";
        while(num.length()>0&&k>0){
            int i=1;
            for(;i<num.length();i++){
                if(num.charAt(i)<num.charAt(i-1)) break;
            }
            if(i==1) num=num.substring(i);
            else if(i==num.length()) num=num.substring(0,i-1);
            else num=num.substring(0,i-1)+num.substring(i);
            while(num.length()>0&&num.charAt(0)=='0') num=num.substring(1);
            k--;
        }
        if(num.equals("")) return "0";
         return num;  
    }
}
```

## Solution 

* MinStack +  Greedy

```java
//time 11ms
class Solution {
    public String removeKdigits(String num, int k) {
        if(k>=num.length()||num.length()==0) return "0";
        Stack<Integer> s=new Stack<>();
        s.push(num.charAt(0)-'0');
        for(int i=1;i<num.length();i++){
            int now=num.charAt(i)-'0';
            while(!s.isEmpty()&&k>0&&now<s.peek()){
                s.pop();
                k--;
            }
            //important
            if(now!=0||!s.isEmpty()){
                s.push(now);
            }
        }
        while(k>0){
            s.pop();
            k--;
        }
        if(s.isEmpty()){
           return "0"; 
        }
        StringBuilder res=new StringBuilder();
        while(!s.isEmpty()){
            res.append(s.pop());
        }
        return res.reverse().toString();
    }
}
```

