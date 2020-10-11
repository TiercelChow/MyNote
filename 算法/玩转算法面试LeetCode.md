[toc]

## 第1章 算法面试到底是什么鬼?

### 1.1 算法面试不仅仅是正确的回答问题

> +-*-正++确本身是一个相对的概念

以对一组数据进行排序为例：

- 一般情况使用快速排序最优；
- 若可能包含有大量重复的元素，三路快排是更好的选择；
- 若大部分数据距离它的正确位置很近，近乎有序，插入排序是更好的选择；
- 若数据的取值范围非常有限，计数排序是更好的选择；
- 若需要稳定排序，则归并排序是更好的选择；
- 若数据存储是链表而非随机读取的数组，则使用归并排序；
- 若数据量很大，或者内存很小，不足以装载在内存里，需要使用外排序算法；

正确还包含对问题的独到见解，优化，代码规范，容错性等。

### 1.2 算法面试只是面试的一部分

- 项目经历和项目中遇到的实际问题
- 影响最深的bug
- 面向对象
- 设计模式
- 网络相关；安全相关；内存相关；并发相关；……
- 系统设计；scalability

### 1.3 如何准备算法面试

> 算法面试的准备范围

- 各种排序算法；
- 基础数据结构和算法的实现：如堆、二叉树、图……
- 基础数据结构的使用：如链表、栈、队列、哈希表、图、Trie、并查集……
- 基础算法：深度优先、广度优先、二分查找、递归……
- 基本算法思想：递归、分治、回溯搜索、贪心、动态规划……

> 选择合适的OJ

