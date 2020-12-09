## x	

> Given a string, sort it in decreasing order based on the frequency of characters.
>
> **Example 1:**
>
> ```
> Input:
> "tree"
> 
> Output:
> "eert"
> 
> Explanation:
> 'e' appears twice while 'r' and 't' both appear once.
> So 'e' must appear before both 'r' and 't'. Therefore "eetr" is also a valid answer.
> ```
>
> 
>
> **Example 2:**
>
> ```
> Input:
> "cccaaa"
> 
> Output:
> "cccaaa"
> 
> Explanation:
> Both 'c' and 'a' appear three times, so "aaaccc" is also a valid answer.
> Note that "cacaca" is incorrect, as the same characters must be together.
> ```
>
> 
>
> **Example 3:**
>
> ```
> Input:
> "Aabb"
> 
> Output:
> "bbAa"
> 
> Explanation:
> "bbaA" is also a valid answer, but "Aabb" is incorrect.
> Note that 'A' and 'a' are treated as two different characters.
> ```

## Solution PriorityQueue

```java
import java.util.PriorityQueue;

class Solution {
    public String frequencySort(String s) {
        PriorityQueue<int []> q=new PriorityQueue<>((a, b)->b[1]-a[1]);
        int c[]=new int [256];
        for(int i=0;i<s.length();i++){
            c[s.charAt(i)]++;
        }
        for(int i=0;i<c.length;i++){
            if(c[i]==0) continue;
            q.add(new int[]{i,c[i]});
        }
        StringBuilder res=new StringBuilder();
        while(!q.isEmpty()){
            int top[]=q.poll();
            String a= String.valueOf((char)(top[0]));
            String tmp=a;
            for(int i=1;i<top[1];i++){
                tmp+=a+"";
            }
            res.append(tmp);
        }
        return res.toString();
    }
}
```

## Solution Map+Bucket Sort

```java
class Solution {
    public String frequencySort(String s) {
        Map<Character, Integer> map = new HashMap<>();
        //map to record character +frequence
        for (char c : s.toCharArray()) 
            map.put(c, map.getOrDefault(c, 0) + 1);
						
        List<Character> [] bucket = new List[s.length() + 1];
        //reverse the map to Bucket Array
        for (char key : map.keySet()) {
            int frequency = map.get(key);
            if (bucket[frequency] == null) bucket[frequency] = new ArrayList<>();
            bucket[frequency].add(key);
        }
				
        StringBuilder sb = new StringBuilder();
        for (int pos = bucket.length - 1; pos >= 0; pos--)
            if (bucket[pos] != null)
                for (char c : bucket[pos])
                    for (int i = 0; i < map.get(c); i++)
                        sb.append(c);

        return sb.toString();
    }
}
```

or we can use map   + map entry / the item in the map



```java


The logic is very similar to NO.347 and here we just use a map a count and according to the frequency to put it into the right bucket. Then we go through the bucket to get the most frequently character and append that to the final stringbuilder.

public class Solution {
    public String frequencySort(String s) {
        Map<Character, Integer> map = new HashMap<>();
        for (char c : s.toCharArray()) 
            map.put(c, map.getOrDefault(c, 0) + 1);
						
        List<Character> [] bucket = new List[s.length() + 1];
        for (char key : map.keySet()) {
            int frequency = map.get(key);
            if (bucket[frequency] == null) bucket[frequency] = new ArrayList<>();
            bucket[frequency].add(key);
        }
				
        StringBuilder sb = new StringBuilder();
        for (int pos = bucket.length - 1; pos >= 0; pos--)
            if (bucket[pos] != null)
                for (char c : bucket[pos])
                    for (int i = 0; i < pos; i++)
                        sb.append(c);

        return sb.toString();
    }
}

And we have normal way using PriorityQueue as follows:
according to user "orxanb", O(nlogm), m is the distinguish character, can be O(1) since only 26 letters. So the overall time complexity should be O(n), the same as the buck sort with less memory use.

public class Solution {
    public String frequencySort(String s) {
        Map<Character, Integer> map = new HashMap<>();
        for (char c : s.toCharArray())
            map.put(c, map.getOrDefault(c, 0) + 1);
						
        PriorityQueue<Map.Entry<Character, Integer>> pq = new PriorityQueue<>((a, b) -> b.getValue() - a.getValue());
        pq.addAll(map.entrySet());
				
        StringBuilder sb = new StringBuilder();
        while (!pq.isEmpty()) {
            Map.Entry e = pq.poll();
            for (int i = 0; i < (int)e.getValue(); i++) 
                sb.append(e.getKey());
        }
        return sb.toString();
    }
}
```



## Solution keep the origin sequence

more information to record the frequence

```java
    public static String frequencySort(String s) {
        Map<Character, int[]> map = new HashMap<>();
        for (int i = 0; i <s.length(); i++) {
            char c = s.charAt(i);
            if (!map.containsKey(c)) map.put(c, new int[]{1, i});
            else {
                int[] freqAndSeq = map.get(c);
                freqAndSeq[0]++;
                map.put(c, freqAndSeq);
            }
        }

        PriorityQueue<Map.Entry<Character, int[]>> pq = new PriorityQueue<>((a, b) ->
                a.getValue()[0] == b.getValue()[0] ? a.getValue()[1] - b.getValue()[1] : b.getValue()[0] - a.getValue()[0]);

        pq.addAll(map.entrySet());
        StringBuilder sb = new StringBuilder();
        while (!pq.isEmpty()) {
            Map.Entry<Character, int[]> e = pq.poll();
            for (int i = 0; i < e.getValue()[0]; i++)
                sb.append(e.getKey());
        }
        return sb.toString();
    }
```

