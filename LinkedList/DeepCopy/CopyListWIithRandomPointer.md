## [Copy List with Random Pointer](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

## Solution TwoMap

```java
class Solution {
    public Node copyRandomList(Node head) {
        if(head==null) return null;
        Map<Node,Integer> old = new HashMap<>();
        Map<Integer ,Node> m =new HashMap<>();
        Node copy = new Node(head.val);
        Node dummy = head;
        Node res = copy;
        m.put(0,copy);
        old.put(head,0);
        int index =0;
        while(head.next!=null){
            Node n = new Node(head.next.val);
            copy.next = n;
            index++;
            m.put(index,n);
            old.put(head.next,index);
            copy = copy.next;
            head =head.next;
        }
        index = 0;
        while(dummy!=null){
            if(dummy.random==null) m.get(index).random=null;
            else{
                m.get(index).random= m.get(old.get(dummy.random));
            }
            index++;
            dummy = dummy.next;
        }
        return res;
    }
}
```

## Map+DFS

```java

class Solution {
    Map<Node, Node> visited = new HashMap<>();

    public Node copyRandomList(Node head) {
        if (head == null) {
            return null;
        }
        if (this.visited.containsKey(head)) {
            return this.visited.get(head);
        }
        Node clone = new Node(head.val);
        this.visited.put(head, clone);
        clone.next = copyRandomList(head.next);
        clone.random = copyRandomList(head.random);
        return clone;
    }
}
```

## Solution O(N) Iteration

```java

public class Solution {
  // Visited dictionary to hold old node reference as "key" and new node reference as the "value"
  HashMap<Node, Node> visited = new HashMap<Node, Node>();

  public Node getClonedNode(Node node) {
    // If the node exists then
    if (node != null) {
      // Check if the node is in the visited dictionary
      if (this.visited.containsKey(node)) {
        // If its in the visited dictionary then return the new node reference from the dictionary
        return this.visited.get(node);
      } else {
        // Otherwise create a new node, add to the dictionary and return it
        this.visited.put(node, new Node(node.val, null, null));
        return this.visited.get(node);
      }
    }
    return null;
  }

  public Node copyRandomList(Node head) {

    if (head == null) {
      return null;
    }

    Node oldNode = head;

    // Creating the new head node.
    Node newNode = new Node(oldNode.val);
    this.visited.put(oldNode, newNode);

    // Iterate on the linked list until all nodes are cloned.
    while (oldNode != null) {
      // Get the clones of the nodes referenced by random and next pointers.
      newNode.random = this.getClonedNode(oldNode.random);
      newNode.next = this.getClonedNode(oldNode.next);

      // Move one step ahead in the linked list.
      oldNode = oldNode.next;
      newNode = newNode.next;
    }
    return this.visited.get(head);
  }
}


```

## O（1）空间

```java
class Solution {
    public Node copyRandomList(Node head) {
        if(head==null) return null;
        Node ptr=head;
        //gaoding  next 
        //we skew the old List ,add the new Node into the old Lost
        while(ptr!=null){
            Node newNode=new Node(ptr.val,null,null);
            newNode.next=ptr.next;
            ptr.next=newNode;
            ptr=newNode.next;
        }
        ptr=head;
        //we assign the random Pointer
        while(ptr!=null){
            if(ptr.random!=null)    ptr.next.random=ptr.random.next;
            else ptr.next.random=null;
            ptr=ptr.next.next;
        }
        ptr=head;
        Node res=ptr.next;
        //we get rid the copy List
        while(ptr!=null){
            Node newNode=ptr.next;
            ptr.next=newNode.next;
            if(ptr.next!=null)    newNode.next=ptr.next.next;
            else newNode.next=null;
            ptr=ptr.next;
            newNode=newNode.next;
        }
        return res;
    }
}
```

