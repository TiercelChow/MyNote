[toc]

## 第一章 基础算法

### 1.1 排序

- 快速排序
- 归并排序

> 快速排序（不稳定的排序）

分治思想

步骤（对左边界为l，右边界为r的一段数进行排序）：

1. 确定分界点：q[l], q[(l + r) / 2], q[r], 随机值
2. 调整区间（重点）：通过x对区间进行划分，使得左边区间都≤x，右边区间都≥x（左右区间不一定相等）
3. 递归处理左右两个区间

调整区间的方式：

1. 设置两个指针i，j分别指向区间的左右两个元素
2. i指针向右移动，直至其指向的元素≥x，停下
3. j指针向左移动，直至其指向的元素≤x，停下
4. 交换i，j所指向的元素（两个指针分别向中间移动一位）
5. 若i和j相遇，结束；否则跳至步骤2

一个模板：

```c++
void quick_sort(int q[], int l, int r) {
    if (l >= r) return;

    int i = l - 1, j = r + 1, x = q[l + r >> 1];
    while (i < j) {
        do i ++ ; while (q[i] < x);
        do j -- ; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }
    quick_sort(q, l, j), quick_sort(q, j + 1, r);
}
```

代码解释：

第3行i和j分别先往两端偏移1的原因使每一次两个数进行交换后都需要往中间走一步，因此索性在每次执行之前就走一步，而开始需要事先向两端偏移1才能抵消该操作带来的错误影响

一个边界问题：9、10行中可以为(q, l, j)、(q, j + 1, r)，这种情况x不能取q[r]和中间值上取整（(l + r + 1) / 2），或者(q, l, i - 1)、(q, i, r)，这种情况x不能取q[l]和中间值下取整（(l + r - 1) / 2），否则都将产生死循环，如q[2] = {1, 2}，若x的取值不符要求，则会发生一直执行quick)_sort(q, 0, 1)的死循环。

若需要使快排为稳定的排序，可将元素扩展成值和下标的二元组后进行排序（但无意义）

> 归并排序（稳定的排序）

同样是分治思想

步骤（对左边界为l，右边界为r的一段数进行排序）：

1. 确定分界点：mid = (l + r) / 2
2. 递归排序左半部分和右半部分
3. 归并——合二为一

主要思想（双指针）：两个指针分别对应数组左半部分和右半部分（均已排好序），然后两个指针均向右移动，比较两个指针的值的大小，值小的依次放入用于存储排好序后的结果的数组（升序排序）

一个模板：

```c++
//首先定义一个与q数组类型、大小相同的数组tmp
void merge_sort(int q[], int l, int r) {
    if (l >= r) return;
    int mid = l + r >> 1;
    merge_sort(q, l, mid);
    merge_sort(q, mid + 1, r);
    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r)
        if (q[i] <= q[j]) tmp[k ++ ] = q[i ++ ];
        else tmp[k ++ ] = q[j ++ ];
    while (i <= mid) tmp[k ++ ] = q[i ++ ];
    while (j <= r) tmp[k ++ ] = q[j ++ ];
    for (i = l, j = 0; i <= r; i ++, j ++ ) q[i] = tmp[j];
}
```

### 1.2 二分搜索

- 整数二分
- 实数二分

> 整数二分

本质并非单调性（有单调性必可二分，但二分不一定必须要单调性）

若存在某个条件可以将区间[l, r]分为两个区间，而两个区间分隔的点为所需要寻找的点（check条件为true的区间的边界点），令mid = (l + r  + 1) / 2;（不补上+1在l, r相邻时会导致死循环）每次根据某条件进行check，（假定右部区间为true）

- if(check(mid))为true，l = mid， 区间更新为[mid, r]
- if(check(mid))为false，r = mid - 1， 区间更新为[l, mid - 1]

（左部区间为true）mid = (l + r) / 2

- if(check(mid))为true，r = mid， 区间更新为[l, mid]
- if(check(mid))为false，l = mid + 1， 区间更新为[mid + 1, r]

解题步骤：先不考虑mid是否需要补1，写出mid和check()，根据check(mid)为true时区间更新情况判断属于哪类情况对mid进行修正

模板：

```c++
bool check(int x) {/* ... */} // 检查x是否满足某种性质

// 区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用：
int bsearch_1(int l, int r) {
    while (l < r) {
        int mid = l + r >> 1;
        if (check(mid)) r = mid;    // check()判断mid是否满足性质
        else l = mid + 1;
    }
    return l;
}
// 区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
int bsearch_2(int l, int r) {
    while (l < r) {
        int mid = l + r + 1 >> 1;
        if (check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```

