# day8

# 344.反转字符串

就是temp换一下即可。

```java
public void reverseString(char[] s) {
    int length = s.length;
    for (int i = 0; i < length / 2; i++) {
        char tmp = s[i];
        s[i] = s[length-1-i];
        s[length-1-i]=tmp;
    }
}
```



# 541. 反转字符串II

边界条件根据错误的用例来修改，range的大小，在尾部不足的时候，需要修改。

注意边界范围。

```java
public String reverseStr(String s, int k) {
    StringBuilder result = new StringBuilder(s);
    // 表示每次要替换的尾元素
    int end;

    // 这里
    for (int i = 0; i < s.length(); i += k * 2) {
        // i + k <= s.length() 为什么要用等号，以及 s.length() - i 通过未通过的用例来修改
        int range = i + k <= s.length() ? k : s.length() - i;

        // 正常的调换即可
        for (int j = i; j < i + range / 2; j++) {
            char temp = s.charAt(j);
            end = i + range - 1 - (j - i);
            result.setCharAt(j, s.charAt(end));
            result.setCharAt(end, temp);
        }
    }

    return result.toString();
}
```

# 剑指Offer 05.替换空格

稍微看了下解析，但是leetcode还是能过就行。不纠结。

```java
public String replaceSpace(String s) {
    return s.replace(" ","%20");
}
```

# 151.翻转字符串里的单词

简单题 能过就行，双指针那个做法保留--||

```java
public String reverseWords(String s) {
    s = s.trim();
    // 注意如何去切割空白符
    String[] words = s.split("\\s+");
    StringBuilder result = new StringBuilder();
    for (int i = words.length - 1; i > 0; i--) {
        result.append(words[i]).append(" ");
    }

    result.append(words[0]);
    return result.toString();
}
```

# 剑指Offer58-II.左旋转字符串

如果直接append就很简单了。

如果要没有额外的空间，也是前面做的，先翻转前一部分，再翻转后一部分，最后整体再翻转一下。

```java
public String reverseLeftWords(String s, int n) {
    StringBuilder result = new StringBuilder(s);
    // 左闭右开
    reverse(result, 0, n);
    reverse(result, n, s.length());
    reverse(result, 0, s.length());
    return result.toString();
}

private void reverse(StringBuilder result, int start, int end) {
    for (int i = start; i < start + (end - start) / 2; i++) {
        char temp = result.charAt(i);
        result.setCharAt(i,result.charAt(end-1 - (i - start)));
        result.setCharAt(end -1- (i - start),temp);
    }
}
```

