# 09 · 常用 STL 速查

> 这份是**怎么用**速查：每行都带中文注释说明功能与参数含义，直接看代码就行。
> 全部基于 `#include <bits/stdc++.h>` + `using namespace std;`（你代码开头若已有就别重复贴）。
> 片段里的 a / n / x 等是**示例数据**，写题时替换成你实际的变量。片段默认放进 main 里用。
> 编译需 C++17（结构化绑定、gcd/lcm、clamp、mt19937 等）。

## 1. unordered_map / unordered_set 用法

哈希表：按键增删查，平均 $O(1)$。下面是全部常用写法。

```cpp
int x = 3;                 // 样例键，写题时换成你的
unordered_map<int, int> cnt;            // 建空表 <键, 值>
cnt[x]++;                               // 计数 +1：键不存在会自动建 0 再 +1，不用先判存在
cnt[x] = 5;                             // 插入 / 覆盖值
if (cnt.count(x))                       // 判断键是否存在
{
    // 存在时执行……
}
auto it = cnt.find(x);                  // 找键，找不到返回 cnt.end()
if (it != cnt.end())
{
    int v = it->second;                 // 取键 x 对应的值
}
cnt.erase(x);                           // 删除键 x

// unordered_set：只存键不存值，用来判断“出现过没有” / 去重
unordered_set<int> st;
st.insert(x);                           // 插入
bool has = st.count(x);                 // 1 = 在，0 = 不在
st.erase(x);                            // 删除

// 遍历整张表：p.first 是键，p.second 是值
for (auto &p : cnt)
{
    int key = p.first;
    int val = p.second;
}

// —— 防卡哈希（Codeforces 上默认 unordered_map 可能被 hack 卡成 O(n^2)）——
// 用法和上面完全相同，只是声明时多带一个参数：
// unordered_map<ll, int, custom_hash> mp;                  // 键是 long long
// unordered_map<pair<int, int>, int, pair_hash> mp2;       // 键是 pair
// unordered_set<ll, custom_hash> hs;
// custom_hash / pair_hash 的定义在文末「附录 A」，用到时整段复制一次即可
```

## 2. priority_queue 堆：取最大 / 最小

```cpp
priority_queue<int> mx;                     // 大根堆：top 是当前最大值
priority_queue<int, vector<int>, greater<int>> mn;  // 小根堆：top 是最小值（模板参数固定这么写）

mx.push(5);                                 // 插入
int t = mx.top();                           // 看堆顶（不删除）
mx.pop();                                   // 删除堆顶
bool emp = mx.empty();                      // 是否为空
int sz = (int)mx.size();                    // 元素个数

// 堆里放（权值, 编号）：用 pair，自动先比权值再比编号
priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;   // 小根堆
pq.push({3, 7});                            // 插入：权值 3、编号 7
auto best = pq.top();                       // 当前最小的 (权值, 编号)
int w = best.first;                         // 取权值
int id = best.second;                       // 取编号
pq.pop();                                   // 删堆顶

// 想按“权值大的先出，权值相同编号小的先出”：把权值取负塞小根堆即可
pq.push({-100, 2});                         // 相当于权值 100 最优先
int real_w = -pq.top().first;               // 取出来后把负号还原

// 更复杂的自定义优先级（比如结构体按多个字段比）：
// auto cmp = [](const Item &a, const Item &b) { return a.val > b.val; };  // 想 val 小先出就写 >
// priority_queue<Item, vector<Item>, decltype(cmp)> q(cmp);               // 声明时把 cmp 传进去
// 记忆点：priority_queue 的比较器方向和 sort 相反（返回 true = 优先级更低、更晚出堆）
```

## 3. 对顶堆：动态中位数

