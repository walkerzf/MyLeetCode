## 题干

> Given a collection of numbers that might contain duplicates, return all possible unique permutations.
>
> Example:
>
> Input: [1,1,2]
> Output:
> [
>     [1,1,2],
>     [1,2,1],
>     [2,1,1]
>   ]
>   



* 这题与permutation 的区别就是存在重复元素，需要去除重复的排列。

## Solution

```java
class Solution {
        List<List<Integer>> res=new LinkedList<>();
        boolean used[];
    public List<List<Integer>> permuteUnique(int[] nums) {
        used=new boolean[nums.length];
        //sort
        Arrays.sort(nums);
        if(nums==null||nums.length==0) return res;
        recursion(nums,new LinkedList<Integer>()); 
        return res;
    }

    private void recursion(int []nums,List<Integer> tmp){
            if(tmp.size()==nums.length){
                res.add(new LinkedList(tmp));
                return;
            }
            for(int i=0;i<nums.length;i++){
                if(used[i]==true) continue;
                //for now , i think it is seen as the repeated ones can only in one sequence to 					//permutaiobn
                if(i-1>=0){
                    if(nums[i-1]==nums[i]&&used[i-1]==false) break;
                }
                used[i]=true;
                tmp.add(nums[i]);
                recursion(nums,tmp);
                used[i]=false;
                tmp.remove(tmp.size()-1);
            }
    }
}
```

```java
 for(int i=0;i<nums.length;i++){
                if(used[i]==true) continue;
                if(i-1>=0){
                    if(nums[i-1]==nums[i]&&used[i-1]==false) break;
                    //去重的这个部分，因为用了break，在第一个位置尝试第二个1的时候，就break出了整个循						//环	
                }
                used[i]=true;
                tmp.add(nums[i]);
                recursion(nums,tmp);
                used[i]=false;
                tmp.remove(tmp.size()-1);
            }
```

# [Permutation II LCCI](https://leetcode-cn.com/problems/permutation-ii-lcci/)

> Write a method to compute all permutations of a string whose charac­ ters are not necessarily unique. The list of permutations should not have duplicates.
>
> ```
> Example1:
> 
>  Input: S = "qqe"
>  Output: ["eqq","qeq","qqe"]
> Example2:
> 
>  Input: S = "ab"
>  Output: ["ab", "ba"]
> Note:
> 
> All characters are English letters.
> 1 <= S.length <= 9
> ```
>
> 

## Solution sort + 剪枝

```java
//time 2ms 
class Solution {
    boolean visited[];
    public String[] permutation(String S) {
        if(S.length()==0) return new String[]{};
        List<String> res =new ArrayList<>();
        char ss [] = S.toCharArray();
        Arrays.sort(ss);
        visited = new boolean[ss.length];
        Arrays.fill(visited,false);
        dfs(ss,res,new StringBuilder(),0);
        String ret []=new String[res.size()];
        int index = 0;
        System.out.println(res.size());
        for(String string:res){
            ret[index++] = string;
        }
        return ret;
    }
    private void dfs(char ss[],List<String > res,StringBuilder s,int count ){
        if(count == ss.length){
            //System.out.println(s);
            res.add(s.toString()); 
            return ;
        }
        for(int i =0;i<ss.length;i++){
            if(visited[i]) continue ;
            //write visited[i-1] =true  slower 
            //because it will back sequence 
            if(i>0&&visited[i-1]==false&&ss[i]==ss[i-1])  continue ;
            visited[i] = true;
            s.append(ss[i]);
            dfs(ss,res,s,count+1);
            visited[i] =false ;
            s.deleteCharAt(s.length()-1);
            
        }
    }
}
```

## Solution no sort 

* in the for loop ,we check the same character is or not used in the same loop 

  

  ```java
   List<String> list = new ArrayList<>();
      public String[] permutation(String S) {
  
          char[] chars = S.toCharArray();
          int[] used = new int[chars.length];
          StringBuilder sb = new StringBuilder();
          dfs(used,chars,sb);
          // 格式处理
          String[] res = new String[list.size()];
          for (int i = 0; i < list.size(); i++) {
              res[i] = list.get(i);
          }
          return res;
      }
  
      public void dfs(int[] used , char[] chars, StringBuilder sb){
          if (sb.length() == chars.length){
              list.add(sb.toString());
              return;
          }
          /**
           *                                              java
           *                j                                     a                      v                     a
           *        a       v        a                        j   v   a              j   a   a             j   a   v
           *      v   a   a   a    a   v
           *      a   v   a   a    v   a
           *  java\jaav\jvaa\jvaa\jaav\java
           *
           *  通过一个选择树可以发现问题。当有重复字母时,出现了重复的树
           *  这里通过一个技巧，这个技巧就是先排序后再处理，不过如果不看题解，我很难想到要先排序。
           *
           *  如果不先排序，那么在同一层遇到相同的字母时应该跳过，这个代码应该怎么写呢？
           *
           *
           *
           */
          /// 每次面临的选择
          for (int i = 0; i < chars.length; i++) {
              if (used[i] == 1) continue;
              /**
               *  used[j] == 0 且 chars[j] == cur 说明在同一层 此时遇到相等的元素需要剪枝，因为该分支和之前的分支重复了
               *  used[j] == 1 且 chars[j] == cur 不是同一层 此时即便遇到相等的元素也是不能剪枝的 比如第二层的a 和 第三层的a
               */
              char cur = chars[i];
              boolean valid = true;
              for (int j = 0; j < i; j++) {
                  //must check the used is false and same Character 
                  //when used one for same chaacter .it can be used in this level loop 
                  if (used[j] == 0 && chars[j] == cur){
                      /// 这个cur不能要
                      valid = false;
                  }
              }
              if (!valid) continue;
              sb.append(chars[i]);
              used[i] = 1;
              dfs(used, chars, sb);
              sb.deleteCharAt(sb.length()-1);
              used[i] = 0;
          }
      }
  
  ```

  

## Solution  Swap the Element 

```java
//time 1ms 
class Solution {
    public String[] permutation(String S) {
        char[] charArray = S.toCharArray();
        permutationImpl(charArray, 0, charArray.length-1);
        String[] ret = new String[list.size()];
        ret = list.toArray(ret);

        return ret;
    }

    HashSet<String> list = new HashSet<>();

    void permutationImpl(char[] charArray, int start, int end) {
        if(start == end) {
            //we use the Set to remove duplications 
            list.add(String.valueOf(charArray));
        } else {
            for(int i = start; i <= end; i++) {
                //we checke the same level loop the same character
                if(i > start && charArray[i] == charArray[i-1])
                    continue;
                //when we choose some character 
                //we swap it to the start positon
                //and start next dfs in start +1 position 
                swap(charArray, i, start);
                permutationImpl(charArray, start+1, end);
                swap(charArray, start, i);
            }
        }
    }

    void swap(char[] charArray, int x, int y) {
        char tmp = charArray[x];
        charArray[x] = charArray[y];
        charArray[y] = tmp;
    }
    
}
```

