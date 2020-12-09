#### [把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

> 给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。
>
>  
>
> ```
> 示例 1:
> 
> 输入: 12258
> 输出: 5
> 解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"
> 
> 
> 提示：
> 
> 0 <= num < ```2^31```
> ```
>
> 
>

## Solution 

* the point is the leading zero is invalid
* we can deal with it as a DP problem like 打家劫社
* we can deal with it as a DFS problem 

```java
//time 11ms 
class Solution {
    int count =0;
    Map<Character ,Character> dic ;
    public int translateNum(int num) {
        String n =String.valueOf(num);
        dfs(n ,0 , "");
        return count;
    }
    private void dfs(String s, int index,String tmp){
        if(index==s.length()){
            count++;
            return;
        }
        String a ="";
        if(s.charAt(index)=='0'){
            dfs(s,index+1,tmp+'a');
        }else{
           for(int i = index ;i<Math.min(s.length(),index+2);i++){
            int increment = Integer.valueOf(s.substring(index,i+1));
            if(increment <= 25){
                a+=('a'+Integer.valueOf(s.substring(index,i+1)));
                dfs(s,i+1,tmp+a);
            }else {
                break;
            }
        }
        }
        
    }
}
```

* DP
  * the result end with index ```i``` equals the sum of two 
    * the current number as a single one
    * the current number and pre number as a Two-bits number when it is ```9<= x<=25```

The Space Opitimization

```java
class Solution {
    public int translateNum(int num) {
        String src = String.valueOf(num);
        int p = 0, q = 0, r = 1;
        for (int i = 0; i < src.length(); ++i) {
            p = q; 
            q = r; 
            r = 0;
            r += q;
            if (i == 0) {
                continue;
            }
            String pre = src.substring(i - 1, i + 1);
            if (pre.compareTo("25") <= 0 && pre.compareTo("10") >= 0) {
                r += p;
            }
        }
        return r;
    }
}
```



* DFS

```java
//time  0ms 
class Solution {
    public int translateNum(int num) {
        if (num<=9) {return 1;}
        //获取输入数字的余数，然后递归的计算翻译方法
        int ba = num%100;
        //如果大于9或者大于26的时候，余数不能按照2位数字组合，比如56，只能拆分为5和6；反例25，可以拆分为2和5，也可以作为25一个整体进行翻译。
        if (ba<=9||ba>=26) {return translateNum(num/10);}
        else  {return translateNum(num/10)+translateNum(num/100);}
    }
}
```

