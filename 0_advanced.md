
- [Union Find](#union-find)
- [Heap](#heap)
- [Monotonous Stack](#monotonous-stack)
- [Deque](#deque)
- [Binary Search](#binary-search)
- [Sweep-Line](#sweep-line)
- [Summary](#summary)
- [Reference](#reference)


# Union Find
Problem List:
- Number of Islands II
- Graph Valid Tree

Reference:
- https://en.wikipedia.org/wiki/Iterated_logarithm
- https://en.wikipedia.org/wiki/Proof_of_O(log*n)_time_complexity_of_union%E2%80%93find


# Heap
- Trapping Rain Water
- Trapping Rain Water II
- Data Stream Median
- Sliding Window Median
- Min Stack
- Expression Expand


# Monotonous Stack
- Largest Rectangle in Histogram
- Maximal Rectangle
- Maximal Tree
- Find The Duplicate Number


# Deque
Deque = Queue + Stack
- Sliding Window Maximum


# Binary Search 
一道题区分五类面试者:
1. 只会写 for 循环: Find Peak Element 只会 O(n)
2. 会优化: Find Peak Element 会 O(log(n))
3. 会优化不会举一反三: Find Peak Element II 只会 O(n<sup>2</sup>)
4. 会优化会举一反三: Find Peak Element II 会 O(nlog(n))
5. 会举一反四: Find Peak Element II 会 O(n)

解题方法: 通过猜值判断是否满足题意，以搜索可能解
1. 找到可行解范围
2. 猜答案
3. 检验条件
4. 调整搜索范围

模板:
```py
# 找到可行解范围
start = 1
end = max(nums)

while start + 1 < end:
    # 猜答案
    middle = start + (end - start) // 2
    # 检验条件
    if check(middle):
        # 调整搜索范围
        start = middle
    else：
        end = middle
```

- Find Peak Element II
- Maximum Average Subarray
- Sqrt(x)
- Wood Cut


# Sweep-Line
扫描问题的思路：
1. 事件往往是以区间的形式存在
2. 区间两端代表事件的开始和结束
3. 需要排序

- Number of Airplanes in the sky
- Building Outline


# Summary
- Heap: 求集合的最大值
- Stack: 
    1. 单调栈的运用, 找左边或右边第一个比它大的元素
    2. 递归转非递归
- Deque: 两端都会有 push 和 pop
- Windows problem:
    1. 加一个数
    2. 删一个数

数据结构查询表:

| 数据结构 | C++ | Java | Python | 本质结构 |
| --- | --- | --- | --- | --- |
| Heap | priority_queue | PriorityQueue | Heapq | 二叉堆 |
| Hash | Unordered_map | HashMap | Dict | 哈希表 |
| TreeMap | Map | TreeMap | Ordereddict | 平衡二叉树 |
| TreeSet | Set | TreeSet | Ordereddict |平衡二叉树 |

注: Python 的 Ordereddict 并不是 TreepMap 和 TreeSet, 只是可以大概地代替。

# Reference
- 纸上谈兵 - 算法与数据结构: https://www.cnblogs.com/vamei/archive/2013/03/22/2974052.html
- 视频: https://www.bilibili.com/video/av51031361/?p=19



