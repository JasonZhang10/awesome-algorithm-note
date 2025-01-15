# awesome-algorithm-note
常用算法：穷举法、回溯法、分治递归、贪心法、动态规划

穷举：也就是枚举法，都一一列出来

回溯：按照某一顺序进行检验，当当前候选不满足条件时，回退到上一步重新选择下一候选

分治：自顶向下的设计，将一个大问题分割若干子问题，各个击破，分而治之，然后子问题解集合起来得出原解

贪心：寄希望于局部最优解得出全局最优解

动态规划：分解的子问题不是互相独立的，保存一个表记录所有已解决的子问题的答案

## 复杂度分析

![img](./img/time.png)

<img src="./img/time-graph.png" alt="img" style="zoom:80%;" />

O(1): 操作数量与输入数据大小 n 无关

O(n): 单层循环

O(n<sup>2</sup>): 嵌套循环

O(2<sup>n</sup>): 二叉树、递归、细胞分裂；指数阶增长非常迅速，在穷举法（暴力搜索、回溯等）中常见。对于数据较大的问题是不可接受的，通常需要使用动态规划或贪心算法等来解决。

O(log n): 分治、递归、每轮缩减一半

O(nlog n): 两层循环，一层n，一层log n

O(n!): 递归，全排列 n! = n * (n-1) * ... * 2 * 1

## 数据结构

- **线性数据结构**：数组、链表、栈、队列、哈希表，元素之间是一对一的顺序关系。
- **非线性数据结构**：树、堆、图、哈希表。

![structure](./img/structure.png)

### 基本数据类型

- 整数类型 `byte`、`short`、`int`、`long` 。
- 浮点数类型 `float`、`double` ，用于表示小数。
- 字符类型 `char` ，用于表示各种语言的字母、标点符号甚至表情符号等。
- 布尔类型 `bool` ，用于表示“是”与“否”判断。

### 绪论

- 原码、反码和补码是在计算机中编码数字的三种方法，它们之间可以相互转换。整数的原码的最高位是符号位，其余位是数字的值。
- 整数在计算机中是以补码的形式存储的。在补码表示下，计算机可以对正数和负数的加法一视同仁，不需要为减法操作单独设计特殊的硬件电路，并且不存在正负零歧义的问题。
- 浮点数的编码由 1 位符号位、8 位指数位和 23 位分数位构成。由于存在指数位，因此浮点数的取值范围远大于整数，代价是牺牲了精度。
- ASCII 码是最早出现的英文字符集，长度为 1 字节，共收录 127 个字符。GBK 字符集是常用的中文字符集，共收录两万多个汉字。Unicode 致力于提供一个完整的字符集标准，收录世界上各种语言的字符，从而解决由于字符编码方法不一致而导致的乱码问题。
- UTF-8 是最受欢迎的 Unicode 编码方法，通用性非常好。它是一种变长的编码方法，具有很好的扩展性，有效提升了存储空间的使用效率。UTF-16 和 UTF-32 是等长的编码方法。在编码中文时，UTF-16 占用的空间比 UTF-8 更小。Java 和 C# 等编程语言默认使用 UTF-16 编码。

## 数组与链表

### 数组

普通算法中的数组（array）是一种线性数据结构，其将相同类型的元素存储在连续的内存空间中，长度固定。

```typescript
/* 初始化数组 */
let arr: number[] = new Array(5).fill(0);
let nums: number[] = [1, 3, 2, 5, 4];
/* 访问数组 */
nums[index] //复杂度O(1)
```

| 操作     | 方法                                           | 示例                              |
| :------- | :--------------------------------------------- | :-------------------------------- |
| **插入** | `push()`                                       | `arr.push(4)` 数组尾部插入        |
|          | `unshift()`                                    | `arr.unshift(0)` 数组头部插入     |
|          | `splice(index, deleteNum, addItem1, addItem2)` | `arr.splice(1, 0, 1.5)`           |
| **删除** | `pop()`                                        | `arr.pop()` 尾部                  |
|          | `shift()`                                      | `arr.shift()` 头部                |
|          | `splice()`                                     | `arr.splice(1, 2)`                |
|          | `filter()`                                     | `arr.filter(item => item == 3)`   |
|          | `slice(beginIndex, endIndex)`                  | 包前不包后                        |
| **查找** | `indexOf()`                                    | `arr.indexOf(3)`                  |
|          | `includes()`                                   | `arr.includes(3)`                 |
|          | `find()`                                       | `arr.find(item => item > 2)`      |
|          | `findIndex()`                                  | `arr.findIndex(item => item > 2)` |

