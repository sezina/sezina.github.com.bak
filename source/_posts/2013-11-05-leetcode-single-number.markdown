---
layout: post
title: "LeetCode --- Single Number"
date: 2013-11-05 09:25
comments: true
categories: [LeetCode]
---

英文原题：

    Given an array of integers, every element appears twice except for one. Find that single one.

    Note:
    Your algorithm should have a linear runtime complexity. 
    Could you implement it without using extra memory?


中文大意：

    给定一个由整数组成的数组，除了一个数之外其他的元素都出现了两次。请找出这个单独的数。
    
    注意：
    你的算法的时间复杂度要是线性的。你是否能够不用额外的存储空间解决这个问题？
    
第一次看到这题，看完第一句的时候估计不少人瞬间就有了解法然后，然后看完“注意”后，傻眼了。
这道题用到了数在计算机中的表示及位运算。一个数与其自身做异或操作`^`等于0！利用这个特性，将数组中所有元素进行异或操作，最后的结果就是那个单独的数字。

```cpp
class Solution {
public:
    int singleNumber(int A[], int n) {
        // IMPORTANT: Please reset any member data you declared, as
        // the same Solution instance will be reused for each test case.
        for (int i = 1; i < n; i++) {
            A[0] ^= A[i];
        }
        return A[0];
    }
};
```