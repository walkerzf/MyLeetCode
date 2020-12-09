## LRU Cache

> Design and implement a data structure for [Least Recently Used (LRU) cache](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU). It should support the following operations: `get` and `put`.
>
> `get(key)` - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
> `put(key, value)` - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.
>
> The cache is initialized with a **positive** capacity.
>
> **Follow up:**
> Could you do both operations in **O(1)** time complexity?
>
> **Example:**
>
> ```
> LRUCache cache = new LRUCache( 2 /* capacity */ );
> 
> cache.put(1, 1);
> cache.put(2, 2);
> cache.get(1);       // returns 1
> cache.put(3, 3);    // evicts key 2
> cache.get(2);       // returns -1 (not found)
> cache.put(4, 4);    // evicts key 1
> cache.get(1);       // returns -1 (not found)
> cache.get(3);       // returns 3
> cache.get(4);       // returns 4
> ```

## Solution  双向链表+Map

* 需要O（1）的复杂度，能实现这种的数据结构挺少的
  * 试一试各种数据结构把

```java
class LRUCache {
    class doubleListNode{
        doubleListNode pre;
        doubleListNode next;
        int val;
        int key;
        public doubleListNode(int key,int val){
            this.key=key;
            this.val=val;
        }
    }
    
    int max;
    int currentCount;
    doubleListNode head;
    doubleListNode tail;
    Map<Integer,doubleListNode> m;
    public LRUCache(int capacity) {
        max=capacity;
        currentCount=0;
        head=new doubleListNode(-1,-1);
        tail=new doubleListNode(-1,-1);
        head.next=tail;
        tail.pre=head;
        m=new HashMap<>();
    }
    
    public int get(int key) {
        if(m.containsKey(key)) {
            remove(m.get(key));
            insertAtLast(m.get(key));
            return m.get(key).val;
        }else return -1;
    }
    
    public void put(int key, int value) {
        doubleListNode node=new doubleListNode(key,value);
       if(m.containsKey(key)){
           remove(m.get(key));
           m.put(key,node);
           insertAtLast(node);
       }else{
           if(currentCount<max){
               currentCount++;
               m.put(key,node);
               insertAtLast(node);
           }else{
               m.remove(head.next.key,m.get(head.next.key));
               remove(head.next);
               m.put(key,node);
               insertAtLast(node);
           }
       }
    }
    
    private void remove(doubleListNode node){
        node.pre.next=node.next;
        node.next.pre=node.pre;
    }
    private void insertAtLast(doubleListNode  node){
        tail.pre.next=node;
        node.pre=tail.pre;
        node.next=tail;
        tail.pre=node;
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */ 
```

