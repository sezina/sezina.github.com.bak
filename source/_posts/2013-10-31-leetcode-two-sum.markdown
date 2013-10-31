---
layout: post
title: "LeetCode --- Two Sum"
date: 2013-10-31 13:04
comments: true
categories: [LeetCode]
---

昨日惊闻LeetCode这东西，听说上面的OJ都是些面试题，又据说是去北美找工作的人都会上去刷刷题，于是带着作为技术人对硅谷无比向往的心情注册了个号开刷，准备在接下来的这一段上算法课时间里把它刷完（一个美好的愿景）。

首先，上一道和所有OJ的第一道a＋b一样的题：
         
      Given an array of integers, find two numbers such
    that they add up to a specific target number.
      The function twoSum should return indices of the
    two numbers such that they add up to the target,
    where index1 must be less than index2. Please note
    that your returned answers (both index1 and index2)
    are not zero-based.
      You may assume that each input would have exactly
    one solution.
      
    **Input**: numbers={2, 7, 11, 15}, target=9
    **Output**: index1=1, index2=2

中文意思就是：
    
    给定一个数列，找到两个数，它们相加刚好等于给定的target的值。
    函数twoSum 应该返回这两个数的下标indx1和index2，且要求index1 < index2，注意，这里的下标不是从0开始的。
    
    **示例输入**：numbers={2, 7, 11, 15}, target=9
    **示例输出**：index1=1, index2=2
    

看完题意简单吧，就是a＋b的问题。我直接写了两个for循环对数组进行循环判断。下面是我的C++实现。

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int> &numbers, int target) {
        // IMPORTANT: Please reset any member data you declared, as
        // the same Solution instance will be reused for each test case.
        vector<int> result;
        for (int i = 0; i < numbers.size(); i++) {
            for (int j = i + 1; numbers[i] <= target && j < numbers.size(); j++) {
                if (numbers[j] <= target && numbers[i] + numbers[j] == target) {
                    result.push_back(i + 1);
                    result.push_back(j + 1);
                    break;
                }
            }
        }
        return result;
    }
};
```