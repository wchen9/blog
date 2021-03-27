---
title: 算法-滑动窗口-习题03
date: 2021-03-26T13:51:34+08:00
draft: true
tags:
  - algorithms
---

### 习题描述
> 给定字符串 S ，找到最多有 K 个不同字符的最长子串的长度。
#### 示例1
```
Input: String="araaci", K=2
Output: 4
Explanation: The longest substring with no more than '2' distinct characters is "araa".
```
#### 示例2
```
Input: String="araaci", K=1
Output: 2
Explanation: The longest substring with no more than '1' distinct characters is "aa".
```
#### 示例3
```
Input: String="cbbebi", K=3
Output: 5
Explanation: The longest substrings with no more than '3' distinct characters are "cbbeb" & "bbebi".
```
### 解法
暴力算法，与上一题类似，从字符串第一个元素开始，双重遍历。使用 Set 来记录独立元素的个数，当 Set 的大小大于 K 时，记录当前子字符串的长度。重复遍历，获取最大的子字符串长度值。
``` java
public static int findLength(String str, int K) {
    if (str == null || str.length() == 0 || str.length() < K) {
        throw new IllegalArgumentException();
    }
    int longest = Integer.MIN_VALUE;
    for (int i = 0; i < str.length(); i++) {
        Set<Character> distinct = new HashSet<>();
        for (int j = i; j < str.length(); j++) {
            distinct.add(str.charAt(j));
            if (distinct.size() > K) {
                longest = Math.max(j - i, longest);
                break;
            }
        }
    }
    return longest != Integer.MIN_VALUE ? longest : -1;
}
```
时间复杂度： O(N ^ 2)

使用滑动窗口算法优化，思路如下：
1. 从数组开始遍历，将每个 char 作为 key 字符保存到 Map 中， Map 的 value 为字符的个数，直到 Map 中含有 K 个不同的元素；
2. 上一步中，满足条件后，作为当前的滑动窗口。记录窗口(子字符串)的当前长度作为当前最长的最长子串；
3. 然后从窗口头开始缩小当前窗口，直到 Map 中的字符个数小于 K (while Map.size() > K)，然后窗口向后滑动；
4. 窗口向后滑动，只要 Map.sise() <= K ，即将当前子串长度与之前记录的长度对比，取大值。

``` java
public static int findLengthBySlidingWindow(String str, int K) {
  if (str == null || str.length() == 0 || str.length() < K) {
      throw new IllegalArgumentException();
  }
  int windowStart = 0;
  int longest = 0;
  Map<Character, Integer> charFrequencyMap = new HashMap<>();
  for (int windowEnd = 0; windowEnd < str.length(); windowEnd++) {
      char rightChar = str.charAt(windowEnd);

      charFrequencyMap.put(rightChar, charFrequencyMap.getOrDefault(rightChar, 0) + 1);
      // 当 size > K 时，说明 map 中已经超过了 K 个 char 字符，需要缩小窗口，直到 size <= K
      while (charFrequencyMap.size() > K) {
          char leftChar = str.charAt(windowStart);
          charFrequencyMap.put(leftChar, charFrequencyMap.get(leftChar) - 1);
          if (charFrequencyMap.get(leftChar) == 0) {
              charFrequencyMap.remove(leftChar);
          }
          windowStart++;
      }
      longest = Math.max(longest, windowEnd - windowStart + 1);
  }
  return longest;
}
```
#### 时间复杂度
滑动窗口算法的时间复杂度为 O(N)。外圈的 for 循环遍历了所有的元素，而内圈的 while 循环仅处理一个元素。因此算法的时间复杂度为 O(N + N)，等价于 O(N)。
#### 空间复杂度
O(K)，因为 Map 中储存了最大 K + 1 个元素。