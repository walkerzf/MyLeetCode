## Display Table of Food Orders in a Restaurant

> Given the array `orders`, which represents the orders that customers have done in a restaurant. More specifically `orders[i]=[customerNamei,tableNumberi,foodItemi]` where `customerNamei` is the name of the customer, `tableNumberi` is the table customer sit at, and `foodItemi` is the item customer orders.
>
> *Return the restaurant's “**display table**”*. The “**display table**” is a table whose row entries denote how many of each food item each table ordered. The first column is the table number and the remaining columns correspond to each food item in alphabetical order. The first row should be a header whose first column is “Table”, followed by the names of the food items. Note that the customer names are not part of the table. Additionally, the rows should be sorted in numerically increasing order.
>
>  
>
> **Example 1:**
>
> ```
> Input: orders = [["David","3","Ceviche"],["Corina","10","Beef Burrito"],["David","3","Fried Chicken"],["Carla","5","Water"],["Carla","5","Ceviche"],["Rous","3","Ceviche"]]
> Output: [["Table","Beef Burrito","Ceviche","Fried Chicken","Water"],["3","0","2","1","0"],["5","0","1","0","1"],["10","1","0","0","0"]] 
> Explanation:
> The displaying table looks like:
> Table,Beef Burrito,Ceviche,Fried Chicken,Water
> 3    ,0           ,2      ,1            ,0
> 5    ,0           ,1      ,0            ,1
> 10   ,1           ,0      ,0            ,0
> For the table 3: David orders "Ceviche" and "Fried Chicken", and Rous orders "Ceviche".
> For the table 5: Carla orders "Water" and "Ceviche".
> For the table 10: Corina orders "Beef Burrito". 
> ```
>
> **Example 2:**
>
> ```
> Input: orders = [["James","12","Fried Chicken"],["Ratesh","12","Fried Chicken"],["Amadeus","12","Fried Chicken"],["Adam","1","Canadian Waffles"],["Brianna","1","Canadian Waffles"]]
> Output: [["Table","Canadian Waffles","Fried Chicken"],["1","2","0"],["12","0","3"]] 
> Explanation: 
> For the table 1: Adam and Brianna order "Canadian Waffles".
> For the table 12: James, Ratesh and Amadeus order "Fried Chicken".
> ```
>
> **Example 3:**
>
> ```
> Input: orders = [["Laura","2","Bean Burrito"],["Jhon","2","Beef Burrito"],["Melissa","2","Soda"]]
> Output: [["Table","Bean Burrito","Beef Burrito","Soda"],["2","1","1","1"]]
> ```
>
>  
>
> **Constraints:**
>
> - `1 <= orders.length <= 5 * 10^4`
> - `orders[i].length == 3`
> - `1 <= customerNamei.length, foodItemi.length <= 20`
> - `customerNamei` and `foodItemi` consist of lowercase and uppercase English letters and the space character.
> - `tableNumberi `is a valid integer between `1` and `500`.

## Solution

被这个排序搞死了

```java
class Solution {
    public List<List<String>> displayTable(List<List<String>> orders) {
        List<List<String>> res =new LinkedList<>();
        List<String> food=new LinkedList<>();
        Map<String,Map<String,Integer>> m=new HashMap<>();
        for(List<String> o:orders){
            for(int i=1;i<o.size();i++){
                if(i==1&&!m.containsKey(o.get(i))) m.put(o.get(i),new HashMap<>());
                else{
                   if(!food.contains(o.get(2))) food.add(o.get(2));
                    if(!m.get(o.get(1)).containsKey(o.get(i))){
                        m.get(o.get(1)).put(o.get(i),1);
                    }else{
                        m.get(o.get(1)).put(o.get(i),m.get(o.get(1)).get(o.get(i))+1);
                    }
                }
            }
        }
        String []a=m.keySet().toArray(new String[]{});
        int []aa=new int[a.length];
        for(int i=0;i<aa.length;i++){
            aa[i]=Integer.valueOf(a[i]);
        }
        Arrays.sort(aa);
        for(int i=0;i<a.length;i++){
            a[i]=String.valueOf(aa[i]);
        }
        Collections.sort(food);
        List<String> head=new LinkedList<>();
        head.add("Table");
        String []ff=food.toArray(new String[]{});
        for(String tmp:food){ head.add(tmp);}
        res.add(head);
        for(int i=0;i<a.length;i++){
            List<String> row=new LinkedList<>();
            row.add(a[i]);
            Map<String,Integer> num=m.get(a[i]);
            for(String f:food){
                if(!num.containsKey(f)) row.add("0");
                else row.add(new String(String.valueOf(num.get(f))));
            }
            res.add(row);
        }
        return res;
    }
}
```

## Solution 优雅的排序

``` TreeSet ```  ``` TreeMap```

```java
class Solution {
    public List<List<String>> displayTable(List<List<String>> orders) {
        TreeSet<String> fod=  new TreeSet<>();
        TreeMap<Integer,Map<String,Integer>> tbs  = new TreeMap<>();
        for(List<String> od:orders){
            String my = od.get(2);
            fod.add(my);
            
            int tb = Integer.valueOf(od.get(1));
            
            if(!tbs.containsKey(tb)){
                Map<String,Integer> mp = new HashMap<>();
                tbs.put(tb,mp);
            }
            Map<String,Integer> cur = tbs.get(tb);
            int v = cur.getOrDefault(my,0)+1;
            cur.put(my,v);
        }
        List<List<String>> res = new ArrayList<>();
        List<String> first =new ArrayList<>();
        first.add("Table");
        for(String f:fod){
            first.add(f);
        }
        res.add(first);
        for(int key:tbs.keySet()){
             Map<String,Integer> cur = tbs.get(key);
            List<String> h =new ArrayList<>();
            h.add(""+key);
            
            for(int j=1;j<first.size();++j){
                String ck = first.get(j);
                int v = cur.getOrDefault(ck,0);
                h.add(""+v);
            }
            res.add(h);
        }
        return res;
        
        
        
    }
}
```

