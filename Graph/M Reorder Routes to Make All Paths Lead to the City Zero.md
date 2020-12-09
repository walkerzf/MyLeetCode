## Reorder Routes to Make All Paths Lead to the City Zero

> There are `n` cities numbered from `0` to `n-1` and `n-1` roads such that there is only one way to travel between two different cities (this network form a tree). Last year, The ministry of transport decided to orient the roads in one direction because they are too narrow.
>
> Roads are represented by `connections` where `connections[i] = [a, b]` represents a road from city `a` to `b`.
>
> This year, there will be a big event in the capital (city 0), and many people want to travel to this city.
>
> Your task consists of reorienting some roads such that each city can visit the city 0. Return the **minimum** number of edges changed.
>
> It's **guaranteed** that each city can reach the city 0 after reorder.
>
> **Example 1:**
>
> **![img](https://assets.leetcode.com/uploads/2020/05/13/sample_1_1819.png)**
>
> ```
> Input: n = 6, connections = [[0,1],[1,3],[2,3],[4,0],[4,5]]
> Output: 3
> Explanation: Change the direction of edges show in red such that each node can reach the node 0 (capital).
> ```
>
> **Example 2:**
>
> **![img](https://assets.leetcode.com/uploads/2020/05/13/sample_2_1819.png)**
>
> ```
> Input: n = 5, connections = [[1,0],[1,2],[3,2],[3,4]]
> Output: 2
> Explanation: Change the direction of edges show in red such that each node can reach the node 0 (capital).
> ```
>
> **Example 3:**
>
> ```
> Input: n = 3, connections = [[1,0],[2,0]]
> Output: 0
> ```
>
>  
>
> **Constraints:**
>
> - `2 <= n <= 5 * 10^4`
> - `connections.length == n-1`
> - `connections[i].length == 2`
> - `0 <= connections[i][0], connections[i][1] <= n-1`
> - `connections[i][0] != connections[i][1]`

## Solution Two List in and out

* we should clearly know it is a tree
*  only ``n -1 ``  edge ,because we sometimes need to reverse the edge direction , so we need to record the edge for each node  ,some edge we do not to reverse ,but we  need it to find next node , so we need the out information and in information

```java
//time  183ms
class Solution {
    public int minReorder(int n, int[][] connections) {
        //use LinkedList leads TLE LinkedList VS ArrayList
        List<Set<Integer>> in =new ArrayList<>();
        List<Set<Integer>> out =new ArrayList<>();
        for(int i=0;i<n;i++){
            in.add(new HashSet<>());
            out.add(new HashSet<>());
        }
        int start=0;
        int end=0;
        for(int i=0;i<connections.length;i++){
            start=connections[i][0];
            end = connections[i][1];
            in.get(end).add(start);
            out.get(start).add(end);
        }
        Queue<Integer> q=new LinkedList<>();
        int res=0;
        q.add(0);
        //similar to BFS
        while(!q.isEmpty()){
            int top=q.poll();
            //remove the relationship
            for(int num:in.get(top)){
                q.add(num);
                //remove from out
                out.get(num).remove(top);
            }
            //clear or tag it as visited
            //in.get(top).clear();
			
            //remove the relationshoip
            for(int num:out.get(top)){
                res++;
                q.add(num);
                //remove from in 
                in.get(num).remove(top);
            }
          //  out.get(top).clear();
        }
        return res;
    }
}
```

## Solution Two Map record in and out And Use visited to avoid already pass

* two Map record the in information and out information

```java
class Solution {
    public int minReorder(int n, int[][] connections) {
        Map<Integer,Set<Integer>> m=new HashMap<>();
        Map<Integer,Set<Integer>> cm=new HashMap<>();
        for(int i=0;i<n;i++){
            m.put(i,new HashSet<>());
            cm.put(i,new HashSet<>());
        }
        int start=0;
        int end=0;
        for(int i=0;i<connections.length;i++){
            start=connections[i][0];
            end=connections[i][1];
            m.get(start).add(end);
            cm.get(end).add(start);
        }
        boolean visited [] =new boolean[n];
        Queue<Integer> q=new LinkedList<>();
        q.add(0);
       // visited[0]=true;
        int res=0;
        while(!q.isEmpty()){
            int top=q.poll();
            if(visited[top]==true) continue;
            visited[top]=true;
            //we add some correct node
            for(int num:cm.get(top)){
                //BFS  avoid already pass 
                if(visited[num] ) continue;
                q.add(num);
                //visited[num]=true;
            }
            for(int num:m.get(top)){
                //BFS avoid already pass
                if(visited[num] ) continue;
                res++;
                q.add(num);
            }
        }
        return res;
    }
}
```

## Solution  DFS Fake Tree

* we create  the graph  (actually the Tree)
  * parent -> child positive 
  * child -> parent negative
* we dfs the Tree  from the index 0
  * we  tag the node as visited to avoid  computing 
  * we meet the negative is ok ,we do not to reverse the edge

```java
//time  36ms
class Solution {
    boolean visited [];
    public int minReorder(int n, int[][] connections) {
        visited=new boolean[n];
        List<List<Integer>> m=new ArrayList<>();
        for(int i=0;i<n;i++){
            m.add(new ArrayList<>());
        }
        for(int i=0;i<connections.length;i++){
            // xx->xx positive
            m.get(connections[i][0]).add(connections[i][1]);
            //xx  -<xx negative
            m.get(connections[i][1]).add(-connections[i][0]);
        }
        //dfs from zero 
        return dfs(m,0);
    }
    private int dfs(List<List<Integer>> m,int index){
        int res=0;
        visited[index]=true;
        //of course ,we deal with the positive one ,but  compute thinking about positive negative
        for(int num:m.get(index)){
            if(visited[Math.abs(num)]) continue;
            res= res+dfs(m,Math.abs(num))+((num>0)?1:0);
        }
        return res;
    }
}
```

## Solution  01BFS

```java
//time  46ms
class Solution {
    public int minReorder(int n, int[][] connections) {
        List<List<int []>> m=new ArrayList<>();
        for(int i=0;i<n;i++){
            m.add(new ArrayList<>());
        }
        boolean visited [] =new boolean [n];
        for(int i=0;i<connections.length;i++){
            m.get(connections[i][0]).add(new int[]{connections[i][1],1});
            m.get(connections[i][1]).add(new int[]{connections[i][0],0});
        }
        Queue<Integer> q=new LinkedList<>();
        q.add(0);
        int res=0;
        while(!q.isEmpty()){
            int top =q.poll();
            visited[top]=true;
            for(int a[]: m.get(top)){
                if(visited[a[0]]) continue;
                res+=a[1];
                q.add(a[0]);
            }
        }
        return res;
    }
}
```

### 01DFS

```java
//time  38ms
class Solution {
    boolean visited [];
    public int minReorder(int n, int[][] connections) {
        List<List<int []>> m=new ArrayList<>();
        for(int i=0;i<n;i++){
            m.add(new ArrayList<>());
        }
        visited =new boolean [n];
        for(int i=0;i<connections.length;i++){
            //weighted graph  0 or 1 
            m.get(connections[i][0]).add(new int[]{connections[i][1],1});
            m.get(connections[i][1]).add(new int[]{connections[i][0],0});
        }
        return dfs(m,0);
    }
    private int dfs(List<List<int []>> m ,int index){
        int res=0;
        visited[index]=true;
        for(int a[]:m.get(index)){
            if(visited[a[0]]) continue;
            res+=a[1];
            res+=dfs(m,a[0]);
        }
        return res;
    }
}
```

