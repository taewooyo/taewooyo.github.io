---
layout: post
title:  " LeetCode : 1. Two Sum "
categories: LeetCode
author: bn-tw2020
---
* content
{:toc}

## [Two Sum](https://leetcode.com/problems/two-sum/)





### Problem

```
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.
You can return the answer in any order.
```
 
---

### Example

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1].
```

---

### Code

``` kotlin
class Solution {
    fun twoSum(nums: IntArray, target: Int): IntArray {
        
        for(i in 0 until nums.size) {
            for(j in i+1 until nums.size) {
                if(nums[i] + nums[j] == target) {
                    return intArrayOf(i, j)
                }
            }
        }
        return intArrayOf()
        
    }
}
```

---

## Summary

* 2021 하반기 취업 준비가 마무리 되고 다시 시작하는 알고리즘 문제이다.

  전부터 릿코드를 풀어보겠다고 생각만하다가 시작하기로 마음 먹었다.

---
