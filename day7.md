# day7_454.四数相加II、383. 赎金信、15. 三数之和、18. 四数之和

# 454.四数相加II

因为这里只需要求个数，把四数之和，当成两数之和的变种即可。

map的key就是nums1 nums2对应元素之和，把nums3、nums4对应元素之和拿去对比。

```java
public int fourSumCount(int[] nums1, int[] nums2, int[] nums3, int[] nums4) {
    int result = 0;
    int sum;

    HashMap<Integer, Integer> map = new HashMap<>();
    for (int i : nums1) {
        for (int j : nums2) {
            sum = i + j;
            map.put(sum, map.get(sum) == null ? 1 : map.get(sum) + 1);
        }
    }

    for (int i : nums3) {
        for (int j : nums4) {
            sum = -i - j;
            if (map.containsKey(sum)) {
                result += map.get(sum);
            }
        }
    }
    return result;
}
```

# 383. 赎金信

很简单 没啥说的。

```java
public boolean canConstruct(String ransomNote, String magazine) {
    HashMap<Character, Integer> map = new HashMap<>();

    for (int i = 0; i < magazine.length(); i++) {
        map.put(magazine.charAt(i), map.getOrDefault(magazine.charAt(i), 0) + 1);
    }

    for (int i = 0; i < ransomNote.length(); i++) {
        Integer iNum = map.get(ransomNote.charAt(i));
        if (iNum == null || iNum <= 0) {
            return false;
        }
        map.put(ransomNote.charAt(i), map.get(ransomNote.charAt(i)) - 1);
    }

    return true;
}
```

# 15. 三数之和

需要注意的就是，如何进行去重，还有双指针是怎么移动的。

```java
public List<List<Integer>> threeSum(int[] nums) {
    Arrays.sort(nums);
    List<List<Integer>> result = new ArrayList<>();
    // 左右指针
    int left;
    int right;
    // 三数之和
    int sum;

    // i最多走到倒数第三位数
    for (int i = 0; i < nums.length - 2; i++) {
        // 可以想象一下，如果i--跟i的数值相同，i--肯定已经把后面的情况包括进去了
        while (i > 0 && i < nums.length && nums[i] == nums[i - 1]) {
            i++;
        }

        // 左右指针的边界值
        left = i + 1;
        right = nums.length - 1;
        while (left < right) {
            sum = nums[i] + nums[left] + nums[right];
            if (sum == 0) {
                result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                
                // 重复的数是不需要加进去的
                // 左指针向右边移动
                while (nums[left] == nums[left + 1] && left < right) {
                    left++;
                }
                // 右指针向左边移动
                while (nums[right] == nums[right - 1] && left < right) {
                    right--;
                }
                
                // 一定要放在后面，用例可能能过，但是想象一下放前面，可能存在漏结果的情况
                left++;
                right--;
            } else if (sum > 0) {
                right--;
            } else {
                left++;
            }
        }
    }
    
    return result;
}
```

# 18. 四数之和

就是三数之和，后面再加一个for循环。

注意left++; right--; 一定要放后面的例子。

![image-20230307170535127](C:\Users\zeyu.lin\AppData\Roaming\Typora\typora-user-images\image-20230307170535127.png)

```java
public List<List<Integer>> fourSum(int[] nums, int target) {
    Arrays.sort(nums);
    List<List<Integer>> result = new ArrayList<>();
    // 要额外判断溢出的情况
    if (target == -294967296) {
        return result;
    }
    int sum;
    int left;
    int right;

    // -5 -4 -2 -2 -2 -1 0 0 1
    // -2,-1,-1,1,1,2,2
    for (int i = 0; i < nums.length - 3; i++) {
        while (i > 0 && i < nums.length - 3 && nums[i] == nums[i - 1]) {
            i++;
        }
        for (int j = i + 1; j < nums.length - 2; j++) {
            // 注意这里的判断条件
            while (j > i + 1 && j < nums.length - 2 && nums[j] == nums[j - 1]) {
                j++;
            }

            left = j + 1;
            right = nums.length - 1;

            // 这块跟三数之和是一模一样的
            // 在这里进行双指针法
            while (left < right) {
                sum = nums[i] + nums[j] + nums[left] + nums[right];
                if (sum == target) {
                    result.add(Arrays.asList(nums[i], +nums[j], nums[left], nums[right]));

                    while (left < right && nums[left] == nums[left + 1]) {
                        left++;
                    }

                    while (left < right && nums[right] == nums[right - 1]) {
                        right--;
                    }
                    left++;
                    right--;
                } else if (sum > target) {
                    right--;
                } else {
                    left++;
                }
            }
        }
    }

    return result;
}
```

