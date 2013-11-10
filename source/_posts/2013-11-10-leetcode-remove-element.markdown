---
layout: post
title: "LeetCode --- Remove Element"
date: 2013-11-10 08:45
comments: true
categories: [LeetCode]
---

原题：

    Given an array and a value, remove all instances of that value in place and return the new length.

    The order of elements can be changed. It doesn't matter what you leave beyond the new length.
    

中文大意：

    给定一个数组和一个数，移除该数组中所有等于这个数的元素并返回删除元素后数组的长度。
    
    数组中元素的顺序可以改变，超出新长度的部分数组的值为什么都可以。
    

需要注意的是，数组一直是那个数组，返回的是新数组的长度，也就是说下标小于新长度的数组元素必须是保存了数组删除了特定数后剩余的所有元素。

```cpp
class Solution {
public:
    int removeElement(int A[], int n, int elem) {
        // IMPORTANT: Please reset any member data you declared, as
        // the same Solution instance will be reused for each test case.
        int del_count = 0;
        for (int i = 0; i < n; ++i)
        {
        	if (A[i] == elem)
        	{
        		del_count++;
        	} else {
        		A[i - del_count] = A[i];
        	}
        }
        return n - del_count;
    }
};
```