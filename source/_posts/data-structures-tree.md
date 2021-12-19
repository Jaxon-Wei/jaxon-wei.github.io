---
title: 数据结构的那些树
date: 2021-11-05 01:06:50
tags: 
- tree
- 数据结构
categories: 
- 基础技术
typora-root-url: ../../source
---

树是一种很基础且应用广泛的数据结构，由n（n>=0）个节点组成具有层次关系的集合，因为把它可视化出来，像一棵抽象的树，所以会使用“树”来统称这类数据结构。

<!-- more -->

树具有以下2个特点：

- 有且只有一个称为根的节点；
- 除根以外的节点可分为m（m>0）个互不相交的有限集T1,T2,T3,...,Tm，其中每一个集合本身又是一棵树，可以称为子树（SubTree）。

关于树的几个术语：

- 边（Edge）：节点之间的连线；
- 路径（Path）：从一个节点到另外一节点之间的有序的边和节点，路径的顺序限制为从上至下，不存在以叶子节点为起点或者以子节点为起点的路径；
- 节点的深度（Depth）：当前节点到根节点之间边的数量，比如：下图节点4的深度为2；
- 节点的层级（Level）：当前节点到根节点的边的数量+1（*level = depth + 1*），比如：下图节点4的深度为3；
- 节点的高度（Height）：当前节点到最深的子节点之间边的数量，叶子节点没有高度，比如：下图节点3的深度为2；
- 节点的度（Degree）：当前节点拥有子节点数量，叶子节点的度数为0；
- 树的高度：为根节点的高度；
- 树的度数（Degree）：树内各节点度的最大值，度数为树的一个状态值；
- 树的阶数（Order）：每个结点能拥有的最多结点数，阶数为树的一个限制定义；

### Binary Tree 二叉树

有且只有1个根节点度数(子节点数)<=2的树，称为：二叉树。二叉树的子节点有左右之分，次序不能任意颠倒。

<img src="/images/data-structures-tree/image-20211105203134311.png" alt="image-20211105203134311" style="zoom:50%;" />

### Binnary Search Tree 二叉查找树

二叉查找树又可以称为：二分搜索树，二叉搜索树。它的定义如下：

- 任意一节点，若它的左子节点不为空，那么它的左子节点值小于该节点的值；
- 任意一节点，若它的右子节点不为空，那么它的右子节点值小于该节点的值；

<img src="/images/data-structures-tree/image-20211105210339238.png" alt="image-20211105210339238" style="zoom:50%;" />

由于二叉树没有约束根节点的左右子节点的高度差，那么二叉查找树的极限状态可能是只有右节点的树或者只有左节点的树，被称为：斜树（Skew Tree)。

<img src="/images/data-structures-tree/image-20211105211729409.png" alt="image-20211105211729409" style="zoom:50%;" />

二叉树的查找时间复杂度为$O(log N)$，最坏情况为$O(N)$

