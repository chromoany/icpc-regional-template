# ICPC Regional Algorithm Templates

[**中文**](README.md) · English

[![PDF updated](https://img.shields.io/github/release-date/chromoany/icpc-regional-template?style=flat-square&label=PDF%20updated&color=2ea44f)](https://github.com/chromoany/icpc-regional-template/releases/latest)
[![License](https://img.shields.io/badge/license-CC0%201.0-lightgrey?style=flat-square)](LICENSE)
[![Issues](https://img.shields.io/github/issues/chromoany/icpc-regional-template?style=flat-square&label=issues)](https://github.com/chromoany/icpc-regional-template/issues)

A personal ICPC regional-contest library: `算法模板/` holds **155 ready-to-compile templates**, and `trick合集.md` + `trick库/` collect **176 contest tricks** — meant for pre-contest review and offline lookup (the printed PDFs are the intended way to read them during a contest).

> The template sources and the trick library are written in Chinese; this file only describes the repository. The PDFs are Chinese as well.

## Download the PDFs

If you would rather print a copy than clone the repo, grab these files — typeset with [folio](https://github.com/chromoany/folio), A4, with a clickable table of contents whose page numbers are the real typeset ones:

| File | Contents | Pages | Size |
|---|---|---|---|
| [**`icpc-template.pdf`**](https://github.com/chromoany/icpc-regional-template/releases/latest/download/icpc-template.pdf) | All 11 topics of `算法模板/` (fast IO → formula cheat sheet) | 205 | 5.2 MB |
| [**`icpc-tricks.pdf`**](https://github.com/chromoany/icpc-regional-template/releases/latest/download/icpc-tricks.pdf) | The six volumes of `trick库/` (math / graphs / DP / data structures / strings / other) | 65 | 1.8 MB |
| [**`icpc-merged.pdf`**](https://github.com/chromoany/icpc-regional-template/releases/latest/download/icpc-merged.pdf) | Everything above in one book: the 11 template topics + the six trick volumes | 289 | 7.1 MB |

Those links always point at the **latest** release: only the current version is kept, older ones are not archived. Typesetting parameters and the version date live on the [Releases](https://github.com/chromoany/icpc-regional-template/releases) page.

## Repository layout

### `算法模板/` — one file per topic

The entry point is **[`算法模板/00-索引.md`](算法模板/00-索引.md)**, which lists every template plus a "merging notes" section (global variable name clashes when you paste several templates into one file).

| File | Topic |
|---|---|
| `01-基础工具.md` | Fast IO, 128-bit IO, common macros and constants |
| `02-数据结构.md` | Fenwick tree, segment tree, DSU, sparse table, persistent segment tree, Mo's algorithm, sqrt decomposition, FHQ-Treap, Li Chao tree, CDQ divide & conquer, parallel binary search, segment tree merging, leftist heap, Cartesian tree, DSU on tree, ODT, LCT, monotonic stack rectangle, hanging-line method, segment tree divide & conquer |
| `03-图论.md` | Shortest paths, k-th shortest path, 0-1 BFS, congruence shortest path, MST, LCA, tree difference, HLD, Tarjan, network flow, bipartite matching, topological sort, difference constraints, 2-SAT, KM, minimum arborescence, Euler path, flows with lower bounds, minimum path cover, centroid decomposition, blossom, centroid & cactus, edge/vertex biconnected contraction, virtual tree, Stoer-Wagner, maximum clique |
| `04-数论.md` | Fast power, linear sieve, extended Euclid, modular inverse, CRT, binomials, Lucas, divisor-block summation, FFT/NTT/FWT, polynomial inverse·log·exp, matrix power, Gaussian elimination, linear basis, Möbius inversion, exLucas, BSGS, primitive roots, Pólya, Simpson, Lagrange interpolation, Prüfer, matrix-tree theorem, Du Jiao sieve, Euclidean-like recursion, Cipolla |
| `05-字符串.md` | String hashing, KMP, Aho-Corasick, Manacher, trie, suffix array, Z-function, suffix automaton, minimal representation, palindromic automaton, generalized SAM, Lyndon factorization |
| `06-计算几何.md` | Basic operations, convex hull, polygons, line piercing, positions and distances, rotating calipers, half-plane intersection, sweep-line area union, circle intersection and circumcircle, Pick's theorem, circle-polygon intersection area |
| `07-动态规划.md` | Memoized search, LIS, LCS, knapsack, interval DP, tree DP, digit DP, slope trick, bitmask DP, monotonic queue optimization, WQS binary search, expected DP, broken-profile DP, SOS DP, Steiner tree, 0-1 fractional programming, divide & conquer on monotone decisions, Slope Trick |
| `08-杂项.md` | Sprague-Grundy games, ternary search, bitset, Texas hold'em comparison, big integers, expression evaluation, date and time, stress testing and data generation (Windows/Linux), in-contest compile & debug commands, interactive problems |
| `09-常用STL.md` | Usage cheat sheet: hash containers, heaps, multiset, monotonic queue, coordinate compression, binary search, `string`, tuple sorting, permutations and k-th element, `<numeric>`, pbds (ordered statistics tree, `gp_hash_table`, pairing heap) |
| `10-随机化.md` | Random utilities, shuffling, minimum enclosing circle, random-weight hashing, Schwartz–Zippel and Freivalds, simulated annealing, hill climbing and 2-opt, Miller–Rabin, Pollard–Rho |
| `11-公式结论速查.md` | Formula and conclusion cheat sheet: combinatorial identities, Catalan, Stirling numbers, binomial inversion and Min-Max inclusion-exclusion, Hall's theorem, common sequences, number theory and graph conclusions, expectation and probability, game conclusions, string conclusions, complexity reference and NTT primitive-root table |

### `trick合集.md` + `trick库/` — trick quick reference (176 entries)

[`trick合集.md`](trick合集.md) is the **index**: every entry's title, volume and keywords, plus the inclusion policy. The entries themselves live in [`trick库/`](trick库/), split by volume:

| File | Topic | Entries |
|---|---|---|
| `01-数学.md` | Math | 43 |
| `02-图论.md` | Graphs | 34 |
| `03-动态规划.md` | DP | 35 |
| `04-数据结构.md` | Data structures | 14 |
| `05-字符串.md` | Strings | 4 |
| `07-其他.md` | Other (including computational geometry) | 46 |

Every entry follows the same shape: title / core idea and conclusion / conditions of use / pitfalls. They are plain conclusions meant for offline lookup — no sample problems, no problem links.

Some entries were reorganised from public submissions on [AlgoWiki](https://www.algowiki.cn/); original authors are credited on that site.

## How to use it

- Looking for an algorithm: check the template index in [`00-索引.md`](算法模板/00-索引.md) first, then search the matching topic file. If the template you need is missing, the "variants" section of a nearby template usually tells you how to adapt one.
- Every template is its own section: applicable model / complexity / code / usage notes / variants. Each code block carries the headers it needs — **paste it, add `main`, submit**.
- Read the "merging notes" at the end of the index before pasting several templates into one file; that is where the global variable name clashes are listed.
- Code style: plain competitive code without comments (`09-常用STL` and `06-计算几何` are reference sheets and are the exception), global variables with static arrays, braces on their own line, arguments passed as indices rather than arrays, fast IO via `read()/readll()`.
- To print a copy: see [Download the PDFs](#download-the-pdfs); each book ships with a table of contents and page numbers.

## Verification

- All 155 complete templates pass `g++ -std=c++17 -O2 -fsyntax-only`. You can reproduce it by saving a code block as a `.cpp` and running the same command.
- The end of [`00-索引.md`](算法模板/00-索引.md) lists the verification status of each template by batch — those that were cross-checked against a brute force say how and with what result, the rest are "compile-checked only". Run a sample before using them for the first time.
- Typesetting constraint: keep code lines within 100 characters (count a CJK character as 2). Longer lines get clipped when converting to A4 PDF, and the compiler says nothing about it — see the note at the top of the index.

## License

Except for the third-party material noted above, the contents of this repository are released under **CC0 1.0 Universal** — see [`LICENSE`](LICENSE). To the fullest extent permitted by law, copyright and related rights are waived: copy, modify, distribute and use commercially, **no attribution required**.

## Feedback

Issues and pull requests are welcome if you spot a mistake or want to add a template. This repository only collects knowledge itself — no editorials, no problem links.

---

Maintained by [@chromoany](https://github.com/chromoany) · Last updated: 2026-09-28
