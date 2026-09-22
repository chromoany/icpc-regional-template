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

### 8a. 构造、长度、拼接、比较

```cpp
string s = "abcabc";                       // 直接初始化
string t(5, 'x');                          // 5 个 'x'："xxxxx"
string u = s + "def";                      // 拼接（生成新串）
s += "def";                                // 原地拼接（推荐，不产生临时对象）
s.append(3, '!');                          // 末尾接 3 个 '!'
s.push_back('Z');                          // 末尾加一个字符
s.pop_back();                              // 删掉最后一个字符

int len = (int)s.size();                   // 长度（size() 与 length() 等价）
bool emp = s.empty();                      // 是否为空（比 s.size() == 0 快）
s.clear();                                 // 清空
s.resize(3);                               // 截断到长度 3（变长时补 '\0'）
s.reserve(1000);                           // 预留容量：大量 += 前调一次，少反复扩容

// 比较：== != < > 都是字典序，逐字符比 ASCII（"ab" < "b"、"ab" < "abc"）
bool same = (s == t);
int c = s.compare(t);                      // >0 / =0 / <0
```

### 8b. 查找（find 家族）

```cpp
string s = "abcabc", sub = "bc";

int p = (int)s.find(sub);                  // 第一次出现的位置，找不到返回 string::npos
int p2 = (int)s.find(sub, 2);              // 从下标 2 开始往后找
int r = (int)s.rfind(sub);                 // 最后一次出现的位置（从后往前找）

// 判断“存在 / 不存在”就这么写，别拿 int 和 -1 直接比
if (s.find(sub) == string::npos) { /* 不存在 */ }

// 字符集版本的查找：参数是"字符集合"，不是子串
int f1 = (int)s.find_first_of("xyz");       // 第一个属于 "xyz" 的字符位置
int f2 = (int)s.find_first_not_of("abc");   // 第一个不属于 "abc" 的字符位置
int f3 = (int)s.find_last_of("abc");        // 最后一个属于 "abc" 的字符位置

// 找出 sub 的所有出现位置（经典写法：pos 从上一个位置 +1 继续）
for (size_t pos = s.find(sub); pos != string::npos; pos = s.find(sub, pos + 1))
{
    // pos 是这次匹配的起点
}
// 上面这种写法允许重叠（"aaa" 里找 "aa" 会得到 2 处）；
// 要不重叠匹配，把 pos + 1 改成 pos + sub.size()
```

### 8c. 取子串、替换、删除、插入

```cpp
string s = "hello world";

string a = s.substr(0, 5);                 // 从下标 0 起取 5 个 → "hello"
string b = s.substr(6);                    // 从下标 6 取到末尾 → "world"
string c = s.substr(6, 100);               // 长度超界自动截到末尾（不报错、不抛异常）

s.replace(0, 5, "hi");                     // 把 [0,5) 这段换成 "hi" → "hi world"
s.insert(2, "XX");                         // 在下标 2 前插入 "XX"
s.insert(2, 3, 'X');                       // 在下标 2 前插入 3 个 'X'
s.erase(2, 3);                             // 删除从下标 2 开始的 3 个字符
s.erase(s.begin() + 2);                    // 删除单个字符（迭代器版）
s.erase(s.begin() + 1, s.begin() + 4);     // 删除 [1,4) 区间

// 相邻去重（配合 unique，用前先 sort）
sort(s.begin(), s.end());
s.erase(unique(s.begin(), s.end()), s.end());
```

### 8d. 数字与字符串互转

```cpp
string num = to_string(12345);             // 整数转字符串
string dnum = to_string(3.14);             // 浮点转字符串会输出 6 位小数："3.140000"
                                           // 要别的精度用 snprintf / ostringstream 控制
int a = stoi("123");                       // 字符串转 int
long long b = stoll("123456789012");        // 转 long long
double d = stod("3.14");                   // 转 double

// 坑：stoi("") / stoi("abc") 抛 std::invalid_argument；stoi("99999999999") 抛 std::out_of_range
//     保险写法：先判空与字符合法性，或整串 try 捕获

// 从串里依次取出所有整数（含负数）：stringstream 版，最省心
string t = "10 20 -30";
stringstream ss(t);
int v;
while (ss >> v) { /* 用 v */ }

// char 数组里循环取数：strtol 会自动把指针推过整个数字，不需要 substr
char buf[] = "10 20 -30";
char *p = buf;
while (*p)
{
    if (isdigit((unsigned char)*p) || (*p == '-' && isdigit((unsigned char)p[1])))
    {
        char *q;
        int x = (int)strtol(p, &q, 10);    // q 被推到数字末尾
        p = q;
        // 用 x
    }
    else p++;                              // 跳过非数字字符
}
```