- [LeetCode](https://www.leetcode.com) 真实的面试问题
- [HackerRank](https://www.hackerrank.com) 对问题的分类很详细

### 1.4 如何回答算法面试问题

- 先通过简单样例进行试验
- 不要忽视暴力解法
- 优化算法
  - 遍历常见的算法思路
  - 遍历常见的数据结构
  - 空间和时间的交换（哈希表）
  - 预处理信息（排序）
  - 瓶颈处寻找答案（时间复杂度高的部分）
- 实际问题的编写
  - 极端条件的判断（数组为空？字符串为空？数量为0？指针为NULL？）
  - 变量名
  - 模块化，复用性

## 第2章 面试中的复杂度分析

### 2.1 究竟什么是大O（Big O）

若一个问题的数据规模为n，那么O(f(n))表示运行算法所需要执行的指令数，和f(n)成正比

在学术界，严格的讲，O(f(n))表示算法执行的上界；在业界，我们就使用O来表示算法执行的最低上界

当算法有多个部分，那么该算法以量级最高的时间复杂度作为主导，如O(n + nlogn)的算法时间复杂度为O(nlogn)，但是该情况的前提多个部分处理的问题规模n相同

若时间复杂度如O(AlogA + B)，A、B之间没有关联，无法保证AlogA是一定大于B的，那么时间复杂度即为O(AlogA + B)

> 一个时间复杂度问题

有一个字符串数组，将数组的每一个我字符串按照字母序排序，之后再将整个字符串数组按照字典序排序，求整个操作的时间复杂度。

假设最长的字符串长度为s；数组中有n个字符串

- 对每个字符串排序：O(slogs)；
- 将数组中的每一个字符串按照字母序排序：O(n*slog(s))；
- 将整个字符串数组按照字典序排序：O(s*nlog(n))；（整型数组比较的时间复杂度为O(1)，而字符串字典序比较的时间复杂度为O(s)）

故时间复杂度为：O(n\*slog(s)) + O(s\*nlog(n)) = O(n\*slogs + s*nlogn) 

算法复杂度在有些情况下是用例相关的，通常说的某一算法的时间复杂度为其平均时间复杂度

### 2-2 对数据规模有一个概念

> 对程序运行时间进行统计方法

```c++
#include <iostream>
#include <ctime>

using namespace std;

int main() {
    //程序开始
    clock_t startTime = clock();
    //...
    //程序结束
    clock_t endTime = clock();
    cout << double(endTime - startTime) / CLOCKS_PER_SEC << "s" << endl;
    //得到的结果单位为秒，endTime - startTime得到的是运行该程序段所需要的时钟周期数，CLOCKS_PER_SEC为一秒钟内CPU运行的时钟周期数
    return 0;
}
```

1s可以执行的操作次数大约为10^8级别

> 空间复杂度

- 多开一个辅助的数组：O(n)
- 多开一个辅助的二维数组：O(n^2)
- 多开常数空间：O(1)

递归的调用有空间代价，递归调用的空间复杂度即为其调用的深度（其等效的循环的空间复杂度为O(1)）

### 2-3 简单的复杂度分析





-  2-4 亲自试验自己算法的时间复杂度
-  2-5 递归算法的复杂度分析
-  2-6 均摊时间复杂度分析（Amortized Time Analysis）
- 2-7 避免复杂度的震荡
- 3-1 从二分查找法看如何写出正确的程序
-  3-2 改变变量定义，依然可以写出正确的算法
-  3-3 在LeetCode上解决第一个问题 Move Zeros
-  3-4 即使简单的问题，也有很多优化的思路
-  3-5 三路快排partition思路的应用 Sort Color
-  3-6 对撞指针 Two Sum II - Input Array is Sorted
-  3-7 滑动窗口 Minimum Size Subarray Sum
- 3-8 在滑动窗口中做记录 Longest Substring Without Repeating Characters
- 4-1 set的使用 Intersection of Two Arrays
-  4-2 map的使用 Intersection of Two Arrays II
-  4-3 set和map不同底层实现的区别
-  4-4 使用查找表的经典问题 Two Sum
-  4-5 灵活选择键值 4Sum II
-  4-6 灵活选择键值 Number of Boomerangs
-  4-7 查找表和滑动窗口 Contain Duplicate II
- 4-8 二分搜索树底层实现的顺序性 Contain Duplicate III
-  5-1 链表，在节点间穿针引线 Reverse Linked List
-  5-2 测试你的链表程序
-  5-3 设立链表的虚拟头结点 Remove Linked List Elements
-  5-4 复杂的穿针引线 Swap Nodes in Pairs
-  5-5 不仅仅是穿针引线 Delete Node in a Linked List
- 5-6 链表与双指针 Remove Nth Node Form End of List
-  6-1 栈的基础应用 Valid Parentheses
-  6-2 栈和递归的紧密关系 Binary Tree Preorder, Inorder and Postorder Traversal
-  6-3 运用栈模拟递归
-  6-4 队列的典型应用 Binary Tree Level Order Traversal
-  6-5 BFS和图的最短路径 Perfect Squares
-  6-6 优先队列
- 6-7 优先队列相关的算法问题 Top K Frequent Elements
-  7-1 二叉树天然的递归结构
-  7-2 一个简单的二叉树问题引发的血案 Invert Binary Tree
-  7-3 注意递归的终止条件 Path Sum
-  7-4 定义递归问题 Binary Tree Path
-  7-5 稍复杂的递归逻辑 Path Sum III
- 7-6 二分搜索树中的问题 Lowest Common Ancestor of a Binary Search Tree
- 8-1 树形问题 Letter Combinations of a Phone Number
-  8-2 什么是回溯
-  8-3 排列问题 Permutations
-  8-4 组合问题 Combinations
-  8-5 回溯法解决组合问题的优化
-  8-6 二维平面上的回溯法 Word Search
-  8-7 floodfill算法，一类经典问题 Number of Islands-
- 8-8 回溯法是经典人工智能的基础 N Queens
-  9-1 什么是动态规划
-  9-2 第一个动态规划问题 Climbing Stairs
-  9-3 发现重叠子问题 Integer Break
-  9-4 状态的定义和状态转移 House Robber
-  9-5 0-1背包问题
-  9-6 0-1背包问题的优化和变种
-  9-7 面试中的0-1背包问题 Partition Equal Subset Sum
-  9-8 LIS问题 Longest Increasing Subsequence
- 9-9 LCS，最短路，求动态规划的具体解以及更多
- 10-1 贪心基础 Assign Cookies
-  10-2 贪心算法与动态规划的关系 Non-overlapping Intervals
-  10-3 贪心选择性质的证明

11-1 结语