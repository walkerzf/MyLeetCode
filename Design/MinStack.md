## 155  MinStack

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

- push(x) -- Push element x onto stack.
- pop() -- Removes the element on top of the stack.
- top() -- Get the top element.
- getMin() -- Retrieve the minimum element in the stack.

 

**Example:**

```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.
```

 

## Solution Two Stack

```c++
另一个栈用来存储至此小于等于min的值
	class MinStack {
private:
    stack<int> s1;
    stack<int> s2;
public:
    void push(int x) {
	    s1.push(x);
	    if (s2.empty() || x <= getMin())  s2.push(x);	    
    }
    void pop() {
	    if (s1.top() == getMin())  s2.pop();
	    s1.pop();
    }
    int top() {
	    return s1.top();
    }
    int getMin() {
	    return s2.top();
    }
};
```

```java
//time 7ms
class MinStack {
    Stack<Integer> s1;
    Stack<Integer> s2;
    /** initialize your data structure here. */
    public MinStack() {
        s1=new Stack<>();
        s2=new Stack<>();
    }
    
    public void push(int x) {
        s1.push(x);
        if(s2.isEmpty()||s2.peek()>=x){
            s2.push(x);
        }
    }
    
    public void pop() {
        //如果直接利用peek ，可能会遇到两个Integer比较，bug
        int num=s1.pop();
        if(num==s2.peek()){
            s2.pop();
        }
    }
    
    public int top() {
        return s1.peek();
    }
    
    public int getMin() {
       
        return s2.peek();
    }
}
```



## Solution One Stack

### 节点带有最小值信息

```java
class MinStack {
    class Node{
        int val;
        int min;
        public Node(int val){
            this.val=val;
        }
    }
    Stack<Node> s;
    
    /** initialize your data structure here. */
    public MinStack() {
        s=new Stack<>();
    }
    
    public void push(int x) {
        Node p=new Node(x);
        if(s.isEmpty()) p.min=x;
        else p.min=Math.min(s.peek().min,x);
        s.push(p);
    }
    
    public void pop() {
        s.pop();
    }
    
    public int top() {
        return s.peek().val;
    }
    
    public int getMin() {
        return s.peek().min;
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```



### 一次性存入两个信息，一个该节点信息，一个最小值

```java
class MinStack {
    int min = Integer.MAX_VALUE;
    Stack<Integer> stack = new Stack<Integer>();
    public void push(int x) {
        // only push the old minimum value when the current 
        // minimum value changes after pushing the new value x
        if(x <= min){          
            stack.push(min);
            min=x;
        }
        stack.push(x);
    }

    public void pop() {
        // if pop operation could result in the changing of the current minimum value, 
        // pop twice and change the current minimum value to the last minimum value.
        if(stack.pop() == min) min=stack.pop();
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return min;
    }
}
```



## Solution LinkedList

* 从头增加链表元素，与第一种方法类似
* 也是在链表的每个Node中增加一个属性，记录从头到现在的最小值，
* 采用头插法，因为试单向链表，要找上一个元素不容易，找下一个元素还可以

```java
class MinStack {
    private Node head;
    
    public void push(int x) {
        if(head == null) 
            head = new Node(x, x);
        else 
            head = new Node(x, Math.min(x, head.min), head);
    }

    public void pop() {
        head = head.next;
    }

    public int top() {
        return head.val;
    }

    public int getMin() {
        return head.min;
    }
    
    private class Node {
        int val;
        int min;
        Node next;
        
        private Node(int val, int min) {
            this(val, min, null);
        }
        
        private Node(int val, int min, Node next) {
            this.val = val;
            this.min = min;
            this.next = next;
        }
    }
}
```

