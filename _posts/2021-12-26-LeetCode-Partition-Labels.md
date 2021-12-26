---
layout: post
title:  " LeetCode : 763. Partition Labels "
categories: LeetCode
author: bn-tw2020
---
* content
{:toc}

## [763. Partition Labels](https://leetcode.com/problems/partition-labels/)





### Problem

```
You are given a string s. We want to partition the string into as many parts as possible so that each letter appears in at most one part.
Return a list of integers representing the size of these parts.
```
 
---

### Example

```
Input: s = "ababcbacadefegdehijhklij"
Output: [9,7,8]
Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits s into less parts.
```

---

### Code

``` kotlin
class Solution {
    fun partitionLabels(s: String): List<Int> {
        var size = s.length
        var answer = mutableListOf<Int>()
        var alpha = IntArray(26)
        
        for(i in 0 until size) alpha[s[i] - 'a'] = i
        
        var farIndex = -1
        var startIndex = 0
        
        for(i in 0 until size) {
            if(alpha[s[i] - 'a'] > farIndex) {
                farIndex = alpha[s[i] - 'a']
            }
            
            if(i == farIndex) {
                answer.add(i - startIndex + 1)
                startIndex = i + 1
            }
        }
        return answer
    }
}
```

---

## Summary

* 문제를 푸는 방법과 요구사항을 파악하는데 어려워서 검색을 통해 해결할 수 있었다.

---

## Reference

* [참고자료](https://bcp0109.tistory.com/205)
