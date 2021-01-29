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

解题步骤：

先不考虑mid是否需要补1，写出mid和check()，根据check(mid)为true时区间更新情况判断属于哪类情况对mid进行修正

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

### 1.4 前缀和与差分

> 一维前缀和

对于一个长度为n + 1的数组，下标从1开始存储n个数，下标为0处存储0那么：

- 前缀和（前i项的和）Si = a[1] + a[2] + ... + a[i]
- 如何求前缀和：S[i] = S[i- 1] + a[i]
- 作用：对于给定的任意区间[l , r]求和公式为S[r] - S[l - 1]

数组下标从0开始是为了是每一个元素都满足上述公式，避免数组的越界情况

```c++
//对于大量输入输出若使用cin，cout提高读取速度的方法
ios::sync_with_stdio(false);
//原理：cin，cout之所以效率低，这是因为C++中，cin和cout要与stdio同步，中间会有一个缓冲，所以导致cin，cout语句输入输出缓慢，这时就可以用这个语句，取消cin，cout与stdio的同步
//副作用：scanf和printf将不可使用
```

> 二维前缀和

对于二维数组a\[i\]\[j\]，其前i行，前j列的和为S\[i\]\[j\]

- S\[i\]\[j\] = S\[i-1\]\[j\] + S\[i][j-1] - S\[i-1\]\[j-1\] + a\[i\]\[j\]
- (x1, y1), (x2, y2)两个点为顶点的矩阵中的数的和为：S\[x2\]\[y2\] - S\[x2\]\[y1-1\] - S\[x1-1\]\[y2\] + S\[x1-1\]\[y1-1\]

> 一维差分

差分为前缀和的逆运算

对于长度为n + 1的数组a[n]，构造一个数组b[n]，使得对于任意的a[i]，都有a[i] = b[1] + b[2] + ... + b[i]，则b[n]称为a[n]的差分

- b[n] = a[n] - a[n -1]
- 对于数组a中区间[i, j]中的每个数需要加上一个数t，只需要b[i] += t，b[j + 1] -=t

差分不需要过多考虑初始的构造问题，先令两个数组都为全0，再遍历a数组，对b数组进行修改即可

> 二维差分

概念同理于一维差分，若需要对数组a的[x1, y1]到[x2, y2]范围的数都加上一个数，操作为：

1. b\[x1\]\[y1\] +=t;
2. b\[x2 + 1\]\[y1\] -= t;
3. b\[x1\]\[y2 + 1\] -= t;
4. b\[x2 + 1\]\[y2 + 1\] += t;

### 1.5 双指针算法

双指针的大概模板：

```c++
for (i = 0, j = 0; i < n; i ++ ) {
    while (j < n  && check()) j ++;
    //每道题具体逻辑
}
```

核心思想：

双重循环实现某一问题的时间复杂度为O(n²)，而在双指针算法中每个指针的移动都不超过n，那么二者移动的总次数小于2n，从而降低了时间复杂度，即将双重循环的朴素算法优化为O(n)

### 1.6 位运算

> lowbit(x)

返回x的最后一位1

具体实现：return x & (- x);

等效于x & (~ x + 1)

​                    x = 1010...10...
​                  ~x = 0101...01...1
​            ~x + 1 = 0101...10...0
x & (~ x + 1) = 0000..010...0

结合每一次找到最后一位1，将其置0，然后循环该过程可以用于统计一个数里有多少个1

### 1.7 离散化

（整数、有序的离散化）

解决的问题：

> 数组中可能存在重复元素

排序 + 去重

```c++
vector<int> alls;
sort(alls.begin(),alls.end());
alls.erase(unique(alls.begin(), alls.end()), alls.end());//unique()作用是去重并返回去重后的数组尾端点
```

unique()实现

```c++
vector<int>::iterator unique(vector<int> &a) {
    int j = 0;
    for (int i = 0; i < a.size(); i ++ ) {
        if (!i ||a[i] != a[i - 1]) a[j ++ ] = a[i];
    }
    return a.begin() + j;
}
```

模板：

```c++
vector<int> alls; // 存储所有待离散化的值
sort(alls.begin(), alls.end()); // 将所有值排序
alls.erase(unique(alls.begin(), alls.end()), alls.end());   // 去掉重复元素

// 二分求出x对应的离散化的值
int find(int x) // 找到第一个大于等于x的位置
{
    int l = 0, r = alls.size() - 1;
    while (l < r)
    {
        int mid = l + r >> 1;
        if (alls[mid] >= x) r = mid;
        else l = mid + 1;
    }
    return r + 1; // 映射到1, 2, ...n
}
```

> 如何计算出x离散化后的值

二分求出x对应的离散化的值

```c++
//找到第一个大于等于x的位置
int find(int x) {
    int l = 0, r = alls.size() - 1;
    while(l < r) {
        int mid = l + r >> 1;
        if (alls[mid] >= x) r = mid;
        else l = mid + 1;
    }
    return r + 1;//映射到1,2,3...n
}
```

### 1.8 区间合并

首先根据区间左端点对区间进行排序，然后扫描区间，扫描过程中进行合并

```c++
// 将所有存在交集的区间合并
typedef pair<int, int> PII;

void merge(vector<PII> &segs)
{
    vector<PII> res;

    sort(segs.begin(), segs.end());

    int st = -2e9, ed = -2e9;
    for (auto seg : segs)
        if (ed < seg.first)
        {
            if (st != -2e9) res.push_back({st, ed});
            st = seg.first, ed = seg.second;
        }
        else ed = max(ed, seg.second);

    if (st != -2e9) res.push_back({st, ed});

    segs = res;
}
```