***在线动图演示：***[Binary Search Tree Visualization (usfca.edu)](https://www.cs.usfca.edu/~galles/visualization/BST.html)

### Balanced Binary Tree 平衡二叉树

为了提升查找效率，避免出现斜树，那么诞生了平衡二叉树，即：对于任意结点左子树与右子树高度差不超过1的二叉搜索树，又被称为AVL树（GM Adelson - Velsky和EM Landis于1962发明）。平衡二叉树引入了***平衡因子***（balance factor）的概念，即：左子树高度减去右子树高度。

平衡二叉树在进行节点的删除、添加时，会影响***平衡因子***，破坏树的平衡，那么在删除、添加节点的时候，就需要进行旋转来保障树的平衡。AVL树的旋转操作分为以下4种：

- 单次左旋转（LL Rotation）：应用于右侧节点的高度大于左侧子节点的高度时，且右侧子节点也是平衡或者右侧较重的；

  ![AVL Tree LL Rotation](/images/data-structures-tree/LL_Rotation.png)

- 单次右旋转（RR Rotation），应用于左侧节点的高度大于右侧子节点的高度时，且左侧子节点也是平衡或者左侧较重的；

  ![AVL Tree RR Rotation](/images/data-structures-tree/RR_Rotation.png)

- 左右旋转（LR Rotation）：应用于左侧子节点的高度大于右侧子节点的高度，并且左侧子节点右侧较重。

  ![AVL Tree LR Rotation](/images/data-structures-tree/LR_Rotation.png)

- 右左旋转（RL Rotation）：应用于右侧子节点的高度大于左侧子节点的高度，并且右侧子节点左侧较重。

![AVL Tree RL Rotation](/images/data-structures-tree/RL_Rotation.png)

平衡二叉树的搜索时间复杂度为$O(log N)$，变更(删除/插入)时间复杂度也为$O(log N)$；

***在线动图演示：***[AVL Tree Visualzation (usfca.edu)](https://www.cs.usfca.edu/~galles/visualization/AVLtree.html)

### Red-Black Trees 红黑树

一种特定的二叉树，为平衡二叉找树的变体，但它又不属于平衡二叉树，因为它的平衡因子可能大于1。

红黑树的约束如下：

- 节点为红或黑色（节点具有颜色属性，故称为红黑树）；
- 根节点为黑色；
- 所有叶子节点（NIL）为黑色；
- 每个红色节点的2个子节点都是黑色（从每个叶子到根的路径上不能有2个连续的红色节点）；
- 从任一节点到其每个叶子节点的所有路径都包含相同数目的黑色节点；

4-5条约束强制了红黑树：从根到叶子的最长路径小于等于最短路径的2倍长，保障了树的平衡因子不会太大。红黑树最差的情况为一棵子树全部为黑色节点，另外一棵子树为依次交替的红黑节点，这时根节点平衡因子为n；

***在线动图演示：***[Red/Black Tree Visualization (usfca.edu)](https://www.cs.usfca.edu/~galles/visualization/RedBlack.html)

linux内核api关于红黑树的说明：[linux/rbtree.rst at master · torvalds/linux · GitHub](https://github.com/torvalds/linux/blob/master/Documentation/core-api/rbtree.rst)

> Red-black trees are similar to AVL trees, but provide faster real-time bounded worst case performance for insertion and deletion (at most two rotations and three rotations, respectively, to balance the tree), with slightly slower (but still O(log n)) lookup time.

红黑树类似于AVL树，但插入和删除最差情况的实时性更好（最多两个分别旋转和三次旋转以平衡树），查找时间稍慢，仍为$O(log N)$。

### Multiway Tree 多路树

对于m阶的多路树，有以下特点：

- 每个结点可以拥有最多$m$个子节点；
- 每个结点拥有最多$m-1$个元素（或称为主健）和$m$个指针指向子节点；

比如下图为4路树：

![1606581653-76844](/images/data-structures-tree/1606581653-76844.png)

### M-way Search Trees 多路搜索树

多路搜索树是一棵限制更加严格的多路树（主键、节点是有序的），它的特征如下：

- 每个结点可以拥有最多$m$个子节点；
- 每个结点拥有最多$m-1$个元素（或称为主健）和$m$个指针指向子节点；
- 每个结点的主键按升序排列；
- 前i个子节点的元素都小于第$i$个元素；
- 后$i+1$个子节点的元素都大于第i个元素；

### B-Tree B树

B树是一种特殊的多路树，它拥有以下差异性的特征：

- 根节点
  - 拥有$2$到$m$个子节点（或者根节点本身就是叶子节点）；
- 内部节点
  - 存储$m-1$个key；
  - 拥有$m/2$到$m$个子节点；
- 叶子节点
  - 存储$(m-1)/2$到$m-1$个key；
  - 拥有相同的深度（Level）；

![image-20211211154802491](/images/data-structures-tree/image-20211211154802491.png)

***在线动图演示：***[B-Tree Visualization (usfca.edu)](https://www.cs.usfca.edu/~galles/visualization/BTree.html)

### B+ Tree B+树

B+ Tree解决了B-Tree通过存储数据指针来索引的弊端，改为只在叶子节点存储数据指针，它的特性如下：

- 根节点
  - 拥有$2$到$m$个子节点（或者根节点本身就是叶子节点）
- 内部节点
  - 存储$m-1$个key；
  - 拥有$m/2$到$m$个子节点；
- 叶子节点
  - 拥有相同的深度（Level）；
  - 存储数据项(data);
  - 存储$L/2$到$L$个数据项；



![image-20211211154847796](/images/data-structures-tree/image-20211211154847796.png)

B+Tree更适用于硬盘存储

- 节点能存储很多的key，一次访问可以加载更多到内存或者缓存中；
- 内部节点只有key没有data，只有叶子节点才存储key和data，可以加载更多的节点数据到内存；

***在线动图演示：***[B+ Tree Visualization (usfca.edu)](https://www.cs.usfca.edu/~galles/visualization/BPlusTree.html)
