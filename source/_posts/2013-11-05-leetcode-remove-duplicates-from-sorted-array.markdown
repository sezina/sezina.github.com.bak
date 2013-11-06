---
layout: post
title: "LeetCode --- Remove Duplicates from Sorted Array"
date: 2013-11-05 13:52
comments: true
categories: [LeetCode]
---

原题：

    Given a sorted array, remove the duplicates in place such that each element appear 
    only once and return the new length.

    Do not allocate extra space for another array, you must do this in place with constant memory.

    For example,
    
    Given input array A = [1,1,2],

    Your function should return length = 2, and A is now [1,2].
    
    
中文大意是：

    给定一个排好序的数组，移除所有重复的元素，使得所有元素在数组中只出现一次，不允许使用额外的数组。
    
    例如，
    输入为 A ＝ [1, 1, 2]
    输出是 2 (去重后数组的长度)
    

这道题在原数组上做操作，比如给出的例子操作后变为了`A = [1, 2, 2]`(赋值)或是`A = [1, 2, 1]`(交换)，只是返回长度为2.

```cpp
class Solution {
public:
    int removeDuplicates(int A[], int n) {
        // IMPORTANT: Please reset any member data you declared, as
        // the same Solution instance will be reused for each test case.
        if (n == 0) {
            return 0;
        }
        int new_len = 1;
        for (int i = 1; i < n; i++) {
            if (A[i] != A[new_len - 1]) {
                A[new_len++] = A[i];
            }
        }
        return new_len;
    }
};
```