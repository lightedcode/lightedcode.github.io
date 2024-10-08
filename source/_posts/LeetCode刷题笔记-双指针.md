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

# [数组-27-移除元素](https://leetcode.cn/problems/remove-element/discussion/)

{% notel default fa-info 题目描述 %}
给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素。元素的顺序可能发生改变。然后返回 nums 中与 val 不同的元素的数量。

假设 nums 中不等于 val 的元素数量为 k，要通过此题，您需要执行以下操作：

更改 nums 数组，使 nums 的前 k 个元素包含不等于 val 的元素。nums 的其余元素和 nums 的大小并不重要。
返回 k。 
{% endnotel %}

```python
#
# @lc app=leetcode.cn id=27 lang=python3
#
# [27] 移除元素
# 使用双指针进行操作
# 快指针先走，如果快指针不等于指定值，则赋值给slow指针
# 115/115 cases passed (39 ms)
# Your runtime beats 42.57 % of python3 submissions
# Your memory usage beats 48.4 % of python3 submissions (16.4 MB)

# @lc code=start
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        slow = 0
        for fast in range(len(nums)):
            if nums[fast] == val:
                continue
            nums[slow] = nums[fast]
            slow += 1
        return slow
# @lc code=end
```
# [数组-80-删除有序数组中的重复项 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-array-ii/description/)

{% notel default fa-info 题目描述 %}
给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使得出现次数超过两次的元素只出现两次 ，返回删除后数组的新长度。
不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

输入：nums = [1,1,1,2,2,3]
输出：5, nums = [1,1,2,2,3]
解释：函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3。 不需要考虑数组中超出新长度后面的元素。
{% endnotel %}

```python
#
# @lc app=leetcode.cn id=80 lang=python3
#
# [80] 删除有序数组中的重复项 II
#
"""
双指针
从第三个元素开始检查，遇到和slow-2下标不同的数时，替换slow并将slow+1
167/167 cases passed (31 ms)
Your runtime beats 94.78 % of python3 submissions
Your memory usage beats 94.1 % of python3 submissions (16.3 MB)
"""

# @lc code=start
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if len(nums) <= 2:
            return len(nums)

        slow = 2  # 从第三个元素开始检查
        for fast in range(2, len(nums)):
            if nums[fast] != nums[slow - 2]:
                nums[slow] = nums[fast]
                slow += 1
        return slow
# @lc code=end
```


# [链表-141-环形链表](https://leetcode.cn/problems/linked-list-cycle/discussion/)

{% notel default fa-info 题目描述 %}
给你一个链表的头节点 head ，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。注意：pos 不作为参数进行传递 。仅仅是为了标识链表的实际情况。
{% endnotel %}

```python
#
# @lc app=leetcode.cn id=141 lang=python3
#
# [141] 环形链表
# 快慢指针
# 29/29 cases passed (46 ms)
# Your runtime beats 69.41 % of python3 submissions
# Your memory usage beats 79.32 % of python3 submissions (18.8 MB)

# @lc code=start
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        slow = head
        fast = head
        while fast is not None and fast.next is not None:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True
        return False
        
# @lc code=end
```
# [链表-142-环形链表II](https://leetcode.cn/problems/linked-list-cycle-ii/description/)
{% notel default fa-info 题目描述 %}
给定一个链表的头节点  head ，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

不允许修改 链表。
{% endnotel %}
```python
#
# @lc app=leetcode.cn id=142 lang=python3
#
# [142] 环形链表 II
# 使用快慢指针：慢指针走一次，快指针走两次，如果有环，那么两个指针必将相遇。
# 指针相遇之后把慢指针调整到头节点，然后一起走，第一次相遇的时候就是第一次入环的节点。
# 17/17 cases passed (48 ms)
# Your runtime beats 43.09 % of python3 submissions
# Your memory usage beats 80.5 % of python3 submissions (18.8 MB)
# @lc code=start
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = head
        fast = head
        while fast is not None and fast.next is not None:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                break
        if not fast or not fast.next:
            return None
        slow = head
        while slow != fast:
            slow = slow.next
            fast = fast.next
        return slow
        
# @lc code=end
```

# [链表-234-回文链表](https://leetcode.cn/problems/palindrome-linked-list/description/)

{% notel default fa-info 题目描述 %}
给你一个单链表的头节点 head ，请你判断该链表是否为回文链表。如果是，返回 true ；否则，返回 false 。

输入：head = [1,2,2,1]
输出：true

输入：head = [1,2]
输出：false
{% endnotel %}