### 链表

链表（linked list）是一种线性数据结构，其中的每个元素都是一个节点对象，各个节点通过“引用”相连接。引用记录了下一个节点的内存地址，通过它可以从当前节点访问到下一个节点。可在内存区域分散储存。

- **单向链表**：即前面介绍的普通链表。单向链表的节点包含值和指向下一节点的引用两项数据。我们将首个节点称为头节点，将最后一个节点称为尾节点，尾节点指向空 `None` 。
- **环形链表**：如果我们令单向链表的尾节点指向头节点（首尾相接），则得到一个环形链表。在环形链表中，任意节点都可以视作头节点。
- **双向链表**：与单向链表相比，双向链表记录了两个方向的引用。双向链表的节点定义同时包含指向后继节点（下一个节点）和前驱节点（上一个节点）的引用（指针）。相较于单向链表，双向链表更具灵活性，可以朝两个方向遍历链表，但相应地也需要占用更多的内存空间。

|          | 数组                           | 链表           |
| :------- | :----------------------------- | :------------- |
| 存储方式 | 连续内存空间                   | 分散内存空间   |
| 容量扩展 | 长度不可变                     | 可灵活扩展     |
| 内存效率 | 元素占用内存少、但可能浪费空间 | 元素占用内存多 |
| 访问元素 | O(1)                           | O(n)           |
| 添加元素 | O(n)                           | O(1)           |
| 删除元素 | O(n)                           | O(1)           |

### 列表(动态数组)

动态数组继承了数组的各项优点，并且可以在程序运行过程中进行动态扩容。许多编程语言中的标准库提供的列表是基于动态数组实现的，例如 Python 中的 `list` 、Java 中的 `ArrayList` 、C++ 中的 `vector` 和 C# 中的 `List`还有JavaScript的数组等。

## 堆栈与队列

### 栈：先进后出

- **浏览器中的后退与前进、软件中的撤销与反撤销**。每当我们打开新的网页，浏览器就会对上一个网页执行入栈，这样我们就可以通过后退操作回到上一个网页。后退操作实际上是在执行出栈。如果要同时支持后退和前进，那么需要两个栈来配合实现。
- **程序内存管理**。每次调用函数时，系统都会在栈顶添加一个栈帧，用于记录函数的上下文信息。在递归函数中，向下递推阶段会不断执行入栈操作，而向上回溯阶段则会不断执行出栈操作。

| 特性         | 栈（Stack）        | 堆（Heap）                    |
| :----------- | :----------------- | :---------------------------- |
| **分配方式** | 自动分配和释放     | 手动分配和释放                |
| **生命周期** | 与函数绑定         | 由程序员控制                  |
| **内存大小** | 较小，固定         | 较大，动态                    |
| **访问速度** | 快                 | 慢                            |
| **管理方式** | 编译器自动管理     | 程序员手动管理                |
| **使用场景** | 局部变量、函数调用 | 动态分配的对象、数组，如: new |
| **内存碎片** | 无                 | 可能存在                      |
| **安全性**   | 高                 | 低                            |

```c
#include <stdio.h>
#include <stdlib.h>

void stackExample() {
    int x = 10; // x 分配在栈上
    printf("Stack: %d\n", x);
} // 函数结束，x 自动释放

void heapExample() {
    int* ptr = (int*)malloc(sizeof(int)); // ptr 分配在堆上
    *ptr = 20;
    printf("Heap: %d\n", *ptr);
    free(ptr); // 需要手动释放
}

int main() {
    stackExample();
    heapExample();
    return 0;
}
```

### 队列：先进先出

## 哈希表

