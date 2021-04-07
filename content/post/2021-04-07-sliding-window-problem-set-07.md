---
title: 算法-滑动窗口-习题07
date: 2021-04-07T11:20:05+08:00
draft: true
tags:
  - algorithms
---
### 习题描述
> 给定两个字符串 pattern 和 str，写一个函数来判断 str 是否包含 pattern 的排列。
#### 示例1
```
Input: String="oidbcaf", Pattern="abc"
Output: true
Explanation: The string contains "bca" which is a permutation of the given pattern.
```
#### 示例2
```
Input: String="odicf", Pattern="dc"
Output: false
Explanation: No permutation of the pattern is present in the given string as a substring.
```
#### 示例3
```
Input: String="bcdxabcdy", Pattern="bcdyabcdx"
Output: true
Explanation: Both the string and the pattern are a permutation of each other.
```
#### 示例4
```
Input: String="aaacb", Pattern="abc"
Output: true
Explanation: The string contains "acb" which is a permutation of the given pattern.
```
