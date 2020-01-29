# Table of Contents
- [Time Complexity](#time-complexity)
- [Stack](#stack)
- [Queue](#queue)
- [Hash](#hash)
- [Heap](#heap)
- [Other](#other)
- [Reference](#reference)


数据结构 (Data Structures) 可以认为是一个数据存储集合以及定义这个集合上的若干操作(功能)。有如下三种考法：
1. 问某种数据结构的基本原理，并要求实现 (Hash的原理并实现)
2. 使用某种数据结构完成事情 (归并K个有序数组)
3. 实现一种数据结构，提供一些特定的功能 (最高频K项问题)

## Time Complexity
数据结构通常会提供“多个”对外接口，只用一个时间复杂度是很难对其进行正确评价，所以通常需要对每个接口的时间复杂度进行描述。

譬如需要设计一个 Set 的数据结构，提供 lowerBound 和 add 两个方法。lowerBound 的意思是，找到比某个数小的最大值。
| 算法实现 | lowerBound | add |
| --- | --- | --- |
| 使用数组存储，每次打擂台进行比较，插入就直接插入到数组最后面 | O(n) | O(1) |
| 使用红黑树(Red-black Tree)存储，Java 里的 TreeSet，C++ 里的 map | O(logn) | O(logn) |

使用哪种算法，取决于这两个方法被调用的频率如何。
- 如果 lowerBound 很少调用, add 非常频繁, 则选择数组
- 如果 lowerBound 和 add 调用的频率都差不多，或者 lowerBound 被调用得更多，则红黑树更优

在面试中的问题，很容易可以找到一个像算法1这样的实现方法，其中一个操作时间复杂度很大，另外一个操作时间复杂度很低。而通常的解决办法，都是让快的操作慢一点，让慢的操作快一点，这样总体得到一个更快的复杂度。

哈希表(HashMap / unordered_map / dict) 任何操作的时间复杂度从严格意义上来说，都是 O(L)/O(size of key) 而不是 O(1)。譬如在 O(1) 的时间内不可能判断两个 1M 长的字符串是否相等。

| 高级数据结构 | O(1)/O(L) | O(logn) | 解决问题 | 考察频率 | 学习难度 | Note | 
| --- | --- | --- | --- | --- | --- | --- |
| Heap | top | push, pop | 全局动态找最大找最小 | 高 | 低 |  |
| Hash Map | insert, find, delete |  | 查询元素是否存在, key-value 查询问题 | 高 | 低 |  |
| Trie | insert, find, delete |  | 和哈希表解决问题类似，查询元素是否存在，key-value 查询问题 | 中 | 低 | O(L) |
| UnionFind | union, find |  | 动态合并集合并判断两个元素是否在同一个集合，MST | 中 | 低 |  |
| Balanced BST |  | insert, find, delete, max, min, lower, upper | 动态增删查改并支持同时找全局最大最小值，找比某个数大的最小值和比某个数小的最大值时可以用(尽可能接近) | 低 | 高 | 如 Red-black Tree |
| Skip List |  | insert, find, delete, max, min, lower, upper |  和 Balanced BST 解决的问题一 样，并能一直维持一个有序链表 | 低 | 高 |
| Binary Indexed Tree |  | insert, delete, range sum | 增删改的同时，解决区间求和问题 | 低 | 中 |  |
| Segment Tree | global max, global min | insert, delete, find, range max, range min, range sum, lower, upper | 增删改的同时，解决区间求值问题，max/min/sum 等等。可以完全替代 | 低 | 中 |  |


## Stack
栈（stack）是一种采用后进先出（LIFO，last in first out）策略的抽象数据结构。栈一般用一个存储结构（常用数组，少见链表）存储元素。并用一个指针记录栈顶位置。栈底位置则是指栈中元素数量为0时的栈顶位置，也即栈开始的位置。

### 主要操作
- push()，将新的元素压入栈顶，同时栈顶上升
- pop()，将新的元素弹出栈顶，同时栈顶下降
- empty()，栈是否为空
- peek()，返回栈顶元素

在各语言的标准库中：
- Java: 使用`java.util.Stack`，它是扩展自`Vector`类，支持`push()`，`pop()`，`peek()`，`empty()`，`search()`等操作
- C++: 使用`<stack>`中的`stack`即可，方法类似Java，只不过C++中`peek()`叫做`top()`，而且`pop()`时，返回值为空
- Python: 直接使用`list`，查看栈顶用`[-1]`这样的切片操作，弹出栈顶时用`list.pop()`，压栈时用`list.append()`

### 基本实现
这里给出一种用`ArrayList`的通用实现方法。(注意：为了将重点放在栈结构上，做了适当简化。该栈仅支持整数类型，若想实现泛型，可用反射机制和object对象传参；此外，可多做安全检查并抛出异常)

```java
public class Stack {
    List<Integer> array = new ArrayList<Integer>();

    public void push(int x) {
        array.add(x);
    }

    public void pop() {
        if (!isEmpty()) {
            array.remove(array.size() - 1);
        }
    }

    public int peek() {
        return array.get(array.size() - 1);
    }

    public boolean isEmpty() {
        return array.size() == 0;
    }
}
```

```py
class Stack:
    def __init__(self):
        self.array = []
		
    def push(self, x):
        self.array.append(x)

    def pop(self):
        if not self.isEmpty():
            self.array.pop()

    def top(self):
        return self.array[-1]

    def isEmpty(self):
        return len(self.array) == 0
```

### 应用
在程序运行时，常说的内存中的堆栈，其实就是栈空间。这一段空间存放着程序运行时，产生的各种临时变量、函数调用，一旦这些内容失去其作用域，就会被自动销毁。
函数调用其实是栈的很好的例子，后调用的函数先结束，所以为了调用函数，所需要的内存结构，栈是再合适不过了。在内存当中，栈从高地址不断向低地址扩展，随着程序运行的层层深入，栈顶指针不断指向内存中更低的地址。

Reference: https://blog.csdn.net/liu_yude/article/details/45058687


## Queue
队列为一种先进先出的线性表。只允许在表的一端进行入队，在另一端进行出队操作。在队列中，允许插入的一端叫队尾，允许删除的一端叫队头，即入队只能从队尾入，出队只能从队头出

### 基本实现
1. 需要两个节点，一个头部节点，也就是dummy节点，它是在加入的第一个元素的前面，也就是dummy.next=第一个元素，这样做是为了方便我们删除元素，还有一个尾部节点，也就是tail节点，表示的是最后一个元素的节点
2. 初始时，tail节点跟dummy节点重合
3. 当我们要加入一个元素时，也就是从队尾中加入一个元素，只需要新建一个值为val的node节点，然后tail.next=node，再移动tail节点到tail.next
4. 当我们需要删除队头元素时，只需要将dummy.next变为dummy.next.next，这样就删掉了第一个元素，这里需要注意的是，如果删掉的是队列中唯一的一个元素，那么需要将tail重新与dummy节点重合
5. 当我们需要得到队头元素而不删除这个元素时，只需要获得dummy.next.val就可以了

```java
class QueueNode {
    public int val;
    public QueueNode next;
    public QueueNode(int value) {
        val = value;
    }
}

public class Queue {
    private QueueNode dummy, tail;

    public Queue() {
        dummy = new QueueNode(-1);
        tail = dummy;
    }

    public void enqueue(int val) {
        QueueNode node = new QueueNode(val);
        tail.next = node;
        tail = node;
    }

    public int dequeue() {
        int ele = dummy.next.val;
        dummy.next = dummy.next.next;

        if (dummy.next == null) {
            tail = dummy;   // reset
        }
        return ele;
    }

    public int peek() {
        int ele = dummy.next.val;
        return ele;
    }

    public boolean isEmpty() {
        return dummy.next == null;
    }
}
```

```py
class QueueNode:
    def __init__(self, value):
        self.val = value
        self.next = None


class Queue:
    def __init__(self):
        self.dummy = QueueNode(-1)
        self.tail = self.dummy

    def enqueue(self, val):
        node = QueueNode(val)
        self.tail.next = node
        self.tail = node

    def dequeue(self):
        ele = self.dummy.next.val
        self.dummy.next = self.dummy.next.next

        if not self.dummy.next:
            self.tail = self.dummy
        return ele

    def peek(self):
        return self.dummy.next.val

    def isEmpty(self):
        return self.dummy.next == None
```

### 循环数组
A data structure that used a array as if it were connected end-to-end.

1. 队列的入队操作是只在队尾进行的，相对的出队操作是只在队头进行的，所以需要两个变量front与rear分别来指向队头与队尾
2. 由于是循环队列，在增加元素时，如果此时 rear = array.length - 1，rear 需要更新为 0；同理，在元素出队时，如果 front = array.length - 1, front 需要更新为 0。对此，可以通过对数组容量取模来更新。

```py
class CircularQueue:
    def __init__(self, n):
        self.circularArray = [0]*n
        self.front = 0
        self.rear = 0
        self.size = 0
        
    """
    @return:  return true if the array is full
    """
    def isFull(self):
        return self.size == len(self.circularArray)

    """
    @return: return true if there is no element in the array
    """
    def isEmpty(self):
        return self.size == 0

    """
    @param element: the element given to be added
    @return: nothing
    """
    def enqueue(self, element):
        if self.isFull():
            raise RuntimeError("Queue is already full")
        self.rear = (self.front+self.size) % len(self.circularArray)
        self.circularArray[self.rear] = element
        self.size += 1

    """
    @return: pop an element from the queue
    """
    def dequeue(self):
        if self.isEmpty():
            raise RuntimeError("Queue is already empty")
        ele = self.circularArray[self.front]
        self.front = (self.front+1) % len(self.circularArray)
        self.size -= 1
        return ele
```


## Hash
A hash function is any function that can be used to map data of arbitrary size onto data of a fixed size. The values returned by a hash function are called hash values, hash codes, digests, or simply hashes.

冲突（Collision），是说两个不同的 key 经过哈希函数的计算后，得到了两个相同的值。解决冲突的方法，主要有两种：
1. 开散列法（Open Hashing）。是指哈希表所基于的数组中，每个位置是一个 Linked List 的头结点。这样冲突的 <key, value> 二元组，就都放在同一个链表中。
2. 闭散列法（Closed Hashing）。是指在发生冲突的时候，后来的元素，往下一个位置去找空位。


## Heap
堆一般为二叉树。A heap is a specialized tree-based data structure which is essentially an almost complete tree that satisfies the heap property: 
- In a max heap, for any given node C, if P is a parent node of C, then the key (the value) of P is greater than or equal to the key of C. 
- In a min heap, the key of P is less than or equal to the key of C. The node at the "top" of the heap (with no parents) is called the root node.
- For time complexity: `add()` takes O(logn), `poll()` takes O(logn), while `peak()` takes O(1)

### MinHeap
#### SiftUp
##### 思路
1. 对于每个元素A[i]，比较A[i]和它的父亲结点的大小，如果小于父亲结点，则与父亲结点交换。
2. 交换后再和新的父亲比较，重复上述操作，直至该点的值大于父亲。

##### Time Complexity
1. 对于每个元素都要遍历一遍，这部分是 O(n)。
2. 每处理一个元素时，最多需要向根部方向交换 logn 次。

因此总的时间复杂度是 O(nlogn)。
```py
class Solution:
    # @param A: Given an integer array
    # @return: void
    def siftup(self, A, k):
        while k != 0:
            father = (k - 1) // 2
            if A[k] > A[father]:
                break
            temp = A[k]
            A[k] = A[father]
            A[father] = temp
            
            k = father
    def heapify(self, A):
        for i in range(len(A)):
            self.siftup(A, i)
```

#### SiftDown
##### 思路
1. 初始选择最接近叶子的一个父结点，与其两个儿子中较小的一个比较，若大于儿子，则与儿子交换。
2. 交换后再与新的儿子比较并交换，直至没有儿子。
3. 再选择较浅深度的父亲结点，重复上述步骤。
```py
class Solution:
    # @param A: Given an integer array
    # @return: void
    def siftdown(self, A, k):
        while k * 2 + 1 < len(A):
            son = k * 2 + 1
            if k * 2 + 2 < len(A) and A[son] > A[k * 2 + 2]:
                son = k * 2 + 2
            if A[son] >= A[k]:
                break
                
            temp = A[son]
            A[son] = A[k]
            A[k] = temp
            k = son
    
    def heapify(self, A):
        for i in range(len(A) - 1, -1, -1):
            self.siftdown(A, i)
```

##### Time Complexity
这个版本的算法，乍一看也是 O(nlogn)， 但是仔细分析一下，算法从第 n/2 个数开始，倒过来进行 siftdown。也就是说，相当于从 heap 的倒数第二层开始进行 siftdown 操作，倒数第二层的节点大约有 n/4 个， 这 n/4 个数，最多 siftdown 1次就到底了，所以这一层的时间复杂度耗费是 O(n/4)，然后倒数第三层差不多 n/8 个点，最多 siftdown 2次就到底了。所以这里的耗费是 O(n/8 * 2), 倒数第4层是 O(n/16 * 3)，倒数第5层是 O(n/32 * 4) ... 因此累加所有的时间复杂度耗费为：`T(n) = O(n/4) + O(n/8 * 2) + O(n/16 * 3) ...`

然后我们用 2T - T 得到：
```
2 * T(n) = O(n/2) + O(n/4 * 2) + O(n/8 * 3) + O(n/16 * 4) ... 
T(n)     =          O(n/4)     + O(n/8 * 2) + O(n/16 * 3) ...

2 * T(n) - T(n) = O(n/2) +O (n/4) + O(n/8) + ...
                = O(n/2 + n/4 + n/8 + ... )
                = O(n)
```
因此得到 T(n) = 2 * T(n) - T(n) = O(n)。

#### Comparison
The reason why their time complexity is different is that: In SiftUp, every element would be possible to be sifted up to the root; while in SiftDown, only all the largest elments would be sifted down to leaf.


## Other
虽然堆栈，堆栈的说法是连起来叫，但是他们还是有很大区别的，连着叫只是由于历史的原因。堆和栈的区别主要为:
- 操作系统方面，可用如下比喻来区别:
    - 使用栈就像是去饭馆里吃饭，只管点菜（发出申请）、付钱、和吃（使用），吃饱了就走，不必理会切菜、洗菜等准备工作和洗碗、刷锅等扫尾工作，它的好处是快捷，但是自由度小。 
    - 使用堆就象是自己动手做喜欢吃的菜肴，比较麻烦，但是比较符合自己的口味，而且自由度大。  
- 数据结构方面，这些都是不同的概念：
    - 这里的堆实际上指的就是（满足堆性质的）优先队列的一种数据结构，第1个元素有最高的优先权
    - 而栈实际上就是满足先进后出的性质的数学或数据结构。


## Reference
- 内存中堆栈的理解: https://blog.csdn.net/liu_yude/article/details/45058687