哈希函数的作用是将一个较大的输入空间映射到一个较小的输出空间。在哈希表中，输入空间是所有 `key` ，输出空间是所有桶（数组索引）。换句话说，输入一个 `key` ，**我们可以通过哈希函数得到该 `key` 对应的键值对在数组中的存储位置**。



元素查询效率：

|          | 数组 | 链表 | 哈希表 |
| -------- | :--- | :--- | :----- |
| 查找元素 | O(n) | O(n) | O(1)   |
| 添加元素 | O(1) | O(1) | O(1)   |
| 删除元素 | O(n) | O(n) | O(1)   |

### 哈希表使用

```javascript
/* 初始化哈希表 */
const map = new Map();
/* 添加操作 */
// 在哈希表中添加键值对 (key, value)
map.set(12836, '小哈');
map.set(15937, '小啰');
map.set(16750, '小算');
map.set(13276, '小法');
map.set(10583, '小鸭');

/* 查询操作 */
// 向哈希表中输入键 key ，得到值 value
let name = map.get(15937);

/* 删除操作 */
// 在哈希表中删除键值对 (key, value)
map.delete(10583);

/* 清除所有键值对 */
map.clear();
```

### 哈希表简单实现

```typescript
/* 键值对 Number -> String */
class Pair {
    public key: number;
    public val: string;

    constructor(key: number, val: string) {
        this.key = key;
        this.val = val;
    }
}

/* 基于数组实现的哈希表 */
class ArrayHashMap {
    private readonly buckets: (Pair | null)[];

    constructor() {
        // 初始化数组，包含 100 个桶
        this.buckets = new Array(100).fill(null);
    }

    /* 哈希函数 */
    private hashFunc(key: number): number {
        return key % 100;
    }

    /* 查询操作 */
    public get(key: number): string | null {
        let index = this.hashFunc(key);
        let pair = this.buckets[index];
        if (pair === null) return null;
        return pair.val;
    }

    /* 添加操作 */
    public set(key: number, val: string) {
        let index = this.hashFunc(key);
        this.buckets[index] = new Pair(key, val);
    }

    /* 删除操作 */
    public delete(key: number) {
        let index = this.hashFunc(key);
        // 置为 null ，代表删除
        this.buckets[index] = null;
    }

    /* 获取所有键值对 */
    public entries(): (Pair | null)[] {
        let arr: (Pair | null)[] = [];
        for (let i = 0; i < this.buckets.length; i++) {
            if (this.buckets[i]) {
                arr.push(this.buckets[i]);
            }
        }
        return arr;
    }

    /* 获取所有键 */
    public keys(): (number | undefined)[] {
        let arr: (number | undefined)[] = [];
        for (let i = 0; i < this.buckets.length; i++) {
            if (this.buckets[i]) {
                arr.push(this.buckets[i].key);
            }
        }
        return arr;
    }

    /* 获取所有值 */
    public values(): (string | undefined)[] {
        let arr: (string | undefined)[] = [];
        for (let i = 0; i < this.buckets.length; i++) {
            if (this.buckets[i]) {
                arr.push(this.buckets[i].val);
            }
        }
        return arr;
    }

    /* 打印哈希表 */
    public print() {
        let pairSet = this.entries();
        for (const pair of pairSet) {
            console.info(`${pair.key} -> ${pair.val}`);
        }
    }
}
```

### 哈希冲突

从本质上看，哈希函数的作用是将所有 `key` 构成的输入空间映射到数组所有索引构成的输出空间，而输入空间往往远大于输出空间。因此，理论上一定存在“多个输入对应相同输出”的情况。例如：

```
12836 % 100 = 36
20336 % 100 = 36
```

都会映射到36索引的bucket。容易想到，哈希表容量 n 越大，多个 `key` 被分配到同一个桶中的概率就越低，冲突就越少。因此，可以通过扩容哈希表来减少哈希冲突。

类似于数组扩容，哈希表扩容需将所有键值对从原哈希表迁移至新哈希表，非常耗时；并且由于哈希表容量 `capacity` 改变，我们需要通过哈希函数来重新计算所有键值对的存储位置，这进一步增加了扩容过程的计算开销。为此，编程语言通常会预留足够大的哈希表容量，防止频繁扩容。因为哈希表扩容需要进行大量的数据搬运与哈希值计算。为了提升效率，我们可以采用以下策略。

