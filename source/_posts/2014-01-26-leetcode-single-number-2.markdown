---
layout: post
title: "LeetCode --- Single Number 2"
date: 2014-01-26 08:49
comments: true
categories: [LeetCode]
---

[OJ地址](http://oj.leetcode.com/problems/single-number-ii/)

英文原题：

    Given an array of integers, every element appears three times except for one. Find that single one.
    
    Note:
    Your algorithm should have a linear runtime complexity. 
    Could you implement it without using extra memory?


中文大意：
    
    给定一个由整数组成的数组，除了一个数之外其他的元素都出现了三次。请找出这个单独的数。
    
    注意：
    你的算法的时间复杂度要是线性的。你是否能够不用额外的存储空间解决这个问题？


有了上一道 [Single Number](http://sezina.github.io/blog/2013/11/05/leetcode-single-number/) 的练习，一看到这道题基本可以确定这道题也要用位运算来解答。

直观的解法就是使用一个`count[32]`的数组保存各位出现`1`的次数然后进行`mod 3`操作。除了一个单独的数外，其他数都出现了三次，那么，我们将所有数写成二进制位表示后，对每一位在所有的数中出现`1`的次数进行统计，会发现每位出现`1`的次数要么是3的倍数，要么就是模3余1，对统计结果进行`mod 3`，结果得`0`或`1`，然后将这些`0`和`1`对应回相应位置就是结果了。

如`1010`、`1100`、`1010`、`1010`四个数，统计结果是：第一位出现`1`的次数是0，第二位是3次，第三位是1次，第四位是4次，即`count[] = {0, 3, 1, 4}`，`mod 3`后得`count[] = {0, 0, 1, 1}`，所以结果是`1100`。

```cpp
class Solution {
public:
    int singleNumber(int A[], int n) {
        // IMPORTANT: Please reset any member data you declared, as
        // the same Solution instance will be reused for each test case.
        int count[32] = {0};
        int result = 0;
        for (int i = 0; i < 32; i++) {
            for (int j = 0; j < n; j++) {
                if ((A[j] >> i) & 1) {
                    count[i]++;
                }
            }
            result |= ((count[i] % 3) << i);
        }
        return result;
    }
};
```

另外一种解法是非常有技巧性的解法，运作原理类似于三进制加法，即遇三清零。位操作只能用二进制来做，所以这里就用二进制来模拟三进制加法。这里需要3条规则：

1. ones用于记录前一解法中`mod 3`得1的数位；
2. twos用于记录前一解法中`mod 3`得2的数位；
3. 当ones和twos相同的数位都为`1`时，说明该位上1出现了3次，清零。

通过这三条规则计算后，最后的ones即是结果。
最后的解法如下：

```cpp
class Solution {
public:
    int singleNumber(int A[], int n) {
        // IMPORTANT: Please reset any member data you declared, as
        // the same Solution instance will be reused for each test case.
        int ones = 0, twos = 0, threes = 0;
        for (int i = 0; i < n; i++) {
            twos |= A[i] & ones;
            ones ^= A[i];
            threes = ~(ones & twos);
            ones &= threes;
            twos &= threes;
        }
        return ones;
    }
};
```