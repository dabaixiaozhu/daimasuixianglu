# day3_203.移除链表元素、707.设计链表、206.反转链表

# 203.移除链表元素

试着用head直接移动试了下，发现 .next 很难去界定边界，这里使用的虚拟节点。

```java
public ListNode removeElements(ListNode head, int val) {
    // 这个用来获取最新的结果
    ListNode temp = new ListNode(-1, head);
    // 这个用来后移操作
    ListNode cur = temp;

    // 因为头时-1开始的，即是无效的，所以可以放心next
    while (cur.next != null) {
        if (cur.next.val == val) {
            cur.next = cur.next.next;
            // 注意要continue，因为比较的是 next，所以此时next变了，cur要保留在原地
            continue;
        }
        cur = cur.next;
    }

    return temp.next;
}
```



# 707.设计链表

主要就是边界值，可以先把写完大逻辑写完，之后再根据题目的要求，给get、delete去添加对应的条件，比如超过数组多少。

要确定函数的目的是什么，比如 addAtIndex，定义就是在index之前添加一个元素，**之前** 这两个字就很重要。

还有就是多用示例，比如addTail新增节点在尾部，传入的index是什么。

```java
public class MyLinkedList {

    private ListNode node;
    private int nodeIndex;

    public MyLinkedList() {
        this.node = new ListNode(-1);
        this.nodeIndex = -1;
    }

    /**
     * 比如 -1 -> 4 -> 5 -> 6 -> 7  index是2，需要返回的就是6，需要向后移动3次
     */
    public int get(int index) {
        // 边界值也通过示例来确定
        if (index < 0 || index > nodeIndex) {
            return -1;
        }

        ListNode temp = node;
        while (index-- >= 0) {
            temp = temp.next;
        }
        return temp.val;
    }

    public void addAtHead(int val) {
        addAtIndex(0, val);
    }

    public void addAtTail(int val) {
        addAtIndex(nodeIndex + 1, val);
    }

    /**
     * 注意这个函数的定义，是在index之前，插入一个元素，替代为新的index。
     * 比如在index=0之前插入一个元素，成为新的头节点
     * 比如在size之前（就是index+1）插入一个元素，成为新的尾节点
     */
    public void addAtIndex(int index, int val) {
        if (index > nodeIndex + 1) {
            return;
        }
        if (index < 0) {
            index = 0;
        }

        ListNode temp = node;

        // 移动到index元素的前一位，才能在next插入对应的val
        //  -1 -> 2 -> 4 -> 7 -> 9 index=2，表示要在index=2节点之前插入val，这里就是 7之前要插入val，所以要先定位到4，要比向后移动2位置
        while (index-- > 0) {
            temp = temp.next;
        }

        ListNode addNode = new ListNode(val);
        addNode.next = temp.next;
        temp.next = addNode;

        this.nodeIndex++;
    }

    public void deleteAtIndex(int index) {
        // 边界值直接看示例
        if (index < 0 || index > nodeIndex) {
            return;
        }
        ListNode temp = node;

        //  -1 -> 2 -> 4 -> 7 -> 9 index=2，表示要删除index=2这个节点，就是7，所以先要移动到4
        while (index-- > 0) {
            temp = temp.next;
        }

        temp.next = temp.next.next;
        this.nodeIndex--;
    }
}
```



# 206.反转链表

正常链表的解法。

```java
public ListNode reverseList(ListNode head) {
    
    if (head == null) {
        return null;
    }
    
    ListNode next = head.next;
    head.next = null;
    ListNode temp;
    
    while (next != null) {
        // 再保留一份剩余链表
        temp = next.next;
        next.next = head;
        head = next;
        next = temp;
    }
    
    return head;
}
```



重点是递归法，主要是要确定**递归函数的目的**，比如这里reverse函数的目的是让当前节点 指向前一个节点。

- 参数1：当前节点
- 参数2：前一个节点

```java
public ListNode reverseList(ListNode head) {
    if (head ==null) {
        return null;
    }
    // 目的 让当前节点 指向前一个节点
    return reverse(head,null);
}

private ListNode reverse(ListNode cur, ListNode prev) {
    if (cur ==null) {
        return prev;
    }
    ListNode next = cur.next;
    cur.next = prev;
    prev = cur;

    return reverse(next,prev);
}
```

