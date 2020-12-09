##  Least Number of Unique Integers after K Removals

> Given an array of integers `arr` and an integer `k`. Find the *least number of unique integers* after removing **exactly** `k` elements**.**
>
> **Example 1:**
>
> ```
> Input: arr = [5,5,4], k = 1
> Output: 1
> Explanation: Remove the single 4, only 5 is left.
> ```
>
> **Example 2:**
>
> ```
> Input: arr = [4,3,1,1,3,3,2], k = 3
> Output: 2
> Explanation: Remove 4, 2 and either one of the two 1s or three 3s. 1 and 3 will be left.
> ```
>
>  
>
> **Constraints:**
>
> - `1 <= arr.length <= 10^5`
> - `1 <= arr[i] <= 10^9`
> - `0 <= k <= arr.length`

the idea is straight ,Remove k least frequent elements to make the remaining ones 

* very similar to **Sort Characters By Frequency**

## Solution  we use the TreeMap

* the creation of Map ,```else is forgotten```

```java
class Solution {
    public int findLeastNumOfUniqueInts(int[] arr, int k) {
        Map<Integer,Integer> m =new HashMap<>();
        for(int i = 0;i<arr.length ;i++){
            if(!m.containsKey(arr[i])) m.put(arr[i],1);
            else m.put(arr[i],m.get(arr[i])+1);
        }
        //res number
        int res = m.keySet().size();
        TreeMap<Integer,List<Integer>> mm = new TreeMap<>();
        for(int num:m.keySet()){
            if(!mm.containsKey(m.get(num))) mm.put(m.get(num),new ArrayList<>());
            mm.get(m.get(num)).add(num);
        }
        for(int num:mm.keySet()){
            if(k>0){
            if(k>=(mm.get(num).size()*num)){
                res = res -mm.get(num).size();
                k-= mm.get(num).size()*num;
            }else{
                res -= (k/num);
                k =k%num;
                break;
            }
        }
    }
        return res;
    }
}
```

## Solution PriorityQueue

* we can use the frequency of Integer to sort the Integer and remove

