---
layout:     post
title:      leetcode contest 279
subtitle:   数据结构
date:       2022-02-08
author:     953max
header-img: 
catalog:      true
tags:
    - leetcode
    - contest

---

### 2164. Sort Even and Odd Indices Independently

You are given a **0-indexed** integer array `nums`. Rearrange the values of `nums` according to the following rules:

1. Sort the values at **odd indices** of `nums` in **non-increasing** order.
   - For example, if `nums = [4,**<u>1</u>**,2,<u><strong>3</strong></u>]` before this step, it becomes `[4,<u><strong>3</strong></u>,2,**<u>1</u>**]` after. The values at odd indices `1` and `3` are sorted in non-increasing order.
2. Sort the values at **even indices** of `nums` in **non-decreasing** order.
   - For example, if `nums = [<u><strong>4</strong></u>,1,<u><strong>2</strong></u>,3]` before this step, it becomes `[<u><strong>2</strong></u>,1,<u><strong>4</strong></u>,3]` after. The values at even indices `0` and `2` are sorted in non-decreasing order.

Return *the array formed after rearranging the values of* `nums`.

**Example 1:**

**Input:** nums = [4,1,2,3]
**Output:** [2,3,4,1]
**Explanation:** 
First, we sort the values present at odd indices (1 and 3) in non-increasing order.
So, nums changes from [4,**<u>1</u>**,2,**<u>3</u>**] to [4,<u><strong>3</strong></u>,2,**<u>1</u>**].
Next, we sort the values present at even indices (0 and 2) in non-decreasing order.
So, nums changes from [<u><strong>4</strong></u>,1,**<u>2</u>**,3] to [<u><strong>2</strong></u>,3,<u><strong>4</strong></u>,1].
Thus, the array formed after rearranging the values is [2,3,4,1].

**Example 2:**

**Input:** nums = [2,1]
**Output:** [2,1]
**Explanation:** 
Since there is exactly one odd index and one even index, no rearrangement of values takes place.
The resultant array formed is [2,1], which is the same as the initial array. 

**Constraints:**

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 100`
  
  