> 实数二分

实数二分与整数二分类似，但循环终止条件一般为l与r之间的差距被认为足够小

模板：

```c++
bool check(double x) {/* ... */} // 检查x是否满足某种性质

double bsearch_3(double l, double r) {
    const double eps = 1e-6;   // eps 表示精度，取决于题目对精度的要求
    while (r - l > eps) {
        double mid = (l + r) / 2;
        if (check(mid)) r = mid;
        else l = mid;
    }
    return l;
}
```

### 1.3 高精度

- 大整数加法
- 大整数减法
- 大整数乘法
- 大整数除法

对于上述运算，均只考虑了两个数都为整数的情况，但是若存在负数情况类似，只是输入需要进行修改，加法若存在负数可转换为减法或者绝对值相加再取负， 减法若存在负数可转换为加法或绝对值之和取负，乘法和出发中若存在负数对符号进行单独处理即可。

> 大整数的存储

通过字符串进行存储，由于进位等操作，有别于正常的高位在前，低位在后的存储方法，运算时将字符串存储的大整数低位在前，高位在后存储在整型数组中

> 大整数加法

思路：

对应位权进行相加，之后处理进位

模板：

```c++
// C = A + B, A >= 0, B >= 0
vector<int> add(vector<int> &A, vector<int> &B) {
    if (A.size() < B.size()) return add(B, A);

    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size(); i ++ ) {
        t += A[i];
        if (i < B.size()) t += B[i];
        C.push_back(t % 10);
        t /= 10;
    }
    if (t) C.push_back(t);
    return C;
}
```

代码解释：

对于在函数的自变量中传入A，B的引用而不是直接传入A，B的原因：直接传入需要将整个数组拷贝，而引用不需要，减少了拷贝过程中时间的消耗，运行速度更快

t用于记录相同权值的位相加的结果并向上一位传递进位

> 大整数减法

思路：

首先判断A和B的大小，若A≥B，计算A - B即可，否则计算- (B - A)；

模板：

```c++
bool cmp(vector<int> &A, vector<int> &B) {
    if(A.size() != B.size()) return A.size() > B.size();
    for (int i = A.size() - 1; i >= 0, i --) {
        if (A[i] != B[i]) return A[i] > B[i];
    }
    return true;
} 

// C = A - B, 满足A >= B, A >= 0, B >= 0
vector<int> sub(vector<int> &A, vector<int> &B) {
    vector<int> C;
    for (int i = 0, t = 0; i < A.size(); i ++ ) {
        t = A[i] - t;
        if (i < B.size()) t -= B[i];
        C.push_back((t + 10) % 10);
        if (t < 0) t = 1;
        else t = 0;
    }

    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}
```

代码解释：                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 

先判断A，B的大小，保证C为正数，若A ≥ B，C直接为其结果， 若A < B，在结果前加个负号即可，代码中(t + 10) % 10用于处理借位的操作，t同时也用于向上一位传递借位的情况。while循环用于处理前导零

> 大整数乘法（一个高精度数乘以一个低精度数的情况）



思路：

先将b与A中每一位相乘，之后处理进位

模板：

```c++
// C = A * b, A >= 0, b > 0
vector<int> mul(vector<int> &A, int b) {
    vector<int> C;
    int t = 0;
    for (int i = 0; i < A.size() || t; i ++ ) {
        if (i < A.size()) t += A[i] * b;
        C.push_back(t % 10);
        t /= 10;
    }
    //b == 0
    //while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}
```

代码解释：上面for循环的写法相当于合并了两个循环，先处理前A.size()位，然后处理A最高位和b相乘加上上一位产生的进位产生的进位

两个高精度数相乘通过[快速傅里叶变换](https://blog.csdn.net/enjoy_pascal/article/details/81478582)实现

> 大整数除法（一个高精度数除以一个低精度数）

思路：

每次取一位与b作除法，将商填入C，用r记录余数

模板：

```c++
// A / b = C ... r, A >= 0, b > 0
vector<int> div(vector<int> &A, int b, int &r) {
    vector<int> C;
    r = 0;
    for (int i = A.size() - 1; i >= 0; i -- ) {
        r = r * 10 + A[i];
        C.push_back(r / b);
        r %= b;
    }
    reverse(C.begin(), C.end());
    while (C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}
```

代码解释：

由于C中记录商高位在前，而为了遵循之前约定的高位再前，故通过reverse()函数将其倒置，该函数需要引入头文件<algorithm>，而最后需要注意处理前导零问题

