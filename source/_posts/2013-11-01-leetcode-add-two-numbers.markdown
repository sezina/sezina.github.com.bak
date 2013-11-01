---
layout: post
title: "LeetCode --- Add Two Numbers"
date: 2013-11-01 16:46
comments: true
categories: [LeetCode]
---

又来了一道a＋b的题目。

英文原题：

      You are given two linked lists representing two 
    non－negative numbers. The digits are stored in reverse
    order and each of their nodes contain a single digit.
    Add the two numbers and return it as a linked list.

    **Input**: (2 -> 4 -> 3) + (5 -> 6 -> 4)
    **Output**: 7 -> 0 -> 8

大意是：

    给定两个用链表表示的非负数，这两个数为逆序存储（即个位->十位->百位这样存储），
    数字的每一位用一个节点表示，求和后将和以同样的方式组织并返回。
    

这道题是考察基本的指针链表，非常简单。

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *addTwoNumbers(ListNode *l1, ListNode *l2) {
        // IMPORTANT: Please reset any member data you declared, as
        // the same Solution instance will be reused for each test case.
        int carry = 0;
        int temp_sum = 0;
        ListNode *result_l = NULL, *last_one;
        while (l1 != NULL && l2 != NULL) {
            temp_sum = l1->val + l2->val + carry;
            if (temp_sum > 9) {
                carry = 1;
                temp_sum -= 10;
            } else {
                carry = 0;
            }
            if (result_l == NULL) {
                result_l = last_one = new ListNode(temp_sum);
            } else {
                last_one->next = new ListNode(temp_sum);
                last_one = last_one->next;
            }
            l1 = l1->next;
            l2 = l2->next;
        }
        
        ListNode *copy_l;
        if (l1 != NULL) {
            copy_l = l1;
        } else {
            copy_l = l2;
        }
        
        while (copy_l != NULL) {
            temp_sum = copy_l->val + carry;;
            if (temp_sum > 9) {
                carry = 1;
                temp_sum -= 10;
            } else {
                carry = 0;
            }
            last_one->next = new ListNode(temp_sum);
            last_one = last_one->next;
            copy_l = copy_l->next;
        }
        
        if (carry) {
            last_one->next = new ListNode(carry);
        }
        return result_l;
    }
};
```