```cpp
// 一边插数一边问当前中位数。两个堆全局（或放结构体里）：
priority_queue<int> le;                     // 大根堆：存“较小的一半”
priority_queue<int, vector<int>, greater<int>> ri;   // 小根堆：存“较大的一半”

void add_num(int x)                         // 插入一个数后自动平衡
{
    if (le.empty() || x <= le.top()) le.push(x);     // 比中位数小/相等 → 进左半
    else ri.push(x);                                 // 否则进右半
    if ((int)le.size() > (int)ri.size() + 1)         // 左半多了，把左半最大的搬去右半
    {
        ri.push(le.top());
        le.pop();
    }
    if ((int)ri.size() > (int)le.size())             // 右半多了，把右半最小的搬回左半
    {
        le.push(ri.top());
        ri.pop();
    }
}

int get_mid()                               // 当前中位数（已插入奇数个时）
{
    return le.top();
}

// 用法：每读一个数就 add_num(x)；随时 get_mid() 拿当前中位数。
// 插入的是偶数个时：中位数取 (le.top() + ri.top()) / 2（用 ll 算防溢出），
// 或按题意取 le.top() 当下中位数。需要支持“删除”时改用 multiset（见第 4 节）。
```

## 4. multiset：可重复有序集合怎么用

```cpp
int x = 5;                 // 样例值
multiset<int> s;                            // 元素自动升序排好，可重复
s.insert(5);                                // 插入
int mn = *s.begin();                        // 最小值
int mx = *s.rbegin();                       // 最大值

// 删除“一个”x：必须 erase(find(x))。直接 s.erase(x) 会把所有等于 x 的全删光！
auto it = s.find(x);
if (it != s.end()) s.erase(it);             // 只删这一个

// 前驱 / 后继（x 可以不在集合里）
auto it2 = s.lower_bound(x);                // 第一个 >= x 的位置
if (it2 != s.begin())
{
    int pre = *prev(it2);                   // 前驱：< x 的最大值
}
if (it2 != s.end())
{
    int nxt = *it2;                         // 后继：>= x 的最小值
}
// 要严格 > x 的后继：用 upper_bound(x)
// multiset 常当“可删堆 / 支持删除的中位数”用：插入 s.insert(x)，删除 s.erase(s.find(x))
```

## 5. 单调队列（deque）：滑动窗口最值

```cpp
// 求每个长度 k 的滑动窗口最小值，整体 O(n)
int a[10] = {0, 3, 1, 4, 1, 5, 9, 2};       // 样例：a[1..7] 放数据
int n = 7, k = 3;
int ans[10];                                // 结果放 ans[1..n-k+1]

deque<int> dq;                              // 存下标，队首永远是当前窗口最小值的位置
for (int i = 1; i <= n; i++)
{
    while (!dq.empty() && dq.front() < i - k + 1) dq.pop_front(); // 弹出已滑出窗口的下标
    while (!dq.empty() && a[dq.back()] >= a[i]) dq.pop_back();    // 队尾 >= 新值就没用了，弹出
    dq.push_back(i);                        // 新下标入队
    if (i >= k) ans[i - k + 1] = a[dq.front()];   // 窗口凑满 k 个就记一次
}
// 要窗口最大值：把上面 a[dq.back()] >= a[i] 改成 a[dq.back()] <= a[i] 即可
```

## 6. 去重 + 离散化

```cpp
// 离散化：把很大的值（如 1e9）压成 1..m 的小“排名”，m = 不同值个数
vector<int> a = {1000000000, 5, 5, 7, 1000000000};    // 样例原数组
int x = 7;                 // 样例值

vector<int> v = a;                    // 复制一份出来排
sort(v.begin(), v.end());             // 升序
v.erase(unique(v.begin(), v.end()), v.end());   // 去重：v 现在装着所有不同值（升序）
int m = (int)v.size();                // 不同值个数

// 某个值 x 的排名（从 1 开始）：
int rank_x = lower_bound(v.begin(), v.end(), x) - v.begin() + 1;
// 把整个数组原地替换成排名（之后每个 a[i] ∈ [1, m]）：
for (int &val : a)
    val = lower_bound(v.begin(), v.end(), val) - v.begin() + 1;
// 排名还原成原值：v[rank - 1]
```

