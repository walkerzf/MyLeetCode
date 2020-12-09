##  Cheapest Flights Within K Stops

> There are `n` cities connected by `m` flights. Each flight starts from city `u` and arrives at `v` with a price `w`.
>
> Now given all the cities and flights, together with starting city `src` and the destination `dst`, your task is to find the cheapest price from `src` to `dst` with up to `k` stops. If there is no such route, output `-1`.
>
> ```
> Example 1:
> Input: 
> n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]
> src = 0, dst = 2, k = 1
> Output: 200
> Explanation: 
> The graph looks like this:
> 
> 
> The cheapest price from city 0 to city 2 with at most 1 stop costs 200, as marked red in the picture.
> Example 2:
> Input: 
> n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]
> src = 0, dst = 2, k = 0
> Output: 500
> Explanation: 
> The graph looks like this:
> 
> 
> The cheapest price from city 0 to city 2 with at most 0 stop costs 500, as marked blue in the picture.
> ```
>
>  
>
> **Constraints:**
>
> - The number of nodes `n` will be in range `[1, 100]`, with nodes labeled from `0` to `n`` - 1`.
> - The size of `flights` will be in range `[0, n * (n - 1) / 2]`.
> - The format of each flight will be `(src, ``dst``, price)`.
> - The price of each flight will be in the range `[1, 10000]`.
> - `k` is in the range of `[0, n - 1]`.
> - There will not be any duplicated flights or self cycles.

**Shortest path with at most k edges in a directed and weighted graph**

## Solution Map +Cost array DFS

* of course BFS will work also 

* it is important to cut the leaf,or will get **TLE**
* also need to keep an eyes on empty Map entry and empty array

```java
class Solution {
    int min =Integer.MAX_VALUE;
    Map<Integer,List<Integer>> m ;
    int c[][];
    int f [][]; 
    boolean visited [];
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int K) {
        // 0 - n-1 
        visited = new boolean[n];
        visited[src]=true;
        c =new int[n][n];
        m =new HashMap<>();
        f =flights;
        for(int i =0;i<flights.length;i++){
            if(!m.containsKey(flights[i][0])) m.put(flights[i][0],new ArrayList<>());
            m.get(flights[i][0]).add(flights[i][1]);
            c[flights[i][0]][flights[i][1]] =flights[i][2];
        }

        dfs(src,dst,K+1,0);
        return min==Integer.MAX_VALUE?-1:min; 
        
    }
    private void dfs(int index ,int dst,int count,int cost){
        
        if(count<0||(cost>=min&&index!=dst)) return ;
        if(count>=0&&index==dst) {
            min = Math.min(min ,cost);
            return;
        }
      //  if(visited[index]) return ;
        if(m.get(index)==null) return ;
        for(int num:m.get(index)){
            if(visited[num]) continue;
            visited[num]=true;
            dfs(num,dst,count-1,cost+c[index][num]);
            visited[num]=false;
        }
    }
}
```

* c++ can use 邻接表

```c++

class Solution {
public:
    int mn = 0x3f3f3f3f;

    int findCheapestPrice(int n, vector<vector<int>> &flights, int src, int dst, int K) {
        vector<vector<int >> m(n, vector<int>(n, 0x3f3f3f3f));
        for (vector<int> &f:flights) {
            m[f[0]][f[1]] = f[2];
        }
        vector<bool> visited(n, false);
        visited[src] = true;
        dfs(m, visited, src, dst, n, K + 1, 0);
        return mn == 0x3f3f3f3f ? -1 : mn;
    }

    void dfs(vector<vector<int >> &m, vector<bool> &visited, int index, int dst, int n, int count, int cost) {
        if (count >= 0 && index == dst) {
            mn = min(mn, cost);
            return;
        }
        if (count < 0 || cost >= mn) return;
        for (int i = 0; i < n; i++) {
            if (m[index][i] == 0x3f3f3f3f || visited[i]) continue;;
            visited[i] = true;
            dfs(m, visited, i, dst, n, count - 1, cost + m[index][i]);
            visited[i] = false;
        }
    }
};
```



## Solution Dijkstra

* Ref: https://leetcode.com/explore/challenge/card/june-leetcoding-challenge/540/week-2-june-8th-june-14th/3360/discuss/115541/JavaPython-Priority-Queue-Solution

