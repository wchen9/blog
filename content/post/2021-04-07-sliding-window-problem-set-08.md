---
title: 算法-滑动窗口-习题08
date: 2021-04-07T11:20:05+08:00
draft: true
tags:
  - algorithms
---
### 习题描述
> 给定一个字符串 s 和一个非空字符串 p，找到 s 中所有是 p 的字母异位词的子串，返回这些子串的起始索引。

> 字符串只包含小写英文字母，并且字符串 s 和 p 的长度都不超过 20100。

>> 说明：
>> 字母异位词指字母相同，但排列不同的字符串。
>> 不考虑答案输出的顺序。

#### 示例1
```
Input: String="ppqp", Pattern="pq"
Output: [1, 2]
Explanation: The two anagrams of the pattern in the given string are "pq" and "qp".
```
#### 示例2
```
Input: String="abbcabc", Pattern="abc"
Output: [2, 3, 4]
Explanation: The three anagrams of the pattern in the given string are "bca", "cab", and "abc".
```

