---
title: 算法-滑动窗口-习题02
date: 2021-03-20T10:21:42+08:00
draft: false
tags:
  - algorithms
---

### 习题描述
> 给定一个整数数组和一个整数 'S'，寻找最小的连续子数组的长度，其和大于等于 'S'。如果不存在这样的子数组，则返回0。
#### 示例1
```
Input: [2, 1, 5, 2, 3, 2], S=7 
Output: 2
Explanation: The smallest subarray with a sum great than or equal to '7' is [5, 2].
```
#### 示例2
```
Input: [2, 1, 5, 2, 8], S=7 
Output: 1
Explanation: The smallest subarray with a sum greater than or equal to '7' is [8].
```
#### 示例3
```
Input: [3, 4, 1, 1, 6], S=8 
Output: 3
Explanation: Smallest subarrays with a sum greater than or equal to '8' are [3, 4, 1] or [1, 1, 6].
```

### 解法

最简单的暴力算法为，从数组最开始算，遍历到数组的最末元素累加，若遍历过程中满足条件，其累加值大于等于 S ，则将该子数组长度值赋给最短长度。然后子数组从下一位开始算，重复上述过程，若满足条件后，将最短长度与上一次满足的最短长度对比，取小值。

``` java
public static int findMinSubArray(int S, int[] arr) {
    int min = Integer.MAX_VALUE;
    boolean found = false;
    for (int i = 0; i < arr.length; i++) {
        int sum = 0;
        for (int j = i; j < arr.length; j++) {
            sum += arr[j];
            if (sum >= S) {
                found = true;
                min = Math.min(j - i + 1, min);
            }
        }
    }
    if (found) {
        return min;
    }
    return -1;
}
```

时间复杂度： O(N ^ 2)。

这个问题实质上与 [上一题](https://blog.wangc.org/2021-03-16-sliding-window-problem-set-01/) 比较类似，除了滑动窗口的大小不固定以外。解法如下：

1. 从数组第一位开始往后累加，直到满足条件即累加的和大于等于 S 。
2. 将上一步中满足条件的子数组作为窗口。记录当前窗口的尺寸为目前的最小值。
3. 当条件满足时，从子数组头开始缩小窗口的尺寸，直到窗口的和小于 S 。缩小窗口的同时，检查：
   - 比较是否当前窗口的长度与之前记录的长度，取小值；
   - 减去窗口的第一个元素值。



![2021-03-25_211452](https://cdn.jsdelivr.net/gh/pivst/images@master/PIC/2021-03-25_211452.723m80s4rq4g.png)

``` java
public static int findMinSubArrayBySlidingWindow(int S, int[] arr) {
   int windowSum = 0;
   int min = Integer.MAX_VALUE;
   int windowStart = 0;
   // 因窗口需要遍历到数组末尾，结束条件为 arr.length-1
   for (int windowEnd = 0; windowEnd < arr.length; windowEnd++) {
       // 当windowSum < S 时，窗口向后挪一位
       windowSum += arr[windowEnd];
       // windowSum >= S 时，条件满足，赋值最小窗口，同时缩小窗口直到 windowSum < S
       while (windowSum >= S) {
           min = Math.min(windowEnd - windowStart + 1, min);
           windowSum -= arr[windowStart];
           windowStart++;
       }
   }
   return min != Integer.MAX_VALUE ? min : 0;
}
```

#### 时间复杂度
滑动窗口算法的时间复杂度为 O(N)。外圈的 for 循环遍历了所有的元素，而内圈的 while 循环仅处理一个元素。因此算法的时间复杂度为 O(N + N)，等价于 O(N)。
#### 空间复杂度
O(1)。