## 7. 二分：lower_bound / upper_bound + 二分答案

```cpp
// lower_bound / upper_bound 要求数组已升序
vector<int> a = {1, 2, 2, 3, 5};            // 样例
int x = 2;                 // 样例值

auto it = lower_bound(a.begin(), a.end(), x);   // 第一个 >= x 的位置
auto it2 = upper_bound(a.begin(), a.end(), x);  // 第一个 >  x 的位置
int pos = it - a.begin();                 // 转成下标（0 开始）
int cnt_x = (int)(it2 - it);              // 等于 x 的个数 = 上界减下界
// 查不到时迭代器 == a.end()
// 数组版（下标 1..n）：pos = lower_bound(a + 1, a + n + 1, x) - a;

// —— 二分答案模板：判定 ok(mid) 随 mid 单调，求“最大的可行 mid” ——
auto ok = [&](int mid) { return 1LL * mid * mid <= 100; };   // 示例判定，换成题目的
int l = 0, r = 100, ans = -1;             // 答案一定落在 [l, r] 内（按题改范围）
while (l <= r)
{
    int mid = (l + r) / 2;
    if (ok(mid)) ans = mid, l = mid + 1;  // mid 可行：记下 ans，再往更大试
    else r = mid - 1;                     // 不可行：只能往更小试
}
// 求“最小的可行 mid”：把可行分支改成记录后 r = mid - 1，不可行分支 l = mid + 1
```

## 8. string 常用操作

```cpp
string s = "abcabc";                       // 样例串

int p = (int)s.find("bc");                 // 子串第一次出现的位置；没找到返回 string::npos
if (p == (int)string::npos) { /* 没找到 */ }
string t = s.substr(0, 3);                 // 从位置 0 开始取 3 个字符（越界自动截到末尾）
string tail = s.substr(2);                 // 从位置 2 取到末尾
s += "def";                                // 末尾拼接
reverse(s.begin(), s.end());               // 原地反转
int len = (int)s.length();                 // 长度

// 数字 <-> 字符串
string num = to_string(12345);             // 数字转字符串
long long v = stoll(num);                  // 字符串转 long long（int 用 stoi）
char c = 'A';
string one = string(1, c);                 // 单个字符转成字符串

// 按空白拆一串数：先放一行里再拆
string line = "10 20 30";
stringstream ss(line);                     // 需要 <sstream>（bits 已带）
int val;
vector<int> nums;
while (ss >> val) nums.push_back(val);

// 读一整行：getline(cin, line);
// 注意：前面用过 cin >> 时，要先 cin.ignore() 再 getline，否则 getline 读到残留的空行
```

## 9. pair / tuple / sort：打包排序与取值

```cpp
// pair：两个值打包，自带字典序（first 优先、再 second），排序直接用
vector<pair<int, int>> a;
a.push_back({3, 1});
sort(a.begin(), a.end());                  // first 升序，相同再按 second 升序
auto [u, v] = a[0];                        // 解包取值（C++17）：u = a[0].first，v = a[0].second

// 自定义排序规则：lambda 里写“谁该排前面”，返回 true = 第一个参数排前面
sort(a.begin(), a.end(), [](const pair<int, int> &x, const pair<int, int> &y)
{
    if (x.first != y.first) return x.first < y.first;   // first 升序
    return x.second > y.second;                         // first 相同：second 降序
});

// tuple：3 个及以上打包，排序同样默认字典序（先比第 0 维）
vector<tuple<int, int, int>> t;
t.push_back({1, 2, 3});
sort(t.begin(), t.end());
int third = get<2>(t[0]);                  // get<下标> 取值

// 取最大 / 最小元素：max_element 返回迭代器
auto it = max_element(a.begin(), a.end());
int mx = it->first;

// 结构体按多个成员比较的最省事写法（按成员 a 升序、b 降序举例）：
// sort(v.begin(), v.end(), [](const Node &x, const Node &y)
// { return tie(x.a, x.b) > tie(y.a, y.b); });    // 用 tie 拼成可比的引用元组
```

