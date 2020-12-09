## [Minimum Time to Collect All Apples in a Tree](https://leetcode-cn.com/problems/minimum-time-to-collect-all-apples-in-a-tree/)

> Given an undirected tree consisting of n vertices numbered from 0 to n-1, which has some apples in their vertices. You spend 1 second to walk over one edge of the tree. Return the minimum time in seconds you have to spend in order to collect all apples in the tree starting at vertex 0 and coming back to this vertex.
>
> The edges of the undirected tree are given in the array edges, where edges[i] = [fromi, toi] means that exists an edge connecting the vertices fromi and toi. Additionally, there is a boolean array hasApple, where hasApple[i] = true means that vertex i has an apple, otherwise, it does not have any apple.
>
> ```
> Example 1:
> ```
>
> ![img](https://assets.leetcode.com/uploads/2020/04/23/min_time_collect_apple_1.png)
>
> ```
> Input: n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,true,false,true,true,false]
> Output: 8 
> Explanation: The figure above represents the given tree where red vertices have an apple. One optimal path to collect all apples is shown by the green arrows.  
> Example 2:
> ```
>
> ![img](https://assets.leetcode.com/uploads/2020/04/23/min_time_collect_apple_2.png)
>
> ```
> Input: n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,true,false,false,true,false]
> Output: 6
> Explanation: The figure above represents the given tree where red vertices have an apple. One optimal path to collect all apples is shown by the green arrows.  
> Example 3:
> 
> Input: n = 7, edges = [[0,1],[0,2],[1,4],[1,5],[2,3],[2,6]], hasApple = [false,false,false,false,false,false,false]
> Output: 0
> ```
>
> ```
> Constraints:
> 
> 1 <= n <= 10^5
> edges.length == n-1
> edges[i].length == 2
> 0 <= fromi, toi <= n-1
> fromi < toi
> hasApple.length == n
> 
> 
> ```
>
> 

## Solution DFS+去重路径

```java
//time  217ms
class Solution {
    public int minTime(int n, int[][] edges, List<Boolean> hasApple) {
        Set<String> visited=new HashSet<>();
        Set<Integer> node=new HashSet<>();
        Map<Integer,List<Integer>> graph=new HashMap<>();
        for(int i=0;i<n;i++){
            graph.put(i,new LinkedList<>());
        }
        for(int i=0;i<edges.length;i++){
            graph.get(edges[i][0]).add(edges[i][1]);
            graph.get(edges[i][1]).add(edges[i][0]);
        }
        Set<Integer> a=new  HashSet<>();
        for(int i=0;i<hasApple.size();i++){
            if(hasApple.get(i)==true) a.add(i);
        }
        dfs(graph,visited,a,0,node,new LinkedList<String>());
        int res=visited.size()*2;
        return res;
        
        
    }
    private void dfs(Map<Integer,List<Integer>> graph,Set<String> visited,Set<Integer> apple,int index, Set<Integer> node,List<String> path){
        if(node.contains(index)) return;
        if(apple.contains(index)){
            for(String s:path){
                visited.add(s);
            }
        }
        node.add(index);
        for(int num:graph.get(index)){
            List<String> p=new LinkedList<>();
            for(String b:path){p.add(b);}
            p.add(index+""+num);
            dfs(graph,visited,apple,num,node,p);
        }
    }
}
```







```c++
class Solution {
public:
    int rs = 0;
    // dfs 返回当前节点根据后续节点bool值，从而确定的最终值
    bool dfs(int root, vector<vector<int>>& tree, vector<bool>& hasApple){
         for(int i = 0; i < tree[root].size(); i++){
            bool pt1 = dfs(tree[root][i],tree,hasApple);
            hasApple[root] =  hasApple[root] || pt1;
         }  
         if(root != 0 && hasApple[root] == true) rs+=1;                                                              
         return hasApple[root];
    }

    int minTime(int n, vector<vector<int>>& edges, vector<bool>& hasApple) {
        vector<int> row;
        vector<vector<int>> tree(n,row);
        // 根据边，构建二维数组多叉树，便于dfs
        for(int i =0;i < edges.size();i++){
            tree[edges[i][0]].push_back(edges[i][1]);
        }
        dfs(0,tree,hasApple);
        return 2*rs;
    }
};
```



## Solution DFS

* ```dfs```函数返回该节点往下的损耗
  * 它的孩子的孩子的损耗
  * 和孩子是否苹果

```java
//time  26ms
class Solution {
    public int minTime(int n, int[][] edges, List<Boolean> hasApple) {
        List<Integer> [] m=new ArrayList[n];
        //because parent > children always
        for(int i=0;i<n;i++){
            m[i]=new ArrayList<>();
        }
        for(int i=0;i<edges.length;i++){
            if(m[edges[i][0]]==null) m[edges[i][0]]=new ArrayList<>(); 
            m[edges[i][0]].add(edges[i][1]);
        }
        return dfs(m,0,hasApple);
    }
    private int dfs(List<Integer> [] m,int parent,List<Boolean> hasApple){
        int totalcost=0;
        //if(m[parent]==null) return hasApple.get(parent)==true?2:0;
        for(int child: m[parent]){
            //孩子的cost
            int cost=dfs(m,child,hasApple);
            //如果孩子自身为苹果或者子树上有苹果
            if(cost>0||hasApple.get(child)==true){
                totalcost+=2+cost;
            }
        }
        return totalcost;
    }
}
```

//如果没有parent >child的条件，在递归中要设置一个visited的Set，没访问到一个根节点，就标记为已访问，下一次碰到是可以跳过

```java
//time  64ms
class Solution {
    Set<Integer> visited=new HashSet<>();
    public int minTime(int n, int[][] edges, List<Boolean> hasApple) {
        List<Integer> [] m=new ArrayList[n];
        //because parent > children always
        for(int i=0;i<n;i++){
            m[i]=new ArrayList<>();
        }
        for(int i=0;i<edges.length;i++){
            m[edges[i][0]].add(edges[i][1]);
            m[edges[i][1]].add(edges[i][0]);
        }
        return dfs(m,0,hasApple);
    }
    private int dfs(List<Integer> [] m,int parent,List<Boolean> hasApple){
        int totalcost=0;
        visited.add(parent);
        //if(m[parent]==null) return hasApple.get(parent)==true?2:0;
        for(int child: m[parent]){
            if(visited.contains(child)) continue;
            int cost=dfs(m,child,hasApple);
            if(cost>0||hasApple.get(child)==true){
                totalcost+=2+cost;
            }
        }
        return totalcost;
    }
}
```

## Solution LCS

```java
//time  8ms
class Solution {
    public int minTime(int n, int[][] edges, List<Boolean> hasApple) {
        int res[]=new int[n];
        for(int i=edges.length-1;i>=0;i--){
            int x=edges[i][0];
            int y=edges[i][1];
            if(hasApple.get(y)==true){
                hasApple.set(x,true);
                res[x]+=(2+res[y]);
            }
        }
        return res[0];
    }
}
```

```java
//time  12ms
class Solution {
    public int minTime(int n, int[][] edges, List<Boolean> hasApple) {
        int res[]=new int[n];
        for(int i=edges.length-1;i>=0;i--){
            int x=edges[i][0];
            int y=edges[i][1];
            if(hasApple.get(y)==true){
                hasApple.set(x,true);
            }
        }
        int cnt=0;
        for(int i=1;i<n;i++){
            if(hasApple.get(i)==true) cnt++;
        }
        return cnt*2;
    }
}
```

