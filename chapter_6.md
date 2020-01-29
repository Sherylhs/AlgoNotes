# Table of Contents
- [隐式图](#隐式图)
- [Copy](#copy)


# 隐式图

## 定义
一个问题如果没有明确说明什么是点，什么是边，但是又需要进行搜索的话，那就是一个隐式图搜索问题了。所以隐式图搜索的问题，首先要分析清楚什么是点什么是边。

## DFS
找到图中的所有满足条件的路径，基本可以确定是DFS。除了二叉树以外的90% DFS的题，要么是排列，要么是组合。

路径 = 方案 = 图中节点的排列组合

在找全子集的问题中：
- DFS是找路径，而BFS是找点
- DFS最省空间，而BFS则更耗费空间
- DFS (2<sup>n</sup>)和 BFS (2<sup>n+1</sup>)的空间复杂度是一样的

## 组合问题
- 问题模型: 求出所有满足条件的 __组合__
- 判断条件: 组合中的元素是顺序无关的
- 时间复杂度: 与 2<sup>n</sup> 相关

## 排列问题
- 问题模型: 求出所有满足条件的 __排列__
- 判断条件: 组合中的元素是顺序相关的
- 时间复杂度: 与 n! 相关

# Copy
下列Java代码调用了 ArrayList 的一个构造函数（Constructor），这个构造函数可以接受另外一个 ArrayList 作为其初始化的状态：
```java
results.add(new ArrayList<Integer>(subset));
```

而在Python中则新建一个list，list的构造接受一个Iterable对象作为参数，并将该对象内的元素按顺序添加到新建的list中：
```py
results.append(list(subset))
```

这种方式，即为深度拷贝(Deep Copy)，又叫做硬拷贝(Hard Copy)或者克隆(Clone)。与之对应的就有软拷贝(Soft Copy)，又名引用拷贝(Reference Copy)。

## Soft Copy
```java
List<Integer> subset = new ArrayList<>();
subset.add(1);  // 此时 subset 是 [1]

List<List<Integer>> results = new ArrayList<>();
results.add(subset);  // 此时 results 是 [[1]]

subset.add(2);  // 此时 subset 是 [1,2]
results.add(subset);  // 此时你以为 results 是 [[1], [1,2]] 而事实上他是[[1,2], [1,2]]

subset.add(3);  // 此时 results 里是 [[1,2,3], [1,2,3]]
```

```py
subset = []
subset.append(1) # 此时subset是[1]

results = []
results.append(subset)  # 此时results是[[1]]

subset.append(2)  # 此时subset是[1, 2]
results.append(subset)  # 此时你以为results是[[1], [1,2]]而事实上他是[[1,2], [1,2]]

subset.append(3)  # 此时results里是[[1,2,3], [1,2,3]]
```

由于每一次 results.add 都是加入了相同的变量 subset，因此如果 subset 有变化，那么 result 里的记录就会同步的发生变化。原因是 results.add(subset) 加入的是 subset 的 reference，也就是 subset 在内存中的地址。那么事实上，当 results 里有两个 subset 的时候，相当于存储的是两个内存地址，而这两个内存地址又是一样的，才会导致如果这个内存地址里存的东西发生了变化，results 看起来就每个元素都发生了变化。

## Deep Copy
当修改操作是`x = ...`时才是对参数x的修改，`x.call_method()`并不是对参数 x 本身的修改，而是对基于参数x进行的操作。
```java
public void func(List<Integer> subset) {
    subset = new ArrayList<Integer>();
    subset.add(1);
}
public void main() {
    List<Integer> subset = new ArrayList<>();
    // 此时 subset 是 []
    func(subset);
    // 此时 subset 还是 []
}
```

```py
def func(subset):
    subset = list(subset)
    subset.append(1)

def main():
    subset = []
    # 此时 subset 是 []
    func(subset)
    # 此时 subset 还是 []
}
```