## 10. 全排列枚举 / 第 k 小 / 打乱 / 循环移位

```cpp
// 枚举全排列：先升序排好，再 do-while 遍历
vector<int> p = {1, 2, 3};
sort(p.begin(), p.end());
do
{
    // 这里处理当前排列 p
} while (next_permutation(p.begin(), p.end()));   // 到最大排列时返回 false，循环结束
// 上一个排列用 prev_permutation（初始要降序）

// 找第 k 小：nth_element 是 O(n)，只保证 a[k] 就位，其他部分乱序
vector<int> a = {9, 3, 7, 1, 5};           // 样例
int k = 2;                 // k 从 0 开始，0 <= k < 数组长度
nth_element(a.begin(), a.begin() + k, a.end());
int kth = a[k];                           // 现在 a[k] 就是第 k+1 小

// 随机打乱 / 循环移位
mt19937 rng((unsigned)chrono::steady_clock::now().time_since_epoch().count());
shuffle(a.begin(), a.end(), rng);         // 随机打乱
rotate(a.begin(), a.begin() + (2 % (int)a.size()), a.end());   // 循环左移 2 位
```

## 11. accumulate / iota / gcd / lcm 等函数速查

```cpp
vector<int> a = {1, 2, 3, 4, 5};           // 样例

int sum = accumulate(a.begin(), a.end(), 0);   // 求和（int 会爆就写 0LL）
int mx = *max_element(a.begin(), a.end());     // 最大值（min_element 同理）
int cnt = count(a.begin(), a.end(), 3);        // 值等于 3 的元素个数
iota(a.begin(), a.end(), 1);                   // 原地填成 1 2 3 4 5（第三参是起始值）
partial_sum(a.begin(), a.end(), a.begin());    // 原地变成前缀和
fill(a.begin(), a.end(), 0);                   // 全部填 0

int g = gcd(12, 18);                           // 最大公约数 = 6（C++17；负数先 abs）
int l = lcm(12, 18);                           // 最小公倍数 = 36（C++17）
int c = clamp(7, 1, 5);                        // 把 7 夹到 [1,5]，结果 5（C++17）
int big = max({1, 5, 3});                      // 多个数一起取最大
```

---

## 附录 A：防卡哈希定义（粘贴一次）

第 1 节末尾提到防卡写法，这里是一次性定义。用时把下面整段复制到代码开头。

```cpp
// unordered_map / unordered_set 专用防卡哈希：按你代码里的头文件粘贴一次即可
struct custom_hash
{
    static uint64_t splitmix64(uint64_t x)
    {
        x += 0x9e3779b97f4a7c15ULL;
        x = (x ^ (x >> 30)) * 0xbf58476d1ce4e5b9ULL;
        x = (x ^ (x >> 27)) * 0x94d049bb133111ebULL;
        return x ^ (x >> 31);
    }
    size_t operator()(uint64_t x) const
    {
        static const uint64_t FIXED_RANDOM = chrono::steady_clock::now().time_since_epoch().count();
        return splitmix64(x + FIXED_RANDOM);
    }
};

// 键是 pair<int, int> 时再用下面这个：
struct pair_hash
{
    size_t operator()(const pair<int, int> &p) const
    {
        static const uint64_t FIXED_RANDOM = chrono::steady_clock::now().time_since_epoch().count();
        uint64_t h = ((uint64_t)p.first << 32) | (uint32_t)p.second;
        return custom_hash::splitmix64(h + FIXED_RANDOM);
    }
};
// 粘完这样声明即可，其余用法和普通 unordered_map 完全一样：
// unordered_map<ll, int, custom_hash> mp;
// unordered_map<pair<int, int>, int, pair_hash> mp2;
// unordered_set<ll, custom_hash> hs;
```
