## Backspace String Compare

> Given two strings `S` and `T`, return if they are equal when both are typed into empty text editors. `#` means a backspace character.
>
> **Example 1:**
>
> ```json
> Input: S = "ab#c", T = "ad#c"
> Output: true
> Explanation: Both S and T become "ac".
> ```
>
> **Example 2:**
>
> ```c++
> Input: S = "ab##", T = "c#d#"
> Output: true
> Explanation: Both S and T become "".
> ```
>
> **Example 3:**
>
> ```c
> Input: S = "a##c", T = "#a#c"
> Output: true
> Explanation: Both S and T become "c".
> ```
>
> **Example 4:**
>
> ```java
> Input: S = "a#c", T = "b"
> Output: false
> Explanation: S becomes "c" while T becomes "b".
> ```
>
> **Note**:
>
> 1. `1 <= S.length <= 200`
> 2. `1 <= T.length <= 200`
> 3. `S` and `T` only contain lowercase letters and `'#'` characters.
>
> **Follow up:**
>
> - Can you solve it in `O(N)` time and `O(1)` space?

## Solution O（N） StringBuilder 

```java
//time 1ms 37.6MB
class Solution {
    public boolean backspaceCompare(String S, String T) {
        StringBuilder s=new StringBuilder();
        StringBuilder t=new StringBuilder();
        char ss[]=S.toCharArray();
        char st[]=T.toCharArray();
        for(int i=0;i<ss.length;i++){
            if(s.length()==0&&ss[i]=='#') continue;
            if(ss[i]!='#') s.append(ss[i]);
            else{
                s.deleteCharAt(s.length()-1);
            }
        }
        for(int i=0;i<st.length;i++){
            if(t.length()==0&&st[i]=='#') continue;
            if(st[i]!='#') t.append(st[i]);
            else{
                t.deleteCharAt(t.length()-1);
            }
        }
        return s.toString().equals(t.toString());
    }
}
```

## Solution Inpalce

* 从后往前，但是会碰到一个问题
* 这里是从后往前直到没有可以“无伤”退的
  * ```b+=(S.charAt(i)=='#')?1:-1;``` 这句话很关键

```java
class Solution {
    public boolean backspaceCompare(String S, String T) {
       //int b=0;
        int i=S.length()-1;
        int j=T.length()-1;
        while(true){
          for(int b=0;i>=0&&(b>0||S.charAt(i)=='#');i--){
              b+=(S.charAt(i)=='#')?1:-1;
          }
           for(int b=0;j>=0&&(b>0||T.charAt(j)=='#');j--){
              b+=(T.charAt(j)=='#')?1:-1;
          }
            if(i>=0&&j>=0&&S.charAt(i)==T.charAt(j)){
                i--;
                j--;
            }else{
                break;
            }
        }
        return i==-1&&j==-1;
    }
}
```

