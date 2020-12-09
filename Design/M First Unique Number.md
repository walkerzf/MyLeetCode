## First Unique Number

> You have a queue of integers, you need to retrieve the first unique integer in the queue.
>
> Implement the `FirstUnique` class:
>
> - `FirstUnique(int[] nums)` Initializes the object with the numbers in the queue.
> - `int showFirstUnique()` returns the value of **the first unique** integer of the queue, and returns **-1** if there is no such integer.
> - `void add(int value)` insert value to the queue.
>
>  
>
> **Example 1:**
>
> ```
> Input: 
> ["FirstUnique","showFirstUnique","add","showFirstUnique","add","showFirstUnique","add","showFirstUnique"]
> [[[2,3,5]],[],[5],[],[2],[],[3],[]]
> Output: 
> [null,2,null,2,null,3,null,-1]
> 
> Explanation: 
> FirstUnique firstUnique = new FirstUnique([2,3,5]);
> firstUnique.showFirstUnique(); // return 2
> firstUnique.add(5);            // the queue is now [2,3,5,5]
> firstUnique.showFirstUnique(); // return 2
> firstUnique.add(2);            // the queue is now [2,3,5,5,2]
> firstUnique.showFirstUnique(); // return 3
> firstUnique.add(3);            // the queue is now [2,3,5,5,2,3]
> firstUnique.showFirstUnique(); // return -1
> ```
>
> **Example 2:**
>
> ```
> Input: 
> ["FirstUnique","showFirstUnique","add","add","add","add","add","showFirstUnique"]
> [[[7,7,7,7,7,7]],[],[7],[3],[3],[7],[17],[]]
> Output: 
> [null,-1,null,null,null,null,null,17]
> 
> Explanation: 
> FirstUnique firstUnique = new FirstUnique([7,7,7,7,7,7]);
> firstUnique.showFirstUnique(); // return -1
> firstUnique.add(7);            // the queue is now [7,7,7,7,7,7,7]
> firstUnique.add(3);            // the queue is now [7,7,7,7,7,7,7,3]
> firstUnique.add(3);            // the queue is now [7,7,7,7,7,7,7,3,3]
> firstUnique.add(7);            // the queue is now [7,7,7,7,7,7,7,3,3,7]
> firstUnique.add(17);           // the queue is now [7,7,7,7,7,7,7,3,3,7,17]
> firstUnique.showFirstUnique(); // return 17
> ```
>
> **Example 3:**
>
> ```
> Input: 
> ["FirstUnique","showFirstUnique","add","showFirstUnique"]
> [[[809]],[],[809],[]]
> Output: 
> [null,809,null,-1]
> 
> Explanation: 
> FirstUnique firstUnique = new FirstUnique([809]);
> firstUnique.showFirstUnique(); // return 809
> firstUnique.add(809);          // the queue is now [809,809]
> firstUnique.showFirstUnique(); // return -1
> ```
>
>  
>
> **Constraints:**
>
> - `1 <= nums.length <= 10^5`
> - `1 <= nums[i] <= 10^8`
> - `1 <= value <= 10^8`
> - At most `50000` calls will be made to `showFirstUnique` and `add`.

## Solution DoubleLinkedList+Map

```java
class FirstUnique {
    class DoubleLinkedList{
        DoubleLinkedList pre;
        DoubleLinkedList next;
        int val;
        public DoubleLinkedList(int val){
            this.val=val;
        }
    }
    
    DoubleLinkedList head;
    DoubleLinkedList tail;
    Map<Integer,DoubleLinkedList> m=new HashMap<>(); 
    Set<Integer> removed=new HashSet<>();
    public FirstUnique(int[] nums) {
        head=new DoubleLinkedList(-1);
        tail=new DoubleLinkedList(-1);
        head.next=tail;
        tail.pre=head;
        for(int i=0;i<nums.length;i++){
            if(!m.containsKey(nums[i])){
                DoubleLinkedList node=new DoubleLinkedList(nums[i]);
                m.put(nums[i] , node);
                add(node);
            }else{
                if(removed.contains(nums[i])){
                    continue;
                } else{
                DoubleLinkedList d=m.get(nums[i]);
                remove(d);
                removed.add(nums[i]);
                }
            }
        }
    }
    
    private void remove(DoubleLinkedList node){
        node.pre.next=node.next;
        node.next.pre=node.pre;
    }
    private void add(DoubleLinkedList node){
        node.pre=tail.pre;
        tail.pre.next=node;
        node.next=tail;
        tail.pre=node;
    }
    
    public int showFirstUnique(){
        return head.next.val;
    }
    
    public void add(int value) {
        if(!m.containsKey(value)){
            DoubleLinkedList node =new DoubleLinkedList(value);
            m.put(value,node);
            add(node);
        }else{
             if(!removed.contains(value)) {
                DoubleLinkedList d=m.get(value);
                remove(d);
                removed.add(value);
            }
        }
    }
}

/**
 * Your FirstUnique object will be instantiated and called as such:
 * FirstUnique obj = new FirstUnique(nums);
 * int param_1 = obj.showFirstUnique();
 * obj.add(value);
 */
```

## Solution HashMap+HashLinkedSet

```java
 private Set<Integer> set = new LinkedHashSet<>(); 
    private Map<Integer, Integer> map = new HashMap<>();
    
    public FirstUnique(int[] nums) {
        for (int n : nums) {
            add(n);
        }
    }
    
    public int showFirstUnique() {
        for (int v : set) {
            return v;
        }
        return -1;
    }
    
    public void add(int value) {
        if (map.containsKey(value)) {
            set.remove(value);
        }else {
            set.add(value);
        }
        map.put(value, 1 + map.getOrDefault(value, 0));
    }
```

## Solution HashSet+LinkedHashSet

```java
   private Set<Integer> unique = new LinkedHashSet<>(); 
    private Set<Integer> total = new HashSet<>();
    
    public FirstUnique(int[] nums) {
        for (int n : nums) {
            add(n);
        }
    }
    
    public int showFirstUnique() {
        for (int v : unique) {
            return v;
        }
        return -1;
    }
    
    public void add(int value) {
        if (total.add(value)) {
            unique.add(value);
        }else {
            unique.remove(value);
        }
    }
```

