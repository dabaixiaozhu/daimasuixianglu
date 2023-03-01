# day1-二分法：704. 二分查找、27. 移除元素

# 704 二分查找

注意点在right这里，取的是nums.length - 1还是nums.length。

```java
public int search(int[] nums, int target) {
    // 因为左闭右闭，所以取能得到的最小/最大下标
    int left = 0;
    int right = nums.length - 1;
    while (left <= right) {
        // 这个中间值比如左右值相等，就是左值
        // 如果差值是奇数的情况，就正好是中间的值
        // 如果差值是偶数的情况，就是偏左边更小的那个值
        // 不能直接 (left+right)/2 因为left+right可能会溢出
        int middle = left + (right - left) / 2;
        if (target < nums[middle]) { // 表示中间值还太大了，往左边找
            // midlle已经配对不上了，又因为右边是闭合的，所以取middle-1
            right = middle - 1;
        } else if (target > nums[middle]) {
            left = middle + 1;
        } else {
            return middle;
        }
    }

    return -1;
}
```

# 27 移除元素

两个指针，关键是用fast这个快指针（相当于一次for循环）遍历过数组，并且将fast对应的内容赋值给slow，如果满足等于val的条件，则跳过不赋值。

```java
public static int removeElement(int[] nums, int val) {
    // 两个指针同时出发
    int fast = 0;
    int slow = 0;
    while (fast < nums.length) {
        // 如果fast没有碰到跟val相同的元素，等于就把数组重新赋值了一遍
        if (nums[fast] != val) {
            nums[slow++] = nums[fast++];
            continue;
        }
        // 如果fast碰到跟val相同的元素，slow就停下脚步，让fast往前走一步，就起到了跳过相等元素的效果
        fast++;
    }
    return slow;
}
```

