# Table of Contents
- [Coding Style](#coding-style)
- [Greedy](#greedy)
- [String](#string)
- [PrefixSum](#prefixsum)
- [Rabin Karp](#rabin-karp)
- [Manacher](#manacher)


# Coding style
- https://zh-google-styleguide.readthedocs.io/en/latest/#
- https://google.github.io/styleguide/javaguide.html


# Greedy
必须“背诵”的[贪心算法题](https://www.jiuzhang.com/qa/2099/)：
- http://www.lintcode.com/problem/majority-number/
- http://www.lintcode.com/problem/create-maximum-number/
- http://www.lintcode.com/problem/jump-game-ii/
- http://www.lintcode.com/problem/jump-game/
- http://www.lintcode.com/problem/gas-station/
- http://www.lintcode.com/problem/delete-digits/
- http://www.lintcode.com/problem/task-scheduler/


# String
- Java: http://www.runoob.com/java/java-string.html
- python: http://www.runoob.com/python/python-strings.html


# PrefixSum
令PrefixSum[i] = A[0] + A[1] + ... + A[i - 1], PrefixSum[0] = 0, 可知构造 PrefixSum 耗费 O(n) 时间和 O(n) 空间。

如需计算子数组从下标i到下标j之间的所有数之和，则有：
`Sum(i~j) = PrefixSum[j + 1] - PrefixSum[i]`。


# Rabin Karp
The key is to convert substring to a hash value and maintaing the value by adding new character in while removing the oldest character out. 

The hardest part is to calculate hash value while maintaining the number not to be out of bound, so it's needed to apply the Distributive Law of mod. For instance, assume MAGIC_NUMBER `m=9`, HASH_SIZE `n=10`, `a=1`, `b=3`, `c=6`:

```
(a * m^2 + b * m + c) % n
= ((am + b) * m + c) % n
= ((((am % n) + b) % n) * m + c) % n
```
-> 
```
(1 * 81 + 3 * 9 + 6) % 10 = 114 % 10 = 4
((((1 * 9 % 10) + 3) % 10) * 9 + 6) % 10
= (((9 + 3) % 10) * 9 + 6) % 10
= (2 * 9 + 6) % 10 = 4
```

In a nutshell, keep in mind that: 
`(am + b) % n = ((am % n) + b) % n`

Refer to the [related question](https://www.lintcode.com/problem/strstr-ii/).


# Manacher
https://segmentfault.com/a/1190000003914228

