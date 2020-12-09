Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.



Example:

Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
Note:

Although the above answer is in lexicographical order, your answer could be in any order you want.



> 这个solution2 跟1比真的是辣鸡，体现在几点。
>
> * 用```HashMap``` 做索引，感觉速度很慢，而且存放Key—Value对简直不能再蠢了。
> * 只想到String不能改变，没有利用 ```+``` 这个对字符串很好用的东西
> * Solution2 中因为是对```tmp```变量的引用，所以需要回溯，Solution1中则不需要，因为根本没有改变s的值

用点熟悉的Collection不好吗…

DFS: TIme O(4^n） Space（4^n+n）

## Solution1 DFS

```java
class Solution {
  List<String> res=new LinkedList<>();
    String a[]=new String[]{
            " ",//0
            "",//1
            "abc",//2
            "def",//3
            "ghi",//4
            "jkl",//5
            "mno",//6
            "pqrs",//7
            "tuv",//8
            "wxyz",//9
    };
    public void findCombiantion(String digits,int index,String s){
        if(index==digits.length()){
            res.add(s);
            return;
        }
       char b=digits.charAt(index); //这次要使用的数字
        String letter=a[b-'0'];
        for(int i=0;i<letter.length();i++){
            findCombiantion(digits,index+1,s+letter.charAt(i));
        }
    }
    public List<String> letterCombinations(String digits) {
   
        if(digits==null||digits.length()==0) return res;
        findCombiantion(digits,0,"");
        return res;
    }
}
```



## Solution 2 DFS

```java
class Solution {
    List<String> res=new LinkedList<>();
    HashMap<Character,String>  dic=new HashMap<>();

    public List<String> letterCombinations(String digits) {
        if(digits==null||digits.length()==0) return res;
        dic.put('2',"abc");
        dic.put('3',"def");
        dic.put('4',"ghi");
        dic.put('5',"jkl");
        dic.put('6',"mno");
        dic.put('7',"pqrs");
        dic.put('8',"tuv");
        dic.put('9',"wxyz");
        recursion(digits,0,new StringBuilder());
        return res;
    }
    private void  recursion(String digits,int index,StringBuilder tmp){
        if(tmp.length()==digits.length()) {
            res.add(""+tmp);
            return;
            }
       for(int i=0; i<dic.get(digits.charAt(index)).length();i++){
            tmp.append(dic.get(digits.charAt(index)).charAt(i)); //繁琐==垃圾
            recursion(digits,index+1,tmp);
            tmp.deleteCharAt(tmp.length()-1);
        }
    }
}
```

![image-20200212235617720](C:\Users\15524\AppData\Roaming\Typora\typora-user-images\image-20200212235617720.png)



##  Solution 3 BFS

Hint：两个滚动的数组，互相拷贝

* 三个for循环
* 有很多次的拷贝，所以效率也并不高

![image-20200213214730103](C:\Users\15524\AppData\Roaming\Typora\typora-user-images\image-20200213214730103.png)