### 8e. 字符判断与大小写

```cpp
char ch = 'a';
bool d1 = isdigit((unsigned char)ch);      // 是否数字 '0'~'9'
bool d2 = isalpha((unsigned char)ch);      // 是否字母
bool d3 = isupper((unsigned char)ch);      // 是否大写
bool d4 = isspace((unsigned char)ch);      // 是否空白（空格 / \t / \n）
char lo = (char)tolower((unsigned char)ch); // 转小写
char hi = (char)toupper((unsigned char)ch); // 转大写
// 注意：这些函数参数是 int，传 char 一定要 (unsigned char) 强转，
//       否则负值字符（中文 / 扩展 ASCII）在部分平台会越界，行为未定义

// 整串转小写 / 大写：必须用 lambda 包一层，否则 ::tolower 重载会冲突
string s = "AbC1";
transform(s.begin(), s.end(), s.begin(), [](char c) { return (char)tolower((unsigned char)c); });
// 转大写把 tolower 换成 toupper

// 计数 / 逐个字符扫
int cnt = (int)count(s.begin(), s.end(), 'a');
for (char c : s) if (isdigit((unsigned char)c)) { /* ... */ }
```

### 8f. 分割、拼接、前后缀

```cpp
// 按分隔符切分（单字符分隔符）：getline 带第三参
string line = "a,b,,c";
vector<string> parts;
stringstream ss(line);
string item;
while (getline(ss, item, ',')) parts.push_back(item);
// 注意：连续分隔符会产生空串（这里能切出 4 段，第 3 段是空的），按需过滤
// for (auto &x : parts) if (!x.empty()) { ... }

// 拼回去（join）
string joined;
for (auto &x : parts)
{
    if (!joined.empty()) joined += ',';
    joined += x;
}

// 判断前缀 / 后缀（C++17 没有 starts_with，C++20 才有，这样写最稳）
string s = "abcdef", pre = "abc", suf = "def";
bool has_pre = (s.size() >= pre.size() && s.compare(0, pre.size(), pre) == 0);
bool has_suf = (s.size() >= suf.size() && s.compare(s.size() - suf.size(), suf.size(), suf) == 0);

// 与 char 数组互转
const char *cp = s.c_str();                // string → const char*（只读，别改）
char arr[] = "hello";
string fromArr = arr;                      // char[] → string
```

### 8g. 读入（cin / getline 的空行坑）

```cpp
string s;
cin >> s;                                  // 读到空白（空格 / 换行）为止，不含空白
getline(cin, s);                           // 读一整行（含空格，不含行尾换行）

// 坑：cin >> 之后紧接 getline 会读到残留的换行，必须先吃掉
int n;
cin >> n;
cin.ignore();                              // 丢掉残留的换行（也可 cin.ignore(1, '\n')）
string line;
getline(cin, line);

// 一直读到 EOF
while (getline(cin, line)) { /* 处理 line */ }

// 行首可能有空白时：getline(cin >> ws, line)，ws 会先吞掉前导空白
```

### 8h. string_view（C++17，只读且零拷贝）

```cpp
// 只做比较 / 切片 / 查找、不改内容时用它，避免大量子串拷贝（长串高频操作时提速明显）
string s = "hello world";
string_view sv(s);
string_view mid = sv.substr(6, 5);         // 不产生任何字符拷贝
size_t p = sv.find("world");               // 接口和 string 基本一致
cout << mid << '\n';                       // 输出 world
// 注意：string_view 不拥有内存，原 string 一旦销毁或重新分配，view 立即失效
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

## 12. pbds 平板电视：有序统计树 / 哈希表 / 可并堆

GNU 扩展库（`__gnu_pbds`），**g++ 自带，主流竞赛环境都可用**。头文件写 `#include <bits/extc++.h>` 就一次包含全部（同时也包含 `bits/stdc++.h`），比 `bits/stdc++.h` + 三个 pbds 头省事。

