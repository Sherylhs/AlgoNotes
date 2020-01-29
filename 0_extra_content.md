# Table of Contents
- [自定义排序](#自定义排序)
    - [Java](#java)
    - [Python](#[python])
- [外排序](#外排序)
- [彩虹排序](#彩虹排序)


# 自定义排序
## Java
Java实现自定义排序，主要有两种方法：实现`Comparable`接口或定义比较类`Comparator`。

### Comparable
以Interval区间为例，在定义该类时，让其实现`Comparable`，并重写其中的`compareTo`方法，使得Interval类可以进行大小比较，这样也可实现自定义的排序：
```java
class Interval implements Comparable<Interval> {
    int left, right;
    Interval(int left, int right) {
        this.left = left;
        this.right = right;
    }

    @Override
    public int compareTo(Interval o) {
        return this.left - o.left;
    }
}

public class Main {
    public static void main(String[] args) {
        List<Interval> A = new ArrayList<>();
        A.add(new Interval(1, 7));
        A.add(new Interval(5, 6));
        A.add(new Interval(3, 4));

        sort(A);
    }
}
```

### Comparator
定义一个新的比较类，使其继承自`Comparator`，并完善其中的`compare()`方法。在调用排序时，使用该比较类进行比较。仍以Interval为例：
```java
class Interval {
    int left, right;
    Interval(int left, int right) {
        this.left = left;
        this.right = right;
    }
}

class IntervalCmp implements Comparator<Interval> {
    @Override
    public int compare(Interval o1, Interval o2) {
        return o1.left - o2.left;
    }
}

public class Main {
    public static void main(String[] args) {
        List<Interval> A = new ArrayList<>();
        A.add(new Interval(1, 7));
        A.add(new Interval(5, 6));
        A.add(new Interval(3, 4));

        A.sort(new IntervalCmp());
    }
}
```

## Python
类似地，Python也可以通过两种方法自定义排序：实现`__lt__`方法或定义`key`函数。

### `__lt__` method
以Interval区间为例，在定义该类时，重写其中的`__lt__`方法，使得Interval类可以进行大小比较，这样也可实现自定义的排序：
```py
class Interval:
    def __init__(self, left, right):
        self.left = left
        self.right = right

    # overwrite __lt__
    def __lt__(self, other):
        # compare Interval's left attribute directly when performing the comparison
        return self.left < other.left

if __name__ == "__main__":
    A = []
    A.append(Interval(1, 7))
    A.append(Interval(5, 6))
    A.append(Interval(3, 4))

    A.sort()
```

### `key` function
可以给sort方法传入一个key函数，表示按照什么标准来对元素进行排序，仍以Interval为例:
```py
class Interval:
    def __init__(self, left, right):
        self.left = left
        self.right = right

def IntervalKey(interval):
    return interval.left 

A = []
A.append(Interval(1, 7))
A.append(Interval(5, 6))
A.append(Interval(3, 4))
A.sort(key=IntervalKey)
```

另外，亦可通过返回一个tuple作为`key`以实现多关键字排序：
```py
class Interval:
    def __init__(self, left, right):
        self.left, self.right = left, right

    # 打印函数，用于直接print一个Interval对象
    def __repr__(self):
        return "Interval(%d, %d)" % (self.left, self.right)


data = [(3, 2), (3, 1), (2, 7), (1, 5), (2, 6), (1, 7)]
intervals = [Interval(left, right) for left, right in data]

print(sorted(intervals, key=lambda i: (i.left, i.right)))
# [Interval(1, 5), Interval(1, 7), Interval(2, 6), Interval(2, 7), Interval(3, 1), Interval(3, 2)]

print(sorted(intervals, key=lambda i: (-i.left, i.right)))
# [Interval(3, 1), Interval(3, 2), Interval(2, 6), Interval(2, 7), Interval(1, 5), Interval(1, 7)]

print(sorted(intervals, key=lambda i: (i.right, i.left)))
# [Interval(3, 1), Interval(3, 2), Interval(1, 5), Interval(2, 6), Interval(1, 7), Interval(2, 7)]

print(sorted(intervals, key=lambda i: (-i.right, i.left)))
# [Interval(1, 7), Interval(2, 7), Interval(2, 6), Interval(1, 5), Interval(3, 2), Interval(3, 1)]
```


# 外排序
外排序算法 (External Sorting)，是指在内存不够的情况下，如何对存储在一个或者多个大文件中的数据进行排序的算法。外排序算法通常是解决一些大数据处理问题的第一个步骤，或者是面试官所会考察的算法基本功。外排序算法是海量数据处理算法中十分重要的一块。在学习这类大数据算法时，经常要考虑到内存、缓存、准确度等因素。

## 基本步骤
外排序算法分为两个基本步骤：
1. 将大文件切分为若干个个小文件，并分别使用内存排好序
2. 使用K路归并算法（k-way merge）将若干个排好序的小文件合并到一个大文件中

### 第一步： 文件拆分
根据内存的大小，尽可能多的分批次的将数据 Load 到内存中，并使用系统自带的内存排序函数（或者自己写个快速排序算法），将其排好序，并输出到一个个小文件中。比如一个文件有1T，内存有1G，那么我们就这个大文件中的内容按照 1G 的大小，分批次的导入内存，排序之后输出得到 1024 个 1G 的小文件。

### 第二步：K路归并算法
K路归并算法使用的是数据结构堆（Heap）来完成的，使用 Java 或者 C++ 可以直接用语言自带的 PriorityQueue（C++中叫priority_queue）来代替。

将 K 个文件中的第一个元素加入到堆里，假设数据是从小到大排序的话，那么这个堆是一个最小堆（Min Heap）。每次从堆中选出最小的元素，输出到目标结果文件中，然后如果这个元素来自第 x 个文件，则从第 x 个文件中继续读入一个新的数进来放到堆里，并重复上述操作，直到所有元素都被输出到目标结果文件中。

## Follow-up
如果每个文件只读入1个元素并放入堆里的话，总共只用到了 1024 个元素，这很小，没有充分的利用好内存。另外，单个读入和单个输出的方式也不是磁盘的高效使用方式。

因此，可以为输入和输出都分别加入一个缓冲（Buffer）。假如一个元素有10个字节大小的话，1024 个元素一共 10K，1G的内存可以支持约 100K 组这样的数据，那么就为每个文件设置一个 100K 大小的 Buffer，每次需要从某个文件中读数据，都将这个 Buffer 装满。当然 Buffer 中的数据都用完的时候，再批量的从文件中读入。输出同理，设置一个 Buffer 来避免单个输出带来的效率缓慢。


# 彩虹排序
## Problem
给出colors=[3, 2, 2, 1, 4], k = 4, 原地操作使得数组变成[1, 2, 2, 3, 4]。要求算法时间复杂度要求到 O(nlogk), k为颜色个数。

## 思路
算法的思想类似与 quickSort 与 mergeSort 结合, quickSort 的思想在于 partition 进行分割, mergeSort 的思想在于直接取中间 (这里表现为取中间大小的数), 分为左右两个相等长度的部分。区别在于 partition 的判定条件变为了中间大小的元素而不是中间位置的元素, 因此等号的取值可以只去一边也不会有影响。

RainbowSort 实现的是将 colors 数组的索引范围 start 到 end 位置排序, 排序的大小范围是 colorFrom 到 coloTo。参考程序: https://www.codetd.com/article/3451409

