## Reconstruct Itinerary

> Given a list of airline tickets represented by pairs of departure and arrival airports `[from, to]`, reconstruct the itinerary in order. All of the tickets belong to a man who departs from `JFK`. Thus, the itinerary must begin with `JFK`.
>
> **Note:**
>
> 1. If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string. For example, the itinerary `["JFK", "LGA"]` has a smaller lexical order than `["JFK", "LGB"]`.
> 2. All airports are represented by three capital letters (IATA code).
> 3. You may assume all tickets form at least one valid itinerary.
> 4. **One must use all the tickets once and only once.**
>
> **Example 1:**
>
> ```
> Input: [["MUC", "LHR"], ["JFK", "MUC"], ["SFO", "SJC"], ["LHR", "SFO"]]
> Output: ["JFK", "MUC", "LHR", "SFO", "SJC"]
> ```
>
> **Example 2:**
>
> ```
> Input: [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
> Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]
> Explanation: Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"].
>              But it is larger in lexical order.
> ```

## Greedy +  Hierholzer's algorithm

Ref: [One path graph](https://en.wikipedia.org/wiki/Eulerian_path#Fleury.27s_algorithm) 

```java
import java.util.*;

class Solution {
    Map<String,PriorityQueue<String>> m ;
     List<String> res ;
    public List<String> findItinerary(List<List<String>> tickets) {
        m = new HashMap<>();
        for(List s:tickets){
            String f = (String) s.get(0);
            String d = (String) s.get(1);
            if(!m.containsKey(f)){
                m.put(f,new PriorityQueue<>((a,b)->a.compareTo(b)));
                m.get(f).add(d);
            }else{
                m.get(f).add(d);
            }
        }
        res = new ArrayList<>();
        dfs("JFK");
        return res;
    }
    private void dfs(String tmp){
        while(m.get(tmp)!=null&&m.get(tmp).size()!=0){
            dfs(m.get(tmp).poll());
        }
        res.add(0,tmp);
    }
}
```

