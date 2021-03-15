---
title: 算法-滑动窗口01-基本思想
date: 2021-03-13T09:25:42+08:00
draft: false
tags:
  - algorithms
---

有一类处理数组或list的问题，我们需要寻找或计算数组的某一固定长度的子数组。如：
> 给定一个数组，计算所有连续的 'K' 长度的子数组的平均数。

以如下数据为例：
```
Array: [1,3,2,6,-1,4,1,8,2], K=5 
```
那么，我们的解决的思路：
1. 前端的5个数值(下标从0到4)，平均值是 (1+3+2+6-1)/5 = 2.2
2. 然后计算后面5个数值(下标从1到5)，平均值是 (3+2+6-1+4)/5 = 2.8
3. 然后计算后面5个数值(下标从2到6)，平均值是 (2+6-1+4+1)/5 =2.4
以此类推。
对于上面这个输入的计算结果是：
```
Output: [2.2,2.8,2.4,3.6,2.8]
```

使用暴力算法来计算，方法为：累加给定的数组的每一个连续的5个元素的子数组，然后除以5计算平均值。

``` java
import java.util.Arrays;

public class AverageOfSubarrayOfSizeK {
    public static double[] findAverages(int K, int[] arr) {
        // 对于一个长度为 arr.length 的数组，存在 arr.length - K + 1 个长度为K的子数组
        double[] result = new double[arr.length - K + 1];
        for (int i = 0; i < arr.length - K + 1; i++) {
            double sum = 0;
            for (int j = i; j < i + K; j++) {
                sum += arr[j];
            }
            result[i] = sum / K;
        }
        return result;
    }

    public static void main(String[] args) {
        double[] result = findAveragesBySlidingWindow(5, new int[]{1, 3, 2, 6, -1, 4, 1, 8, 2});
        System.out.printf("Averages of subarrays of size K: " + Arrays.toString(result));
    }
}
```

时间复杂度： O(N * K)，N为输入数组的长度。

这个算法的问题在于，对于连续的两个子数组，其中重叠的部分会被计算两次。以上面提供的入参为例如图所示：

![index](https://cdn.jsdelivr.net/gh/pivst/images@master/PIC/index.24tfwtp0oci.svg)

我们可以通过滑动窗口来解决这个问题。如下图所示，我们将每个连续的子数组看作一个话滑动窗口。当我们计算下个子数组时，将窗口向后移动一个元素。所以，我们可以通过，先减去被移出窗口的元素的值，然后再加上刚被包括进窗口的元素值，来重用上步中累加出来的总和。这样我们可以省去遍历整个子数组来计算总和。因此，这个算法的复杂度达到 O(N)。

![sliding-window](https://cdn.jsdelivr.net/gh/pivst/images@master/PIC/sliding-window.3iq8w7sfjdfk.svg)

代码如下：

``` java
import java.util.Arrays;

public class AverageOfSubarrayOfSizeK {
    public static double[] findAveragesBySlidingWindow(int K, int[] arr) {
        // 对于一个长度为 arr.length 的数组，存在 arr.length - K + 1 个长度为K的子数组
        double[] result = new double[arr.length - K + 1];
        double windowSum = 0;
        int windowStart = 0;
        for (int windowEnd = 0; windowEnd < arr.length; windowEnd++) {
            windowSum += arr[windowEnd];
            if (windowEnd >= K - 1) {
                result[windowStart] = windowSum / K;
                windowSum -= arr[windowStart];
                windowStart++;
            }
        }
        return result;
    }

    public static void main(String[] args) {
        double[] result = findAveragesBySlidingWindow(5, new int[]{1, 3, 2, 6, -1, 4, 1, 8, 2});
        System.out.printf("Averages of subarrays of size K: " + Arrays.toString(result));
    }
}
```

未完待续...