```python
#
# @lc app=leetcode.cn id=234 lang=python3
#
# [234] 回文链表
#

# @lc code=start
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next


"""
方法一：
    将原始链表放到栈里，每次和弹出的数据对比是否一致
    93/93 cases passed (325 ms)
    Your runtime beats 43.74 % of python3 submissions
    Your memory usage beats 72.01 % of python3 submissions (37.7 MB)
"""


class Solution:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        if head is None:
            return False
        stack = []
        cur = head
        n = 0
        while cur is not None:
            stack.append(cur.val)
            cur = cur.next
            n += 1
        cur = head
        mid = 0
        if n == 1:
            return True
        else:
            mid = n // 2
        while mid != 0:
            if cur.val == stack.pop():
                cur = cur.next
                mid -= 1
            else:
                return False
        return True

"""
方法二：
    1. 快慢指针把右侧的链表放到栈里去
    2. 依次和左边的链表对比是否一致
93/93 cases passed (315 ms)
Your runtime beats 49.3 % of python3 submissions
Your memory usage beats 67.72 % of python3 submissions (37.8 MB)
"""


class Solution2:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        if head is None:
            return False
        slow = head
        fast = head.next
        stack = []
        if fast is None:
            return True
        else:
            while fast.next is not None and fast.next.next is not None:
                fast = fast.next.next
                slow = slow.next                     
            # 找到中点后进栈
            slow = slow.next
            while slow is not None:
                stack.append(slow.val)
                slow = slow.next
            cur = head
            while len(stack) != 0:
                if cur.val != stack.pop():
                    return False
                cur = cur.next
            return True
        
"""
方法三：
    1. 快慢指针，当找到中点的时候，下一个节点指向中点，将链表反转
    2. 记录两个头节点，往回遍历，判断是否一致
    3. 往回遍历的过程中，将链表反转回来
    93/93 cases passed (281 ms)
Your runtime beats 82.66 % of python3 submissions
Your memory usage beats 92.77 % of python3 submissions (33.1 MB)
"""


class Solution3:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        if head is None:
            return False
        slow = head
        fast = head.next
        if fast is None:
            return True
        else:
            while fast.next is not None and fast.next.next is not None:
                fast = fast.next.next
                slow = slow.next                          
            # 找到中点后反转链表
            fast = slow.next
            slow.next = None
            flag = None
            while fast is not None:
                flag = fast.next
                fast.next = slow
                slow = fast
                fast = flag
            # 最后一个节点
            flag = slow
            fast = head
            result = True
            while fast is not None and slow is not None:
                if fast.val != slow.val:
                    result = False
                    break
                fast = fast.next
                slow = slow.next
            # 将链表反转回来
            slow = flag.next
            flag.next = None
            while slow is not None:
                fast = slow.next
                slow.next = flag
                flag = slow
                slow = fast
            return result


# @lc code=end
```

# [字符串-344-反转字符串](https://leetcode.cn/problems/reverse-string/description/)

{% notel default fa-info 题目描述 %}
编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 s 的形式给出。
不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。
示例 1：

输入：s = ["h","e","l","l","o"]
输出：["o","l","l","e","h"]
示例 2：

输入：s = ["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]
{% endnotel %}
```

#
# @lc app=leetcode.cn id=344 lang=python3
#
# [344] 反转字符串
# 双指针方法，一个从前往后一个从后往前

# 477/477 cases passed (45 ms)
# Your runtime beats 38.94 % of python3 submissions
# Your memory usage beats 87.03 % of python3 submissions (21.5 MB)

# @lc code=start
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left = 0
        right = len(s) - 1
        while left < right:
            tmp = s[left]
            s[left] = s[right]
            s[right] = tmp
            left += 1
            right -= 1
# @lc code=end
```
# [链表-876-链表的中间结点](https://leetcode.cn/problems/middle-of-the-linked-list/description/)

{% notel default fa-info 题目描述 %}
给你单链表的头结点 head ，请你找出并返回链表的中间结点。

如果有两个中间结点，则返回第二个中间结点。
{% endnotel %}

```
#
# @lc app=leetcode.cn id=876 lang=python3
#
# [876] 链表的中间结点
# 快慢指针+虚拟节点
# 慢指针走一步，快指针走两步
# 36/36 cases passed (33 ms)
# Your runtime beats 80.25 % of python3 submissions
# Your memory usage beats 60.82 % of python3 submissions (16.4 MB)

# @lc code=start
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = head
        fast = head
        while fast is not None and fast.next is not None:
            slow = slow.next
            fast = fast.next.next
        return slow
# @lc code=end

```

# [数组-977-有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/description/)
{% notel default fa-info 题目描述 %}
给你一个按 非递减顺序 排序的整数数组 nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。
示例 1：
输入：nums = [-4,-1,0,3,10]
输出：[0,1,9,16,100]
解释：平方后，数组变为 [16,1,0,9,100]
排序后，数组变为 [0,1,9,16,100]
示例 2：

输入：nums = [-7,-3,2,3,11]
输出：[4,9,9,49,121]

{% endnotel %}

```
#
# @lc app=leetcode.cn id=977 lang=python3
#
# [977] 有序数组的平方
"""
方法一：用栈实现
137/137 cases passed (52 ms)
Your runtime beats 56.15 % of python3 submissions
Your memory usage beats 31.12 % of python3 submissions (18.3 MB)
"""
class Solution1:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        stack = []
        res = []
        for i in nums:
            squre = i * i
            while stack and stack[-1] < squre:
                res.append(stack.pop())
            stack.append(squre)
        while stack:
            res.append(stack.pop())
        return res
"""
方法二：双指针，一个在头一个在尾，比较绝对值，然后移动其中一个下标
137/137 cases passed (43 ms)
Your runtime beats 91.07 % of python3 submissions
Your memory usage beats 67.76 % of python3 submissions (18.1 MB)
"""
# @lc code=start
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        res = [0] * len(nums)
        left = 0
        right = len(nums) - 1
        index = len(nums) - 1
        while left <= right:
            num_left = nums[left]
            num_right = nums[right]
            if abs(num_left) < abs(num_right):
                res[index] = num_right * num_right
                right -= 1
            else:
                res[index] = num_left * num_left
                left += 1
            index -= 1
        return res
        
# @lc code=end

```