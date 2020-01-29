# Table of Contents
- [Double Pointers](#double-pointers)
- [Sorting](#sorting)


# Double Pointers
## 相向/背向 (Opposite Direction)
- Reverse 类： 三步翻转、回文串
- Two Sum 类
- Partition 类

## 同向 (Same Direction)
- 数组去重
- 滑动窗口
- 两数之差
- 链表中点
- 带环链表

# Sorting
## Quick Sort
Other than set pivot to the middle index, getting the pivot value from middle position before partition since the value in the middle would change.

In partition, it's important to leave the pivot value on either side, which means both pointers would stop at the position equals to pivot value then swap and both continue to move one position, so that the left and right part can be distributed equally and randomly. The recursion would possibly cause stack overflow otherwise.

## Merge Sort
Normally, since Merge Sort needs extra space to manage the sorting, it takes more time than Quick Sort even though both of them have time complexity O(nlogn). Do not remember to merge both sides back after each side finishes its sorting.

## Comparison
QuickSort: from the whole to the part
Merge Sort: from the part to the whole

| Sorting | Best | Average | Worst | Memory | Stable |
| --- | --- | --- | --- | --- | --- |
| QuickSort | nlogn | nlogn | n<sup>2</sup> |  logn ~ n | No |
| Merge Sort | nlogn | nlogn | nlogn | n | Yes |
| Insertion Sort | n | n<sup>2</sup> | n<sup>2</sup> | 1 | Yes |
| Selection Sort | n<sup>2</sup> | n<sup>2</sup> | n<sup>2</sup> | 1 | No |
| Bubble Sort | n | n<sup>2</sup> | n<sup>2</sup> | 1 | Yes |
| Shell Sort | nlogn | - | - | 1 | No |

Reference: https://en.wikipedia.org/wiki/Sorting_algorithm



