# day04_24. 两两交换链表中的节点、19.删除链表的倒数第N个节点、面试题 02.07. 链表相交、142.环形链表II

# 24.两两交换链表中的节点

主要是把流程理顺清楚，节点间的指向要弄清楚。

- 保存未整理部分
- 整理前两个节点
- 为未整理部分的下一个while更新条件

```java
public ListNode swapPairs(ListNode head) {
    // 用于不断地拼接最新的temp
    ListNode hummy = new ListNode(-1);
    ListNode result = hummy;
    // 就是剩余还未调转的内容
    ListNode temp;

    while (head != null && head.next != null) {
        temp = head.next.next;
        // head跟head.next调换位置，那么新的头就是 head.next
        // 如果是第一次进入while，就是整条链表的头
        hummy.next = head.next;

        // 这两步是只关注前俩元素，进行调换
        head.next.next = head;
        head.next = temp;

        // hunmmy此时也要往后移动两步，拼接下一个while的新内容
        hummy = hummy.next.next;
        // 相当于转到temp，继续下一轮的比较
        head = head.next;
    }

    if (head != null) {
        hummy.next = head;
    }

    return result.next;
}
```



# 19.删除链表的倒数第N个节点

因为链表无法像数组直接获取到第几个元素，并且不知道链表的总体长度。

所以思想就是快慢指针，类似用相减法来算出删除的位置，重点是要举例子：

- 头元素被删除的情况
- 其余元素被删除的情况

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    // 可以去想一下，如果 1->2 ->3 删除的是3，即第1个元素，那么需要前面还有个-1，才方便把第一个元素去掉
    ListNode result = new ListNode(-1,head);
    ListNode temp = result;

    // 举一个例子，去看具体走了多少步
    while (n-- > 0) {
        head = head.next;
    }

    // 根据上面的head走了多少步，这里举例去看下是head!=null 还是head.next!=null等
    while (head != null) {
        head = head.next;
        temp = temp.next;
    }

    temp.next = temp.next.next;

    return result.next;
}
```

# 面试题 02.07. 链表相交

相交其实差的还是步数，要注意比较的不是val，要直接比较ListNode是否相同。

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    // 顶一下headA肯定是最长的，如果不是则与headB交换
    int lengthA = 0;
    int lengthB = 0;

    ListNode tempA = headA;
    ListNode tempB = headB;

    while (tempA != null) {
        tempA = tempA.next;
        lengthA++;
    }
    while (tempB != null) {
        tempB = tempB.next;
        lengthB++;
    }

    // 这里保证headA是更长的
    int step = lengthA - lengthB;
    if (step < 0) {
        ListNode temp = headB;
        headB = headA;
        headA = temp;
        step = Math.abs(step);
    }

    // 这样headA就必须先走step步
    while (step-- > 0) {
        headA = headA.next;
    }

    while (headA != null) {
        if (headA == headB) {
            return headA;
        }

        headA = headA.next;
        headB = headB.next;
    }

    return null;
}
```

# 142.环形链表II

主要还是数学问题：

1. 什么时候相遇，为什么在slow进入环内的**第一圈**，就必然会被fast追上，因为就算fast再慢，也能在slow走一圈时走两圈，所以公式必然是：

2*(x+y)=x+y+n(y+z)

2. 相遇后，如何求出x到底是多少？

x+y=n(y+z)

x=(n-1)(y+z)+z

第二个公式的意思是，x的值可以是我从相遇点开始，每次一步，可能旋转了n圈后，会跟一个从头开始每次一步的指针相遇，相遇点就是相交点，如果n越大，表示x越长。

```java
public ListNode detectCycle(ListNode head) {

    ListNode slow = head;
    ListNode fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        // 表示两个节点相遇了
        if (slow == fast) {
            fast = head;
            while (slow != fast) {
                fast = fast.next;
                slow = slow.next;
            }
            return slow;
        }
    }
    return null;
}
```

还有个注意点是，如果while中这么些，会超时的

```java
while (slow != fast) {
    fast = fast.next;
    slow = slow.next;
    // 主要就是这个
    if (slow == fast) {
    	return slow;
    }
}
```

