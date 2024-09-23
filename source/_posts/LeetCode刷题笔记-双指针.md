---
title: LeetCode刷题笔记-双指针
date: 2024-09-01 13:57:42
cover_image:
cover_image_alt:
tags:
    - LeetCode
    - Algorithm
    - 数据结构与算法
    - 双指针
categories:
    - Code
---

# 背景
双指针法基本都是应用在数组，字符串与链表的题目上，通过两个指针减少算法复杂度。将部分题目和解法放在下面，作为回顾

# [字符串-09-回文数](https://leetcode.cn/problems/palindrome-number/description/)

{% notel default fa-info 题目描述 %}
给你一个整数 x ，如果 x 是一个回文整数，返回 true ；否则，返回 false 。
回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。
例如，121 是回文，而 123 不是。
{% endnotel %}

```python
#
# @lc app=leetcode.cn id=9 lang=python3
#
# [9] 回文数
#
"""
方法一：数字转换为字符串，双指针判断
11511/11511 cases passed (57 ms)
Your runtime beats 59.24 % of python3 submissions
Your memory usage beats 81.19 % of python3 submissions (16.3 MB)
"""
class Solution1:
    def isPalindrome(self, x: int) -> bool:
        if x < 0:
            return False
        else:
            s = str(x)
            first = 0
            last = len(s) - 1
            while first < last:
                if s[first] != s[last]:
                    return False
                first += 1
                last -= 1
            return True

"""
方法二：不通过转换字符串来判断
将数字的后半部分反转，和前半部分比较
每次对10取余数获得最后一位，然后倒序生成新的数
11511/11511 cases passed (53 ms)
Your runtime beats 76.37 % of python3 submissions
Your memory usage beats 8.7 % of python3 submissions (16.5 MB)

"""

# @lc code=start
class Solution:
    def isPalindrome(self, x: int) -> bool:
        if x < 0:
            return False
        elif x != 0 and x % 10 == 0:
            return False
        else:
            reverse_num = 0
            while x > reverse_num:
                reverse_num = reverse_num * 10 +  x % 10
                x = x // 10
            if x == reverse_num or x == reverse_num // 10:
                return True
            return False
# @lc code=end
```

# [链表-19-删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/discussion/)

{% notel default fa-info 题目描述 %}
给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。
示例 1：
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
示例 2：

输入：head = [1], n = 1
输出：[]
示例 3：

输入：head = [1,2], n = 1
输出：[1]
{% endnotel %}

```python
#
# @lc app=leetcode.cn id=19 lang=python3
#
# [19] 删除链表的倒数第 N 个结点
# 快慢指针+虚拟头节点
# 快指针先走n步，慢指针从头走可以找到倒数第n个节点
# 208/208 cases passed (37 ms)
# Your runtime beats 64.38 % of python3 submissions
# Your memory usage beats 49.93 % of python3 submissions (16.4 MB)

# @lc code=start
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        slow = None
        fast = head
        i = 0
        while fast is not None and i < n:
            fast = fast.next
            i += 1
        if fast is None:
            return head.next
        slow = head
        fast = fast.next
        while fast is not None:
            fast = fast.next
            slow = slow.next
        next = slow.next
        if next:
            slow.next = next.next
        else:
            slow = next
            return slow
        return head
            
        
# @lc code=end
```

# [链表-24-两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/discussion/)

{% notel default fa-info 题目描述 %}
给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

示例 1：
输入：head = [1,2,3,4]
输出：[2,1,4,3]
示例 2：

输入：head = []
输出：[]
示例 3：

输入：head = [1]
输出：[1]

{% endnotel %}


```python
#
# @lc app=leetcode.cn id=24 lang=python3
#
# [24] 两两交换链表中的节点
#

"""
双指针操作：
每次交换temp之后的两个节点
由原来的temp - node1 - node2
交换成 temp - node2 - node1

55/55 cases passed (37 ms)
Your runtime beats 63.54 % of python3 submissions
Your memory usage beats 59.51 % of python3 submissions (16.4 MB)
"""

# @lc code=start
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if not head or not head.next:
            return head
        dummy = ListNode(0, head)
        temp = dummy
        while temp.next and temp.next.next:
            first = temp.next
            second = temp.next.next
            temp.next = second
            first.next = second.next
            second.next = first
            temp = first
        return dummy.next
# @lc code=end
```

# [数组-26-删除有序数组中的重复项]()

{% notel default fa-info 题目描述 %}
给你一个 非严格递增排列 的数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。元素的 相对顺序 应该保持 一致 。然后返回 nums 中唯一元素的个数。

考虑 nums 的唯一元素的数量为 k ，你需要做以下事情确保你的题解可以被通过：

更改数组 nums ，使 nums 的前 k 个元素包含唯一元素，并按照它们最初在 nums 中出现的顺序排列。nums 的其余元素与 nums 的大小不重要。
返回 k 。
{% endnotel %}
```python
#
# @lc app=leetcode.cn id=26 lang=python3
#
# [26] 删除有序数组中的重复项
# 双指针解决，通过快指针在前面判断和原数字是否相等来进行修改原数组

# 362/362 cases passed (40 ms)
# Your runtime beats 65.57 % of python3 submissions
# Your memory usage beats 39.38 % of python3 submissions (17.6 MB)

# @lc code=start
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if len(nums) == 0:
            return 0
        slow, fast = 0, 0
        while fast < len(nums):
            if nums[slow] == nums[fast]:
                fast += 1
                continue
            slow += 1
            nums[slow] = nums[fast]
        return slow + 1
# @lc code=end

```