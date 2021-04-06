---
title: 算法-滑动窗口-习题04
date: 2021-03-27T14:58:25+08:00
draft: false
tags:
  - algorithms
---

### 习题描述
> 给定字符串，寻找最长的不含重复字符子串。
#### 示例1
```
Input: String="aabccbb"
Output: 3
Explanation: The longest substring without any repeating characters is "abc".
```
#### 示例2
```
Input: String="abbbb"
Output: 2
Explanation: The longest substring without any repeating characters is "ab".
```
#### 示例3
```
Input: String="abccde"
Output: 3
Explanation: Longest substrings without any repeating characters are "abc" & "cde".
```
### 解法
这个问题跟上一题类似，我们可以使用 Map 来记忆每个字符上次出现的位置。当遇到重复字符时，收缩窗口，保证在窗口中只存在唯一字符。
``` java
public static int findLength(String str) {

    int windowStart = 0, maxLength = 0;
    Map<Character, Integer> charIndexMap = new HashMap<>();

    for (int windowEnd = 0; windowEnd < str.length(); windowEnd++) {
        char rightChar = str.charAt(windowEnd);
        if (charIndexMap.containsKey(rightChar)) {
            // charIndexMap 中不仅包含当前窗口的字符，还包括之前被滑出的字符，
            // 因此需要 max 来获取当前重复 char 的下一个字符作为 start
            windowStart = Math.max(windowStart, charIndexMap.get(rightChar) + 1);
        }
        charIndexMap.put(rightChar, windowEnd);
        maxLength = Math.max(maxLength, windowEnd - windowStart + 1);
    }
    return maxLength;
}
```
#### 时间复杂度
O(N)。
#### 空间复杂度
O(K)，K 为字符串中的唯一字符数目。