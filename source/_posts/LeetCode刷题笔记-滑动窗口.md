---
title: LeetCode刷题笔记-滑动窗口
date: 2024-09-02 11:48:33
cover_image:
cover_image_alt:
thumbnail:
tags:
    - LeetCode
    - Algorithm
    - 数据结构与算法
    - 滑动窗口
categories:
    - Code
---

# 解题思路
{% notel default fa-info 思路 %}
滑动窗口问题主要可以解决子数组的问题，比如寻找某刻条件的最长或最短的子数组。
维护一个窗口，不断滑动，考虑好扩充窗口和缩小窗口的条件，一般向右边界用于扩充，左边界用于缩小。
这样时间复杂度是O(N)，因为可以通过一次遍历所有窗口的数据
{% endnotel %}

```python
left = 0
right = 0
while right < len(nums):
    # 增大窗口
    window.append(nums[right])
    right += 1

    while 需要减小窗口:
        window.remove[left]
        left += 1
```

# [03-字符串-无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/description/)

{% notel default fa-info 题目描述 %}
给定一个字符串 s ，请你找出其中不含有重复字符的 最长 子串 的长度。
示例 1:
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
{% endnotel %}

```python
#
# @lc app=leetcode.cn id=3 lang=python3
#
# [3] 无重复字符的最长子串
#
"""
滑动窗口思路：

987/987 cases passed (85 ms)
Your runtime beats 20.47 % of python3 submissions
Your memory usage beats 34.44 % of python3 submissions (16.5 MB)

"""
# @lc code=start
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        if len(s) <= 1:
            return len(s)
        left = 0
        right = 0
        res = 0
        window = {}
        while right < len(s):
            c = s[right]
            right += 1
            # 扩充window
            window[c] = window.get(c, 0) + 1
            # 当window中出现重复元素的时候缩小窗口
            while window.get(c, 0) > 1:
                d = s[left]
                left += 1
                window[d] -= 1
            # 记录最大的子串长度
            res = max(res, right - left)
        return res
# @lc code=end
```

# [76-字符串-最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/discussion/)

{% notel default fa-info 题目描述 %}
给你一个字符串 s 、一个字符串 t 。返回 s 中涵盖 t 所有字符的最小子串。如果 s 中不存在涵盖 t 所有字符的子串，则返回空字符串 "" 。
注意：
对于 t 中重复字符，我们寻找的子字符串中该字符数量必须不少于 t 中该字符数量。
如果 s 中存在这样的子串，我们保证它是唯一的答案。
示例 1：
输入：s = "ADOBECODEBANC", t = "ABC"
输出："BANC"
解释：最小覆盖子串 "BANC" 包含来自字符串 t 的 'A'、'B' 和 'C'。
{% endnotel %}
```python
#
# @lc app=leetcode.cn id=76 lang=python3
#
# [76] 最小覆盖子串
#
"""
滑动窗口思路：
扩充右边界：

缩小左边界：


268/268 cases passed (111 ms)
Your runtime beats 79.33 % of python3 submissions
Your memory usage beats 74.86 % of python3 submissions (16.7 MB)
"""

# @lc code=start
class Solution:
    def minWindow(self, s: str, t: str) -> str:
        window = {}
        need = {}
        for i in t:
            need[i] = need.get(i, 0) + 1
        left = 0
        right = 0
        valid = 0
        start = 0
        length = float('inf')
        # 扩充右边界
        while right < len(s):
            c = s[right]
            right += 1
            # 当前字符在需要的字符中
            if need.get(c):
                # 扩充窗口
                window[c] = window.get(c, 0) + 1
                # 当需要的字符齐全后，将valid标识+1
                if window[c] == need[c]:
                    valid += 1
            # 当valid标识和need的字符相等，可以移动左边界        
            while valid == len(need):
                # 在移动左边界前找到最短的子串长度和start标识
                if right - left < length:
                    start = left
                    length = right - left
                # 需要移除出窗口的字符
                r = s[left]
                left += 1
                # 如果需要移除的字符在need中
                if need.get(r):
                    # 先判断是否是否已经把need的数全部移除
                    if window[r] == need[r]:
                        valid -= 1
                    # 移除字符
                    window[r] -= 1
        return "" if length == float('inf') else s[start: start + length]
# @lc code=end
```

# [209-数组-长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/description/)
{% notel default fa-info 题目描述 %}
给定一个含有 n 个正整数的数组和一个正整数 target 。

找出该数组中满足其总和大于等于 target 的长度最小的 子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。
示例 1：
输入：target = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。
{% endnotel %}
```python
#
# @lc app=leetcode.cn id=209 lang=python3
#
# [209] 长度最小的子数组
#
"""
滑动窗口

21/21 cases passed (50 ms)
Your runtime beats 80.24 % of python3 submissions
Your memory usage beats 13.98 % of python3 submissions (26.9 MB)

"""

from typing import List
# @lc code=start
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        left = 0
        right = 0
        sums = 0
        res = float('inf')
        while right < len(nums):
            # 累加窗口的和
            sums += nums[right]
            # 当区间和大于等于target的时候缩小窗口
            while sums >= target:
                res = min(res, right - left + 1)
                sums -= nums[left]
                left += 1
            # 扩充右边界
            right += 1
        # 如果没有大于等于target的情况，返回0
        return res if res != float('inf') else 0


# @lc code=end

if __name__ == '__main__':
    target = 11
    nums = [1,1,1,1,1,1,1,1]
    so = Solution()
    print(so.minSubArrayLen(target, nums))
```