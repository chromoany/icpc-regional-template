# ICPC 区域赛算法模板

[![PDF 更新](https://img.shields.io/github/release-date/chromoany/icpc-regional-template?style=flat-square&label=PDF%20%E6%9B%B4%E6%96%B0&color=2ea44f)](https://github.com/chromoany/icpc-regional-template/releases/latest)
[![下载量](https://img.shields.io/github/downloads/chromoany/icpc-regional-template/total?style=flat-square&label=%E4%B8%8B%E8%BD%BD%E9%87%8F)](https://github.com/chromoany/icpc-regional-template/releases)
[![许可](https://img.shields.io/badge/%E8%AE%B8%E5%8F%AF-CC0%201.0-lightgrey?style=flat-square)](LICENSE)
[![问题反馈](https://img.shields.io/github/issues/chromoany/icpc-regional-template?style=flat-square&label=%E9%97%AE%E9%A2%98%E5%8F%8D%E9%A6%88)](https://github.com/chromoany/icpc-regional-template/issues)

个人整理的 ICPC 区域赛算法模板库：`算法模板/` 收录 **155 个可直接编译使用的完整模板**，`trick合集.md` + `trick库/` 收录 **176 条竞赛技巧速查**，供赛前复习与离线查阅。

## 下载 PDF

不想 clone 仓库、只想打印一份带走的话，直接下这两个文件 —— 由 [folio](https://github.com/chromoany/folio) 排版，A4 幅面，目录页码是排版后的真实页码、可点击跳转：

| 文件 | 内容 | 页数 | 大小 |
|---|---|---|---|
| [**`icpc-template.pdf`**](https://github.com/chromoany/icpc-regional-template/releases/latest/download/icpc-template.pdf) | `算法模板/` 全部 11 个专题（快读 → 公式结论速查） | 205 | 5.2 MB |
| [**`icpc-tricks.pdf`**](https://github.com/chromoany/icpc-regional-template/releases/latest/download/icpc-tricks.pdf) | `trick库/` 六个分册（数学 / 图论 / 动态规划 / 数据结构 / 字符串 / 其他） | 65 | 1.8 MB |

上表链接恒指向**最新版**：仓库只保留当前一版 PDF，不存档历史版本，排版更新直接覆盖。版本日期与排版参数见 [Releases](https://github.com/chromoany/icpc-regional-template/releases) 页面。

## 目录结构

### `算法模板/` · 按专题分文件

入口是 **[`算法模板/00-索引.md`](算法模板/00-索引.md)**，含完整模板清单与「合并注意」（多模板拼进同一份代码时的全局变量重名表）。

| 文件 | 专题 |
|---|---|
| `01-基础工具.md` | 快读 read / readll / 128 位 IO / 常用宏与常量 / 快写 |
| `02-数据结构.md` | 树状数组 / 线段树 / 并查集 / 带权并查集 / 可撤销并查集 / ST表 / 主席树 / 莫队 / 分块 / FHQ-Treap / 李超树 / CDQ分治 / 整体二分 / 线段树合并 / 左偏树 / 笛卡尔树 / DSU on tree / 珂朵莉树ODT / LCT / 单调栈最大矩形 / 悬线法 / 线段树分治 |
| `03-图论.md` | 最短路 / 次短路 / 0-1 BFS / 同余最短路 / MST / LCA / 树上差分 / 树链剖分 / Tarjan / 网络流 / 二分图 / 拓扑 / 差分约束 / 2-SAT / KM / 最小树形图 / 欧拉路 / 上下界流 / 最小路径覆盖 / 点分治 / 带花树 / 重心与基环树 / 边双缩点 / 点双缩点(圆方树) / 虚树 / Stoer-Wagner 全局最小割 / 最大团 |
| `04-数论.md` | 快速幂 / 线性筛 / 扩欧 / 逆元 / CRT / 组合数 / Lucas / 数论分块 / FFT / NTT / FWT / 多项式求逆·ln·exp / 矩阵快速幂 / 高斯消元 / 线性基 / 莫比乌斯反演 / exLucas / BSGS / 原根 / Pólya / Simpson / 拉格朗日插值 / Prüfer / 矩阵树定理 / 杜教筛 / 类欧几里得 / Cipolla |
| `05-字符串.md` | 字符串哈希 / KMP / AC自动机 / Manacher / Trie / 后缀数组 / Z函数（扩展KMP） / 后缀自动机 / 最小表示法 / 回文自动机PAM / 广义SAM / Lyndon 分解 |
| `06-计算几何.md` | 基础运算 / 凸包 / 多边形 / 刺穿直线 / 位置关系与距离 / 旋转卡壳 / 半平面交 / 扫描线面积并 / 圆交与外接圆 / Pick定理 / 圆与多边形交面积 |
| `07-动态规划.md` | 记忆化搜索 / LIS / LCS / 背包 / 区间DP / 树形DP / 数位DP / 斜率优化 / 状压 / 单调队列优化 / WQS二分 / 期望DP / 轮廓线DP / SOS DP / 斯坦纳树 / 0-1分数规划 / 决策单调性分治 / Slope Trick |
| `08-杂项.md` | SG函数与博弈 / 三分 / bitset / 德州扑克比牌 / 高精度 / 表达式求值 / 日期时间 / 对拍与造数据（三步上手，Win/Linux 双平台）/ 赛场编译调试命令 / 交互题 |
| `09-常用STL.md` | 注释式用法速查：哈希 / 堆与对顶堆 / multiset / 单调队列 / 离散化 / 二分 / string / tuple排序 / 排列与第k小 / numeric 工具 / pbds（有序统计树、gp_hash_table、配对堆） |
| `10-随机化.md` | 随机数工具 / 随机打乱 / 最小圆覆盖（随机增量） / 随机权值哈希 / Schwartz–Zippel 与 Freivalds / 模拟退火 / 爬山法与 2-opt / Miller–Rabin / Pollard–Rho |
| `11-公式结论速查.md` | 公式结论速查（纯正文）：组合恒等式 / Catalan / 斯特林数 / 二项式反演与 Min-Max 容斥 / Hall 定理 / 常用数列 / 数论与图论结论 / 期望概率 / 博弈结论 / 字符串结论 / 数据范围与复杂度对照与 NTT 模数原根表 |

### `trick合集.md` + `trick库/` · trick 速查（共 176 条）

[`trick合集.md`](trick合集.md) 是**总索引**：含全部条目一览（标题 / 词条 / 关键词）与收录口径。
条目本身按词条分册存放在 [`trick库/`](trick库/)：

| 文件 | 主题 | 条数 |
|---|---|---|
| `01-数学.md` | 数学 | 43 |
| `02-图论.md` | 图论 | 34 |
| `03-动态规划.md` | 动态规划 | 35 |
| `04-数据结构.md` | 数据结构 | 14 |
| `05-字符串.md` | 字符串 | 4 |
| `07-其他.md` | 其他（含计算几何相关条目） | 46 |

条目结构统一为：标题（描述技巧本身）/ 核心思路与结论 / 使用条件 / 易错点，均为便于离线查阅的纯结论形式，不含例题与题目链接。

其中部分条目整理自 [AlgoWiki](https://www.algowiki.cn/) 的公开技巧投稿，原作者信息见该站对应条目页。

## 怎么用

- 找算法：先查 [`00-索引.md`](算法模板/00-索引.md) 的「模板总目录」，再在对应专题文件里搜关键词；库内没有的按模板「变体」一节的指引自行补。
- 每个模板是独立章节：适用模型 / 复杂度 / 代码 / 使用说明 / 变体。代码块自带所需头文件，**复制后补 `main` 即可提交**。
- 把多个模板拼进同一份代码前，先看索引末尾的「合并注意」——那里列了全局变量重名表。
- 代码风格：纯净代码无注释（`09-常用STL`、`06-计算几何` 两个速查文件例外）、全局变量 + 静态数组、大括号换行、传参传下标不传数组、快读 `read()/readll()`。
- 直接打印：见上面的 [下载 PDF](#下载-pdf)，每份含目录与页码。

## 验证

- 全部完整模板（155 个）通过 `g++ -std=c++17 -O2 -fsyntax-only` 语法检查，可自行复现：把代码块存为 `.cpp` 后运行同一条命令。
- [`00-索引.md`](算法模板/00-索引.md) 末尾按批次列出了各模板的验证状态——做过数值对拍的标注对拍方式与结果，其余为「仅编译验证」，首次使用前建议先跑一遍样例。
- 排版约束：代码单行控制在 100 字符宽以内（中文按 2 字符计），超出在转 A4 PDF 时会被裁掉且编译不报错，详见索引开头的说明。

## 许可

除上述第三方整理内容外，本仓库内容以 **CC0 1.0 Universal** 释出，全文见 [`LICENSE`](LICENSE)：在最大范围内放弃著作权与相关权利，可以任意复制、修改、分发、商用，**无需署名**。

## 反馈

发现问题或有想补的模板，欢迎提交 [Issue](https://github.com/chromoany/icpc-regional-template/issues) 或 PR。本仓库只收知识点本身，不收题解与题目链接。

---

维护：[@chromoany](https://github.com/chromoany)　最后更新：2026-09-26
