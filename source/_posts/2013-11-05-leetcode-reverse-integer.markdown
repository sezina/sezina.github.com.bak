---
layout: post
title: "LeetCode --- Reverse Integer"
date: 2013-11-05 09:11
comments: true
categories: [LeetCode]
---

原题：


    Reverse digits of an integer.

    Example1: x = 123, return 321
    Example2: x = -123, return -321


    Have you thought about this?
    Here are some good questions to ask before coding.
    Bonus points for you if you have already thought through this!

    If the integer's last digit is 0, what should the output be? ie, cases such as 10, 100.

    Did you notice that the reversed integer might overflow? Assume the input 
    is a 32-bit integer, then the reverse of 1000000003 overflows. How should you handle such cases?

    Throw an exception? Good, but what if throwing an exception is not an option? 
    You would then have to re-design the function (ie, add an extra parameter).
    
    
中文大意是：

    反转给定的整数。
    
    如果考虑了以下问题有加分。
    当给定的整数末尾为0的情况。
    给定的整数在32位int范围内，但反转后会溢出的情况，如1000000003反转后就会溢出。
    
    抛出异常来处理？好，但如果抛出异常不是我们希望的做法呢？你将要重新设计函数。
    

说实在的我没搞懂这道题要考察什么，如果说是那两种意外情况，那只需声明一个long long类型的变量就搞定了。


```cpp
class Solution {
public:
    int reverse(int x) {
        // IMPORTANT: Please reset any member data you declared, as
        // the same Solution instance will be reused for each test case.
        long long result = 0;
        while (x) {
            result = result * 10 + (x % 10);
            x /= 10;
        }
        
        return result;
    }
};
```