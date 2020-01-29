# Table of Contents
- [Memoization](#memoization)


# Memoization
将函数的计算结果保存下来，下次通过同样的参数访问时，直接返回保存下来的结果。

问：
1. 对这个函数有什么限制条件没有？
2. 和系统设计中的什么很像？

记忆化搜索通常能够将指数级别的时间复杂度降低到多项式级别。

记忆化搜索 = 动态规划(DP):
- 记忆化搜索是动态规划的一种实现方式
- 记忆化搜索是用搜索的方式实现了动态规划


## Dynamic Programming
### Dynamic Programming vs Divide-and-Conquer
动态规划的题会有交集，可以避免重复运算；而分治的题一般不会有交集

### 适用DP的场景
- 求最值 (max/min)
- 求方案总数 (sum)
- 求可行性 (or)

### 不适用DP的场景
- 求所有的具体方案 (最坏情况下用了DP也没有优化效果)
- 输入数据是无序的 (除了背包类)
- 暴力算法时间复杂度已经是多项式级别 (2<sup>n</sup> → n<sup>2</sup>)

### 高频动态规划类别
- 坐标型
    - 数字三角形 Triangle
- 背包型
    - 背包问题 Backpack
- 区间型
    - 石子归并 Stone Game
- 序列型
    - 单词拆分 Word Break 系列
    - 解码方式 Decode ways
- 双序列型
    - 最长公共子串 Longest Common Subsequence
    - 最短编辑距离 Edit Distance
    - 通配符匹配 Wildcard Matching
