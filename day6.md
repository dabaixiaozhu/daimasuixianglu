# day6_242.有效的字母异位词、349. 两个数组的交集、202. 快乐数、1. 两数之和

# 242.有效的字母异位词

主要就是想到用一个26长度的int数组。

```java
if (s.length() != t.length()) {
        return false;
    }
    int[] ints = new int[26];

    // s和t必须长度相同的，如果不同则必定为false，这样就能在一个for循环中去进行增减
    for (int i = 0; i < s.length(); i++) {
        ints[s.charAt(i) - 97] = ints[s.charAt(i) - 97] + 1;
        ints[t.charAt(i) - 97] = ints[t.charAt(i) - 97] - 1;
    }

    for (int num : ints) {
        if (num != 0) {
            return false;
        }
    }

    return true;
```

# 349. 两个数组的交集

主要掌握stream流的用法，把数组转为List后再操作，要知道怎么再转为数组。

```java
public int[] intersection(int[] nums1, int[] nums2) {
    // boxed是将int转为Integer
    List<Integer> nums1List = Arrays.stream(nums1).boxed().collect(Collectors.toList());
    List<Integer> nums2List = Arrays.stream(nums2).boxed().collect(Collectors.toList());

    return nums1List.stream().filter(num -> nums2List.contains(num)).distinct().mapToInt(Integer::intValue).toArray();
}
```

# 202. 快乐数

主要就是题目那句，和如果重复出现，表示sum是有可能重复的，所以给while带来了截止条件。

```java
public boolean isHappy(int n) {
    List<Integer> sumList = new ArrayList<>();
    int sum = getSum(n);
    while (sum != 1) {
        if (sumList.contains(sum)) {
            return false;
        }
        sumList.add(sum);
        sum = getSum(sum);
    }

    return true;
}

private int getSum(int n) {
    int sum = 0;

    while (n / 10 > 0) {
        sum += Math.pow(n % 10, 2);
        n = (int) Math.floor((double) n / 10);
    }

    sum += Math.pow(n % 10, 2);
    return sum;
}
```

# 1. 两数之和

关键点就在于想到使用map，并且key是nums[i]，这样可以直接使用map的特性直接判断target-num[i] 是否存在。

还有需要考虑如果两个元素相等的情况，比如 3+3=6，所以if要放在第一个for循环里。

```java
 public int[] twoSum(int[] nums, int target) {
     int[] result = new int[2];
     HashMap<Integer, Integer> map = new HashMap<>();
     for (int i = 0; i < nums.length; i++) {
         if (map.containsKey(target-nums[i])){
             result[0] = i;
             result[1] = map.get(target-nums[i]);
             break;
         }
         map.put(nums[i], i);
     }

     return result;
 }
```

