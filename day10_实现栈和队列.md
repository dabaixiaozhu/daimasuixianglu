# day10_实现栈和队列

# 232.用栈实现队列

就是用ArrayDeque。

```java
class MyQueue {

   private ArrayDeque<Integer> queue;

    public MyQueue() {
        this.queue = new ArrayDeque<>();
    }

    public void push(int x) {
        queue.addLast(x);
    }

    public int pop() {
        return queue.pollFirst();
    }

    public int peek() {
        return queue.peekFirst();
    }

    public boolean empty() {
        return queue.isEmpty();
    }
}
```

# 225. 用队列实现栈

就是用ArrayDeque。

```java
class MyStack {
    private ArrayDeque<Integer> stack;

    public MyStack() {
        this.stack = new ArrayDeque<>();
    }

    public void push(int x) {
        stack.addLast(x);
    }

    public int pop() {
        return stack.pollLast();
    }

    public int top() {
        return stack.peekLast();
    }

    public boolean empty() {
        return stack.isEmpty();
    }
}
```