```cpp
#include <bits/extc++.h>
using namespace std;
using namespace __gnu_pbds;      // pbds 的东西都在这个命名空间里

// —— ① tree：有序统计树（平衡树），支持排名与第 k 小，比手写 FHQ 省事 ——
// 模板参数：<键类型, 映射值类型（做 set 写 null_type）, 比较器, 底层树, 更新节点大小的策略>
tree<int, null_type, less<int>, rb_tree_tag, tree_order_statistics_node_update> t;   // 当 set 用
// 当 multiset 用（键里再塞一个编号就不会被去重）
tree<pair<int, int>, null_type, less<pair<int, int>>,
     rb_tree_tag, tree_order_statistics_node_update> ms;
tree<int, long long, less<int>, rb_tree_tag, tree_order_statistics_node_update> mp2;   // 当 map 用

// —— ② gp_hash_table：比 unordered_map 更快的哈希表 ——
// 默认哈希会被 hack，正式比赛加 custom_hash
// 防卡写法：gp_hash_table<ll, int, custom_hash> gmp;（custom_hash 见附录 A）
gp_hash_table<long long, int> gmp;

// —— ③ 配对堆：比 std::priority_queue 多了 modify / join / erase，代价是常数略大 ——
__gnu_pbds::priority_queue<int, greater<int>, pairing_heap_tag> heap;   // 小根；要大根换成 less
__gnu_pbds::priority_queue<int, greater<int>, pairing_heap_tag>::point_iterator it;   // modify 用

int main()
{
    t.insert(1);                     // 插入：重复插入无效（和 set 一样）
    t.insert(5);
    t.insert(3);
    t.erase(5);                      // 按键删除
    int r = t.order_of_key(3);       // 排名：严格小于 3 的元素个数（0 基，这里 = 1）
    int kth = *t.find_by_order(0);   // 第 0 小（0 基），越界返回 t.end()，用前先判 != t.end()
    int cnt = t.size();              // 元素个数

    gmp[123] = 7;                    // 用法与 unordered_map 完全一致
    if (gmp.find(123) != gmp.end())  // 查找；遍历 / erase 也一样
    {
        int v = gmp[123];
    }

    heap.push(4);                    // push / top / pop 与 std::priority_queue 相同
    heap.push(2);
    heap.push(9);
    int big = heap.top();            // 堆顶 = 2（小根）
    heap.pop();                      // 弹出堆顶
    it = heap.push(5);               // push 返回该元素的迭代器
    heap.modify(it, 0);              // 把该元素改成 0
}
```

使用说明：
- **tree 没有内置的"第 k 大"**：第 k 大 = `*t.find_by_order(t.size() - k)`；前驱 / 后继用 `order_of_key` 转化：后继 = `*t.find_by_order(t.order_of_key(x))`（若 `x` 本身在树里则是 `order_of_key(x)+1`）。
- **tree 的复杂度**是 $O(\log n)$ 每次操作，常数比手写 treap 大；只在"要排名 / 第 k 小又不想写平衡树"时用。底层 `rb_tree_tag` 通用，`splay_tree_tag` 常数更小但更易退化。
- **gp_hash_table 的迭代顺序不稳定**，不要依赖遍历顺序；`.resize(n)` 预分配能再快一截。
- **配对堆 / `__gnu_pbds::priority_queue` 与 `std::priority_queue` 接口不同**：没有 `clear()`（用 `heap = decltype(heap)();` 清空）、`size()`/`empty()` 有；`join` 可 $O(1)$ 合并两个堆（`a.join(b)` 后 b 被掏空），Dijkstra 的"可并堆优化"和"需要修改堆中元素"的题用它。
- 只需 pbds 单个组件时也可分别 include：`<ext/pb_ds/assoc_container.hpp>` + `<ext/pb_ds/tree_policy.hpp>`（tree）、`<ext/pb_ds/priority_queue.hpp>`（堆）。
- **注意**：pbds 不属于 C++ 标准，判题机若是 clang / MSVC 可能没有；ICPC 与国内 OJ 的 g++ 环境都有。