1. 改良哈希表数据结构，**使得哈希表可以在出现哈希冲突时正常工作**。
2. 仅在必要时，即当哈希冲突比较严重时，才执行扩容操作。

改良方法主要包括：

#### 链式地址

在原始哈希表中，每个桶仅能存储一个键值对。链式地址（separate chaining）将单个元素转换为链表，将键值对作为链表节点，将所有发生冲突的键值对都存储在同一链表中。

![hash-chain](./img/hash-chain.png)

值得注意的是，当链表很长时，查询效率 O(n) 很差。**此时可以将链表转换为“AVL 树”或“红黑树”**，从而将查询操作的时间复杂度优化至 O(log ⁡n) 。

#### 开放寻址

开放寻址（open addressing）不引入额外的数据结构，而是通过“多次探测”来处理哈希冲突，探测方式主要包括线性探测、平方探测和多次哈希等(不再深入探究)。目前不同编程语言都已通过不同技术方式实现哈希改良。

## 排序

理想情况：运行快、原地(无须借助额外的辅助数组)、稳定(相等元素在数组中的相对顺序不发生改变)、自适应、通用性好。

### 冒泡排序(bubble sort)

通过连续地比较与交换相邻元素实现排序

```javascript
/* 冒泡排序 */
function bubbleSort(nums) {
    // 外循环：未排序区间为 [0, i]
    for (let i = nums.length - 1; i > 0; i--) {
        // 内循环：将未排序区间 [0, i] 中的最大元素交换至该区间的最右端
        for (let j = 0; j < i; j++) {
            if (nums[j] > nums[j + 1]) {
                // 交换 nums[j] 与 nums[j + 1]
                let tmp = nums[j];
                nums[j] = nums[j + 1];
                nums[j + 1] = tmp;
            }
        }
        if (!flag) break; // 此轮“冒泡”未交换任何元素，直接跳出
    }
}
```

> - **时间复杂度为 O(n<sup>2</sup>)、自适应排序**：在引入 `flag` 优化后，最佳时间复杂度可达到 O(n) 。
> - **空间复杂度为 O(1)、原地排序**：指针 i 和 j 使用常数大小的额外空间。
> - **稳定排序**：由于在“冒泡”中遇到相等元素不交换。

### 选择排序(selection sort)

开启一个循环，每轮从未排序区间选择最小的元素，将其放到已排序区间的末尾

```javascript
/* 选择排序 */
function selectionSort(nums) {
    let n = nums.length;
    // 外循环：未排序区间为 [i, n-1]
    for (let i = 0; i < n - 1; i++) {
        // 内循环：找到未排序区间内的最小元素
        let k = i;
        for (let j = i + 1; j < n; j++) {
            if (nums[j] < nums[k]) {
                k = j; // 记录最小元素的索引
            }
        }
        // 将该最小元素与未排序区间的首个元素交换
        [nums[i], nums[k]] = [nums[k], nums[i]];
    }
}
```

> 时间复杂度O( n<sup>2</sup> )；空间复杂度O(1)

### 插入排序(insertion sort)

开启一个循环，每轮从未排序区间选择第一个的元素，将其向前移动放到已排序区间的正确位置

```javascript
/* 插入排序 */
function insertionSort(nums) {
    // 外循环：已排序区间为 [0, i-1]
    for (let i = 1; i < nums.length; i++) {
        let base = nums[i],
            j = i - 1;
        // 内循环：将 base 插入到已排序区间 [0, i-1] 中的正确位置
        while (j >= 0 && nums[j] > base) {
            nums[j + 1] = nums[j]; // 将 nums[j] 向右移动一位
            j--;
        }
        nums[j + 1] = base; // 将 base 赋值到正确位置
    }
}
```

> 时间复杂度O( n<sup>2</sup> )；空间复杂度O(1)；稳定排序

> 实际使用中**插入排序的使用频率显著高于冒泡排序和选择排序**，冒泡的计算开销比插入要大，选择排序的时间复杂度一直是O(n<sup>2</sup>)，插入排序在本来就有序时能达到最优O(n)

