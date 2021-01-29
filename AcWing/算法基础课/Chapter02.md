[toc]

## 第二章 数据结构

### 2.1 链表与邻接表

> 动态链表（结构体+指针）

```c++
struct Node
{
  int val;
  Node Next;
};
//每次获取新结点都需要new操作，非常慢
new Node();
```

若采用结构体时在开始初始化一定数量结点可以变快，但其本质与数组模拟链表类似，故通过数组模拟链表。

> 单链表

邻接表：存储树和图

通过两个数组对单链表进行模拟，e[N]表示每个结点存储的值value，ne[N]表示每个结点的next指针的值，两个数组通过数组下标进行关联，空结点的下标用-1表示。

```c++
// head存储链表头，e[]存储节点的值，ne[]存储节点的next指针，idx表示当前用到了哪个节点
int head, e[N], ne[N], idx;

// 初始化
void init()
{
    head = -1;
    idx = 0;
}

// 在链表头插入一个数a
void insert(int a)
{
    e[idx] = a, ne[idx] = head, head = idx ++ ;
}

// 将头结点删除，需要保证头结点存在
void remove()
{
    head = ne[head];
}
```

> 双链表

优化某些问题

每个结点有两个指针分别指向其左结点和右结点。