---
title: 算法-滑动窗口-习题05
date: 2021-04-07T11:20:05+08:00
draft: true
tags:
  - algorithms
---
### 习题描述
> 给定一个仅有小写字母的字符串，假定允许用任意字符替换不超过 k 个字母，找到最长的替换后仅包含相同字符的子串。
#### 示例1
```
Input: String="aabccbb", k=2
Output: 5
Explanation: Replace the two 'c' with 'b' to have a longest repeating substring "bbbbb".
```
#### 示例2
```
Input: String="abbcb", k=1
Output: 4
Explanation: Replace the 'c' with 'b' to have a longest repeating substring "bbbb".
```
#### 示例3
```
Input: String="abccde", k=1
Output: 3
Explanation: Replace the 'b' or 'd' with 'c' to have the longest repeating substring "ccc".
```
