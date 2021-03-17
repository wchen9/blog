---
title: 算法-滑动窗口-习题01
date: 2021-03-16T21:04:03+08:00
draft: false
tags:
  - algorithms
---

### 习题描述
> 给定一个整数数组 nums 和一个整数 'K' ，找到该数组的任意大小为 K 的连续子数组和的最大值
#### 示例1
```
Input: [2, 1, 5, 1, 3, 2], k=3 
Output: 9
Explanation: Subarray with maximum sum is [5, 1, 3].
```
#### 示例2
```
Input: [2, 3, 4, 1, 5], k=2 
Output: 7
Explanation: Subarray with maximum sum is [3, 4].
```
### 解法
最简单的暴力算法是计算给定数组的所有 K 长度的子数组之和来寻找最大的和。我们可以遍历给定数组的每一个元素，然后把后面的 K 个元素累加起来来计算子数组的和。如图所示：

![sum-of-subarray-by-brute-force](https://cdn.jsdelivr.net/gh/pivst/images@master/PIC/sum-of-subarray-by-brute-force.4f5jd1zjcc5c.png)

``` java
public class MaxSumSubArrayOfSizeK {
    public static int findMaxSumSubArray(int k, int[] arr) {
        int maxSum = 0, windowSum;
        // 最后一个窗口的下标 arr.length - k
        for (int i = 0; i <= arr.length - k; i++) {
            windowSum = 0;

            for (int j = i; j < i + k; j++) {
                windowSum += arr[j];
            }
            maxSum = Math.max(maxSum, windowSum);
        }

        return maxSum;
    }
}
```

这种算法的时间复杂度是 O(N * K)，N 为输入数组的长度。

然而，参考前文的思想，我们可以利用前面一个算出来的子数组之和来避免重复计算，代码如下：
``` java
public class MaxSumSubArrayOfSizeK {
    public static int findMaxSumSubArrayBySlidingWindow(int k, int[] arr) {
        int maxSum = 0, windowSum = 0;
        int windowStart = 0;
        for (int windowEnd = 0; windowEnd < arr.length; windowEnd++) {
            windowSum += arr[windowEnd];
            if (windowEnd >= k - 1) {
                maxSum = Math.max(maxSum, windowSum);
                windowSum -= arr[windowStart];
                windowStart++;
            }
        }
        return maxSum;
    }
}
```
这种算法的时间复杂度是 O(N)，空间复杂度是 O(1)。

