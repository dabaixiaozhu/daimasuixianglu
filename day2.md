# day2-977.有序数组的平方、209.长度最小的子数组、59.螺旋矩阵II

# 977.有序数组的平方

双指针法，因为最大的平方和要么是最左边 要么是最右边，把其中的最大值放入新的数组即可。

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        int[] result = new int[nums.length];
        int i = nums.length - 1;
        while (left <= right) {
            // 取左右值的最大平方和
            if (Math.pow(nums[left], 2) >= Math.pow(nums[right], 2)) {
                result[i--] = (int) Math.pow(nums[left++], 2);
            } else {
                result[i--] = (int) Math.pow(nums[right--], 2);
            }
        }
        return result;
    }
}
```





# 209.长度最小的子数组

双指阵法，还是要注意边界值，right相当于遍历过nums一轮。

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        int right = 0;
        int result = Integer.MAX_VALUE;
        int sum = 0;
        
        while (right < nums.length) {
            sum += nums[right++];
            // 注意在同一个right中，left可能是可以往后移动好几步的
            while (sum >= target) {
                result = Math.min(result, right - left);
                sum -= nums[left++];
            }
        }
        return result == Integer.MAX_VALUE ? 0 : result;
    }
}
```

# 59.螺旋矩阵II

主要是要考虑每次循环的开头在哪里，结尾在哪里，这里是使用左闭右开。

并且x、y的取名规则是，横轴和纵轴，对应的二维数组的参数却是第二个、第一个。

```java
public static int[][] generateMatrix(int n) {
    int[][] result = new int[n][n];
    // 每个数组中的数值
    int index = 1;
    
    // 要分为奇偶，奇数圈数是 n/2，再加中间一个(n^2,n^2)；偶数就是 n/2 圈
    if (n % 2 == 1) {
        result[n / 2][n / 2] = n * n;
    }
    
    // 注意是左闭右开
    for (int i = 0; i < n / 2; i++) {
        // 想象二维数组，这是x轴的变化（横轴），对应二维数组的第二个参数
        int x = i;
        // 想象二维数组，这是x轴的变化（纵轴），对应二维数组的第一个参数
        int y = i;
    
        // 上 注意比如n是5，一共能循环4次，切startx循环出来后是4，不需要再加1
        for (; x < n - i - 1; x++) {
            result[y][x] = index++;
        }
    
        // 右
        for (; y < n - i - 1; y++) {
            result[y][x] = index++;
        }
    
        // 下
        for (; x > i; x--) {
            result[y][x] = index++;
        }
    
        // 左
        for (; y > i; y--) {
            result[y][x] = index++;
        }
    }
    return result;
}
```