### 快速排序(quick sort)

基于分治策略

1. 选取数组最左端元素作为基准数，初始化两个指针 `i` 和 `j` 分别指向数组的两端。
2. 设置一个循环，在每轮中使用 `i`（`j`）分别寻找第一个比基准数大（小）的元素，然后交换这两个元素。
3. 循环执行步骤 `2.` ，直到 `i` 和 `j` 相遇时停止，最后将基准数交换至两个子数组的分界线。

```javascript
/* 元素交换 */
swap(nums, i, j) {
    let tmp = nums[i];
    nums[i] = nums[j];
    nums[j] = tmp;
}

/* 哨兵划分 */
partition(nums, left, right) {
    // 以 nums[left] 为基准数
    let i = left,
        j = right;
    while (i < j) {
        while (i < j && nums[j] >= nums[left]) {
            j -= 1; // 从右向左找首个小于基准数的元素
        }
        while (i < j && nums[i] <= nums[left]) {
            i += 1; // 从左向右找首个大于基准数的元素
        }
        // 元素交换
        this.swap(nums, i, j); // 交换这两个元素
    }
    this.swap(nums, i, left); // 将基准数交换至两子数组的分界线
    return i; // 返回基准数的索引
}

/* 快速排序 */
quickSort(nums, left, right) {
    // 子数组长度为 1 时终止递归
    if (left >= right) return;
    // 哨兵划分
    const pivot = this.partition(nums, left, right);
    // 递归左子数组、右子数组
    this.quickSort(nums, left, pivot - 1);
    this.quickSort(nums, pivot + 1, right);
}
```

> 时间复杂度O( n log n)；空间复杂度O(n)

**基准数优化**可以尽量防止出现基准数大小过大或国小而导致的左右树不平均，时间复杂度增加的情况：

```javascript
/* 选取三个候选元素的中位数 */
medianThree(nums, left, mid, right) {
    let l = nums[left],
        m = nums[mid],
        r = nums[right];
    // m 在 l 和 r 之间
    if ((l <= m && m <= r) || (r <= m && m <= l)) return mid;
    // l 在 m 和 r 之间
    if ((m <= l && l <= r) || (r <= l && l <= m)) return left;
    return right;
}

/* 哨兵划分（三数取中值） */
partition(nums, left, right) {
    // 选取三个候选元素的中位数
    let med = this.medianThree(
        nums,
        left,
        Math.floor((left + right) / 2),
        right
    );
    // 将中位数交换至数组最左端
    this.swap(nums, left, med);
    // 以 nums[left] 为基准数
    let i = left,
        j = right;
    while (i < j) {
        while (i < j && nums[j] >= nums[left]) j--; // 从右向左找首个小于基准数的元素
        while (i < j && nums[i] <= nums[left]) i++; // 从左向右找首个大于基准数的元素
        this.swap(nums, i, j); // 交换这两个元素
    }
    this.swap(nums, i, left); // 将基准数交换至两子数组的分界线
    return i; // 返回基准数的索引
}
```

## 分治

一个问题是否适合分治，可以分三个判断依据：

- **问题可以分解**：原问题可以分解成规模更小、类似的子问题，以及能够以相同方式递归地进行划分。
- **子问题是独立的**：子问题之间没有重叠，互不依赖，可以独立解决。
- **子问题的解可以合并**：原问题的解通过合并子问题的解得来。

### 经典分治算法问题

- **寻找最近点对**：该算法首先将点集分成两部分，然后分别找出两部分中的最近点对，最后找出跨越两部分的最近点对。
- **大整数乘法**：例如 Karatsuba 算法，它将大整数乘法分解为几个较小的整数的乘法和加法。
- **矩阵乘法**：例如 Strassen 算法，它将大矩阵乘法分解为多个小矩阵的乘法和加法。
- **汉诺塔问题**：汉诺塔问题可以通过递归解决，这是典型的分治策略应用。
- **求解逆序对**：在一个序列中，如果前面的数字大于后面的数字，那么这两个数字构成一个逆序对。求解逆序对问题可以利用分治的思想，借助归并排序进行求解。

## 回溯

## 动态规划

## 贪心