```java
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
        //built weighted graph 
        Map<Integer, Map<Integer, Integer>> prices = new HashMap<>();
        for (int[] f : flights) {
            if (!prices.containsKey(f[0])) prices.put(f[0], new HashMap<>());
            prices.get(f[0]).put(f[1], f[2]);
        }
        Queue<int[]> pq = new PriorityQueue<>((a, b) -> (Integer.compare(a[0], b[0])));
        pq.add(new int[] {0, src, k + 1});
        //every time we choose  the minimum cost in current flight  to finish next step 
        while (!pq.isEmpty()) {
            int[] top = pq.remove();
            int price = top[0];
            int city = top[1];
            int stops = top[2];
            if (city == dst) return price;
            if (stops > 0) {
                Map<Integer, Integer> adj = prices.getOrDefault(city, new HashMap<>());
                for (int a : adj.keySet()) {
                    pq.add(new int[] {price + adj.get(a), a, stops - 1});
                }
            }
        }
        return -1;
    }
```



```c++
class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>> &flights, int src, int dst, int K) {
        unordered_map<int, vector<pair<int, int> >> m;
        for (vector<int> &f:flights) {
            m[f[0]].push_back(make_pair(f[1], f[2]));
        }
        queue<pair<int, int >> q;
        q.push(make_pair(src, 0));
        vector<int> pp(n, 0x3f3f3f3f);
        pp[src] = 0;
		// bfs can repeated visited 
        while (K-- >= 0 && !q.empty()) {
            int size = q.size();
            while (size > 0) {
                size--;
                pair<int, int> top = q.front();
                q.pop();
                for (pair<int, int> &a:m[top.first]) {
                    int next = a.first;
                    int cost = top.second + a.second;
                    if (pp[next] > cost) {
                        pp[next] = cost;
                    } else continue;
                    //reach the dst ,do not need to record next point
                    if (next == dst) pp[dst] = min(pp[dst], cost);
                    else q.push(make_pair(next, cost));
                }

            }
        }
        return pp[dst] == 0x3f3f3f3f ? -1 : pp[dst];
    }
};
```

```c++
class Solution {
public:
struct edge{
    int price;
    int dst;
    int dist;
    edge(int price_,int dst_,int dist_):price(price_),dst(dst_),dist(dist_){}
    bool operator<(const edge& other) const{
        return price>other.price;
    }
};
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
	vector<vector<pair<int, int>>> G(n);
	for (auto &c : flights) {
		G[c[0]].emplace_back(c[2], c[1]);
	}
	vector<int> price(n, 1e7);
	vector<int> dist(n, INT_MAX);
	price[src] = 0;
	dist[src] = 0;
	priority_queue<edge> Q;
	Q.emplace(0, src, -1);

	while (!Q.empty()) {
		int this_price = Q.top().price;
		int this_src = Q.top().dst;
		int this_dist = Q.top().dist;
		Q.pop();
        // attention .the dist restriction
		if (price[this_src] < this_price && this_dist >= dist[this_src]) {
			continue;
		}
		if (price[dst] != 1e7) { return price[dst]; }
		if (price[this_src] == 1e7) {
			price[this_src] = this_price;
			dist[this_src] = this_dist;
		}
		
		for (auto &c : G[this_src]) {
			if (this_dist < K) {
					Q.emplace(c.first+this_price, c.second, this_dist + 1);
			}
		}
	}
	return price[dst]==1e7?-1:price[dst];


    }
};
```



## Solution DP

```c++
class Solution
{
public:
    
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K)
    {        
        vector<vector<int>> dp(K+2, vector<int>(n, INT_MAX));
        //important condition definition
        //dp[i][j] = Dist to reach j using atmost i edges from src
        
        for(int i = 0; i <= K+1; i++)
        {
            dp[i][src] = 0; // Dist to reach src always zero
        }
        
        for(int i = 1; i <= K+1; i++)
        {
            for(auto &f: flights)
            {
                //Using indegree
                int u = f[0];
                int v = f[1];
                int w = f[2];
                
                if(dp[i-1][u] != INT_MAX)
                    dp[i][v] = min(dp[i][v], dp[i-1][u] + w);
            }
        }
        //k+1 step because k stop ,we need k +1 flight 
        return (dp[K+1][dst] == INT_MAX)? -1: dp[K+1][dst];
    }
};
```

```
class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        vector<vector<int>> dp(K+2,vector<int>(n,0x3f3f3f3f));
        for(int i = 0 ;i<=K+1;i++) dp[i][src] = 0;
        for(int i = 1 ;i<=K+1;i++){
            for(vector<int > &f:flights){
                int start = f[0];
                int end = f[1];
                int cost = f[2];
                if(dp[i-1][start]!=0x3f3f3f3f){
                    dp[i][end] = min(dp[i][end],dp[i-1][start]+cost);
                }
            }
        }
        return dp[K+1][dst]==0x3f3f3f3f?-1:dp[K+1][dst];
    }
};
```

