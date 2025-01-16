## 汉诺塔

> 给定三根柱子，记为 `A`、`B` 和 `C` 。起始状态下，柱子 `A` 上套着 n 个圆盘，它们从上到下按照从小到大的顺序排列。我们的任务是要把这 n 个圆盘移到柱子 `C` 上，并保持它们的原有顺序不变（如图 12-10 所示）。在移动圆盘的过程中，需要遵守以下规则。
>
> 1. 圆盘只能从一根柱子顶部拿出，从另一根柱子顶部放入。
> 2. 每次只能移动一个圆盘。
> 3. 小圆盘必须时刻位于大圆盘之上。
>
> **我们将规模为 i 的汉诺塔问题记作 f(i)** 

从本质上看，**我们将问题 f(3) 划分为两个子问题 f(2) 和一个子问题 f(1)** 。按顺序解决这三个子问题之后，原问题随之得到解决。这说明子问题是独立的，而且解可以合并。而向上推，汉诺塔问题的分治策略：将原问题 f(n) 划分为两个子问题 f(n−1) 和一个子问题 f(1) ，并按照以下顺序解决这三个子问题。

1. 将 n−1 个圆盘借助 `C` 从 `A` 移至 `B` 。
2. 将剩余 1 个圆盘从 `A` 直接移至 `C` 。
3. 将 n−1 个圆盘借助 `A` 从 `B` 移至 `C` 。

对于这两个子问题 f(n−1) ，**可以通过相同的方式进行递归划分**，直至达到最小子问题 f(1) 。而 f(1) 的解是已知的，只需一次移动操作即可。

```javascript
/* 移动一个圆盘 */
function move(src, tar) {
    // 从 src 顶部拿出一个圆盘
    const pan = src.pop();
    // 将圆盘放入 tar 顶部
    tar.push(pan);
}

/* 求解汉诺塔问题 f(i) */
function dfs(i, src, buf, tar) {
    // 若 src 只剩下一个圆盘，则直接将其移到 tar
    if (i === 1) {
        move(src, tar);
        return;
    }
    // 子问题 f(i-1) ：将 src 顶部 i-1 个圆盘借助 tar 移到 buf
    dfs(i - 1, src, tar, buf);
    // 子问题 f(1) ：将 src 剩余一个圆盘移到 tar
    move(src, tar);
    // 子问题 f(i-1) ：将 buf 顶部 i-1 个圆盘借助 src 移到 tar
    dfs(i - 1, buf, src, tar);
}

/* 求解汉诺塔问题 */
function solveHanota(A, B, C) {
    const n = A.length;
    // 将 A 顶部 n 个圆盘借助 B 移到 C
    dfs(n, A, B, C);
}
```



## 构建树

> 给定一棵二叉树的前序遍历 `preorder` 和中序遍历 `inorder` ，请从中构建二叉树，返回二叉树的根节点。假设二叉树中没有值重复的节点（如图所示）。

![tree-build](C:../img/tree-build.png)

根据定义，`preorder` 和 `inorder` 都可以划分为三个部分。

- 前序遍历：`[ 根节点 | 左子树 | 右子树 ]` ，例如图的树对应 `[ 3 | 9 | 2 1 7 ]` 。
- 中序遍历：`[ 左子树 | 根节点 ｜ 右子树 ]` ，例如图的树对应 `[ 9 | 3 | 1 2 7 ]` 。

根据以上划分方法，**我们已经得到根节点、左子树、右子树在 `preorder` 和 `inorder` 中的索引区间**。而为了描述这些索引区间，我们需要借助几个指针变量。

- 将当前树的根节点在 `preorder` 中的索引记为 i 。
- 将当前树的根节点在 `inorder` 中的索引记为 m 。
- 将当前树在 `inorder` 中的索引区间记为 [l,r] 。

如表 12-1 所示，通过以上变量即可表示根节点在 `preorder` 中的索引，以及子树在 `inorder` 中的索引区间。

表 12-1  根节点和子树在前序遍历和中序遍历中的索引

|        | 根节点在 `preorder` 中的索引 | 子树在 `inorder` 中的索引区间 |
| :----- | :--------------------------- | :---------------------------- |
| 当前树 | i                            | [l,r]                         |
| 左子树 | i+1                          | [l,m−1]                       |
| 右子树 | i+1+(m−l)                    | [m+1,r]                       |

请注意，右子树根节点索引中的 (m−l) 的含义是“左子树的节点数量”，建议结合图 12-7 理解。

[![根节点和左右子树的索引区间表示](https://www.hello-algo.com/chapter_divide_and_conquer/build_binary_tree_problem.assets/build_tree_division_pointers.png)](https://www.hello-algo.com/chapter_divide_and_conquer/build_binary_tree_problem.assets/build_tree_division_pointers.png)

图 12-7  根节点和左右子树的索引区间表示

### 4.  代码实现

为了提升查询 m 的效率，我们借助一个哈希表 `hmap` 来存储数组 `inorder` 中元素到索引的映射：

build_tree.js

```
/* 构建二叉树：分治 */
function dfs(preorder, inorderMap, i, l, r) {
    // 子树区间为空时终止
    if (r - l < 0) return null;
    // 初始化根节点
    const root = new TreeNode(preorder[i]);
    // 查询 m ，从而划分左右子树
    const m = inorderMap.get(preorder[i]);
    // 子问题：构建左子树
    root.left = dfs(preorder, inorderMap, i + 1, l, m - 1);
    // 子问题：构建右子树
    root.right = dfs(preorder, inorderMap, i + 1 + m - l, m + 1, r);
    // 返回根节点
    return root;
}

/* 构建二叉树 */
function buildTree(preorder, inorder) {
    // 初始化哈希表，存储 inorder 元素到索引的映射
    let inorderMap = new Map();
    for (let i = 0; i < inorder.length; i++) {
        inorderMap.set(inorder[i], i);
    }
    const root = dfs(preorder, inorderMap, 0, 0, inorder.length - 1);
    return root;
}
```

图 12-8 展示了构建二叉树的递归过程，各个节点是在向下“递”的过程中建立的，而各条边（引用）是在向上“归”的过程中建立的。

[![构建二叉树的递归过程](https://www.hello-algo.com/chapter_divide_and_conquer/build_binary_tree_problem.assets/built_tree_step1.png)](https://www.hello-algo.com/chapter_divide_and_conquer/build_binary_tree_problem.assets/built_tree_step1.png)

图 12-8  构建二叉树的递归过程

每个递归函数内的前序遍历 `preorder` 和中序遍历 `inorder` 的划分结果如图 12-9 所示。

[![每个递归函数中的划分结果](https://www.hello-algo.com/chapter_divide_and_conquer/build_binary_tree_problem.assets/built_tree_overall.png)](https://www.hello-algo.com/chapter_divide_and_conquer/build_binary_tree_problem.assets/built_tree_overall.png)

图 12-9  每个递归函数中的划分结果

设树的节点数量为 n ，初始化每一个节点（执行一个递归函数 `dfs()` ）使用 O(1) 时间。**因此总体时间复杂度为 O(n)** 。

哈希表存储 `inorder` 元素到索引的映射，空间复杂度为 O(n) 。在最差情况下，即二叉树退化为链表时，递归深度达到 n ，使用 O(n) 的栈帧空间。**因此总体空间复杂度为 O(n)** 。
