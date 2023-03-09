# day9_KMP

# 28. 实现 strStr()

就是KMP，我这里的next数组，下标保存的是**当前元素**的最长前后缀。

```java
/**
 * 获取最正宗的next数组
 */
public static int[] getNext(String str) {
    int[] next = new int[str.length()];
    // 首位固定是0的
    next[0] = 0;
    // 最长前缀的下一个元素的下标
    // 初始化的时候，由于没有最长前缀，所以下一个元素是第一个元素
    int temp = 0;

    // 这里的i就是除了首位元素，剩余的next数组的下标
    for (int i = 1; i < str.length(); i++) {
        // temp > 0 表示temp之前还有相等的前后缀
        while (temp > 0 && str.charAt(i) != str.charAt(temp)) {
            // 注意是i位置去跟temp比对，如果不匹配，要跳转到前一位的最长前缀的末尾的 **下一个元素** 去比较
            temp = next[temp - 1];
        }

        // 最终匹配成功，在最长前缀的基础上 +1
        if (str.charAt(i) == str.charAt(temp)) {
            temp++;
        }

        next[i] = temp;
    }

    return next;
}

/**
     * KMP
     */
public int strStr(String haystack, String needle) {
    int[] next = getNext(needle);
    int temp = next[0];
    for (int i = 0; i < haystack.length(); i++) {
        while (temp > 0 && haystack.charAt(i) != needle.charAt(temp)) {
            temp = next[temp - 1];
        }

        if (haystack.charAt(i) == needle.charAt(temp)) {
            if (temp == needle.length() - 1) {
                // 这个通过用例去写
                return i - needle.length()+1;
            }
            temp++;
        }
    }

    return -1;
}
```

# 459.重复的子字符串

双指阵法，还是很精巧的。

如果可以进行重复，想象一下，随着s的移动，在完全重复前（也就是 s+s），可以复现处s，表示可以由重复的子字符串组成。

```java
public boolean repeatedSubstringPattern(String s) {
    String temp = s;
    s = s.substring(1)+s.substring(0,s.length()-1);

    return s.contains(temp);
}
```

KMP更为精巧。字符串长度 % (字符串长度-最长相等前后缀)=0，以下是我验证的过程。

注意要判断最长相等前后缀=0的情况，单独拿出来判断

![image-20230309110144569](C:\Users\zeyu.lin\AppData\Roaming\Typora\typora-user-images\image-20230309110144569.png)

```java
public boolean repeatedSubstringPattern(String s) {
    int[] next = getNext(s);
    int max = next[s.length() - 1];
    if (max > 0 && s.length() % (s.length() - max) == 0) {
        return true;
    }
    return false;
}

private int[] getNext(String s) {
    int[] next = new int[s.length()];
    next[0] = 0;
    int temp = 0;

    for (int i = 1; i < s.length(); i++) {
        while (temp > 0 && s.charAt(i) != s.charAt(temp)) {
            temp=next[temp-1];
        }
        if (s.charAt(i) == s.charAt(temp)) {
            temp++;
        }
        next[i] = temp;
    }

    return next;
}
```

