## Morris Traversal

>  ```morris```遍历是二叉树遍历算法的超强进阶算法，跟递归、非递归（栈实现）的空间复杂度，```morris```遍历可以将非递归遍历中的空间复杂度降为O(1)。从而实现时间复杂度为O(N)，而空间复杂度为O(1)的精妙算法。
> ```morris```遍历利用的是树的叶节点左右孩子为空（树的大量空闲指针），实现空间开销的极限缩减。

Ref:https://zhuanlan.zhihu.com/p/101321696

## PreOrder

```java
public static void morrisPre(Node head) {
    if(head == null){
        return;
    }
    Node cur = head;
    Node mostRight = null;
    while (cur != null){
        mostRight = cur.left;
        if(mostRight != null){
            while (mostRight.right !=null && mostRight.right != cur){
                mostRight = mostRight.right;
            }
            if(mostRight.right == null){
                mostRight.right = cur;
                System.out.print(cur.value+" ");
                cur = cur.left;
                continue;
            }else {
                mostRight.right = null;
            }
        }else {
            System.out.print(cur.value + " ");
        }
        cur = cur.right;
    }
    System.out.println();
}
```

## Inorder

```java
public static void morrisIn(Node head) {
    if(head == null){
        return;
    }
    Node cur = head;
    Node mostRight = null;
    while (cur != null){
        mostRight = cur.left;
        if(mostRight != null){
            while (mostRight.right !=null && mostRight.right != cur){
                mostRight = mostRight.right;
            }
            if(mostRight.right == null){
                mostRight.right = cur;
                cur = cur.left;
                continue;
            }else {
                mostRight.right = null;
            }
        }
        System.out.print(cur.value+" ");
        cur = cur.right;
    }
    System.out.println();
}
```

## PostOrder

```java
public static void morrisPos(Node head) {
       if(head == null){
           return;
       }
       Node cur = head;
       Node mostRight = null;
       while (cur != null){
           mostRight = cur.left;
           if(mostRight != null){
               while (mostRight.right !=null && mostRight.right != cur){
                   mostRight = mostRight.right;
               }
               if(mostRight.right == null){
                   mostRight.right = cur;
                   cur = cur.left;
                   continue;
               }else {
                   mostRight.right = null;
                   printEdge(cur.left);
               }
           }
           cur = cur.right;
       }
       printEdge(head);
       System.out.println();
   }
   public static void printEdge(Node node){
       Node tail =reverseEdge(node);
       Node cur = tail;
       while (cur != null ){
           System.out.print(cur.value+" ");
           cur =cur.right;
       }
       reverseEdge(tail);
   }
   public static Node reverseEdge(Node node){
       Node pre = null;
       Node next = null;
       while (node != null){
           next = node.right;
           node.right = pre;
           pre = node;
           node = next;
       }
       return pre;
   }
```

