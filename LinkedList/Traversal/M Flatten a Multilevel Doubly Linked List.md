## Flatten a Multilevel Doubly Linked List

> You are given a doubly linked list which in addition to the next and previous pointers, it could have a child pointer, which may or may not point to a separate doubly linked list. These child lists may have one or more children of their own, and so on, to produce a multilevel data structure, as shown in the example below.
>
> Flatten the list so that all the nodes appear in a single-level, doubly linked list. You are given the head of the first level of the list.
>
>  
>
> **Example 1:**
>
> ```
> Input: head = [1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]
> Output: [1,2,3,7,8,11,12,9,10,4,5,6]
> Explanation:
> 
> The multilevel linked list in the input is as follows:
> 
> 
> 
> After flattening the multilevel linked list it becomes:
> ```
>
> **Example 2:**
>
> ```
> Input: head = [1,2,null,3]
> Output: [1,3,2]
> Explanation:
> 
> The input multilevel linked list is as follows:
> 
>   1---2---NULL
>   |
>   3---NULL
> ```
>
> **Example 3:**
>
> ```
> Input: head = []
> Output: []
> ```
>
>  
>
> **How multilevel linked list is represented in test case:**
>
> We use the multilevel linked list from **Example 1** above:
>
> ```
>  1---2---3---4---5---6--NULL
>          |
>          7---8---9---10--NULL
>              |
>              11--12--NULL
> ```
>
> The serialization of each level is as follows:
>
> ```
> [1,2,3,4,5,6,null]
> [7,8,9,10,null]
> [11,12,null]
> ```
>
> To serialize all levels together we will add nulls in each level to signify no node connects to the upper node of the previous level. The serialization becomes:
>
> ```
> [1,2,3,4,5,6,null]
> [null,null,7,8,9,10,null]
> [null,11,12,null]
> ```
>
> Merging the serialization of each level and removing trailing nulls we obtain:
>
> ```
> [1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]
> ```
>
>  
>
> **Constraints:**
>
> - Number of Nodes will not exceed 1000.
> - `1 <= Node.val <= 10^5`

## Solution DFS

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node prev;
    public Node next;
    public Node child;
};
*/

class Solution {
    public Node flatten(Node head) {
        if(head==null) return head;
        dfs(head);
        return head;
    }
    /**
        merge need return the head and tail ,actually retur tail
    */
    private Node dfs(Node node){
        if(node==null) return null;
        Node head  = node ;
        while(node.next!=null){
            Node tail = null;
            if(node.child!=null){
                tail = dfs(node.child);
                Node n = node.next;
                n.prev = tail;
                tail.next = n;
                node.next = node.child;
                node.child.prev = node;
                node.child = null;
            }
            if(tail==null) node = node.next;
            else node =tail.next;
        }
        if(node.child!=null){
            dfs(node.child);
            node.next = node.child;
            node.child.prev = node;
            node.child = null;
        }
        while(node.next!=null) node=node.next;
        return node;
    }
}
```

* change the condition of the ``` while ``` 

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node prev;
    public Node next;
    public Node child;
};
*/

class Solution {
    public Node flatten(Node head) {
        if(head==null) return head;
        dfs(head);
        return head;
    }
    /**merge need return the head and tail ,actually retur tail */
    private Node dfs(Node node){
        Node pre =null;
        Node cur = node;
        while(cur!=null){
            if(cur.child!=null){
                Node tail = dfs(cur.child);
                Node n = cur.next;
                if(n!=null) n.prev=tail;
                tail.next =n;
                cur.next = cur.child;
                cur.child.prev=cur;
                cur.child=null;
                pre=tail;
                cur =n;
            }else{
                pre = cur;
                cur = cur.next;
            }
        }
        return pre;   
    }
    
}
```

## Solution Iterative

* one time ,we only flatten one child ,not the dfs

```java
class Solution {
    public Node flatten(Node head) {
        if( head == null) return head;
	// Pointer
        Node p = head; 
        while( p!= null) {
            /* CASE 1: if no child, proceed */
            if( p.child == null ) {
                p = p.next;
                continue;
            }
            /* CASE 2: got child, find the tail of the child and link it to p.next */
            Node temp = p.child;
            // Find the tail of the child
            while( temp.next != null ) 
                temp = temp.next;
            // Connect tail with p.next, if it is not null
            temp.next = p.next;  
            if( p.next != null )  p.next.prev = temp;
            // Connect p with p.child, and remove p.child
            p.next = p.child; 
            p.child.prev = p;
            p.child = null;
        }
        return head;
    }
}
```

