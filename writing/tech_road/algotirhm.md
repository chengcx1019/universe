

## codejam

比实现代码更重要的是思考问题的方式以及如何构思将要采取的方案。

明确的知道你基本上可以解决问题比一昧的担心问题的一些细枝末节而迟迟不动手要有用的多。

发现问题的模式和策略

- [ ] 优先队列的实现方式

  - [ ] 数组，堆（斐波那契堆）

- [ ] 树

  - [ ] 堆
  - [x] 二叉搜索树（dfs前中后序遍历）\bfs
  - [ ] 二叉平衡树AVL
    1. 本身首先是一棵二叉搜索树
    2. 带有平衡条件：每个非叶子节点左右子树的高度之差的绝对值（平衡因子）最多为1.
  - [ ] 红黑树
    1. 节点是红色或黑色。
    2. 根是黑色。
    3. 所有叶子都是黑色（叶子是NIL节点）。
    4. 每个红色节点必须有两个黑色的子节点。（从每个叶子到根的所有路径上不能有两个连续的红色节点。）
    5. 从任一节点到其每个叶子的所有简单路径都包含相同数目的黑色节点。

- [ ] 图

  基本定义：

  ```
  连通图：图的任意两点之间都存在一条边
  ```

  - [ ] 存储结构
    - [ ] 邻接表
    - [ ] 邻接矩阵（邻接矩阵的性质）
  - [ ] 应用
    - [x] 最小生成树：无向有权图，是原图边的子集，边的权值总和最小

  寻找边的权值总和最小的连通子图

  - [x] 图的搜索及最短路径
  - [x] DFS
  - [x] BFS
  - [x] 正权重，单源顶点最短路径Dijkstra
  - [x] 含负权重但不含负环，单源顶点最短路径Bellman-For
  - [x] 任意两个顶点间最短路径FloydWarshall

二分查找，归并排序，快速排序

- [ ] 查找

  查找相对而言比较简单，不外乎顺序查找，二分查找，哈希表查找和二叉排序树查找。

- [ ] 排序

  稳定排序

  - [ ] 中值排序

应用：

利用图解决迷宫问题时，如何根据迷宫去构建图，更具体来说，如何从迷宫中确定顶点及边。





## 剑指offer2

2

2.4

- 重点掌握二分查找、归并排序和快速排序，做到随时正确、完整的写出代码，
- 如果面试要求在二维数组（可能表现为迷宫或者棋盘）上搜索路径，那么可以尝试回溯法。通常回溯法很适合递归代码实现。
- 如果是求某个问题的最优解，而且该问题可以分为多个子问题，那么我们可以尝试用动态规划。用自上而下的递归思路去分析动态规划问题的时候，会发现子问题之间存在重叠的更小的子问题。为了避免不必要的重复计算，我们用自下而上的循环代码来实现，也就是把子问题的最优解先算出来并用数组保存下来，接下来基于子问题的解计算大问题的解。
- 能得到最优解的那么往往要考虑贪婪算法





### 动态规划

动态规划实质上是一种优化技术，动态规划的思想为：建立详细的最优解表。先从简单的子问题开始。通过较小问题的最优解创建越来越大问题的最优解。

#### 动态规划和分治：

动态规划是分治范式的延伸，只有当问题具有明确限制和先决条件时，才能对问题采用动态规划方法。

这两个条件是：

- 问题的最优解由子问题的最优解构成
- 问题可以被分解为子问题，这些子问题可以重复使用或者可以使用递归算法解决而不会产生新的子问题

如果满足这两个限制，那么分治问题就可以采用动态规划的方法完成。

> 二分查找是一个典型的分治问题，但二分查找没有重叠子问题，不是动态规划问题。

动态规划的两种技术(**memoization** and **tabulation**) 

- memoization

  **Memoization (top-down cache filling)** refers to the technique of caching and reusing previously computed results

  ```python
  def fib(n, mem: dict):
      if n = 0 or n = 1:
          return n
      if n not in mem:
          mem[n] = fib(n - 1, mem) + fib(n - 2, mem)
      return mum[n]
  ```

- tabulation

  **Tabulation (bottom-up cache filling)** is similar but focuses on filling the entries of the cache. Computing the values in the cache is easiest done iteratively. 

  ```python
  def fib(n, mem: dict):
      mem[0] = 0
      mem[1] = 1
      for i in range(2, n+1):
          mem[i] = mem[i-1] + mem[i-2]
      return mem[n]
  ```

  缓存有效的原因是分治问题里有重叠的子问题

  二分查找没有重叠子问题，不是动态规划问题

#### 解决动态规划问题的步骤

> [参考](http://blog.refdash.com/dynamic-programming-tutorial-example/)

- 识别动态规划问题

- 确定问题变量

- 清楚的表达递归关系

  假定已经完成了 所有的子问题之后，如何解决主要的问题，

- 确定基案本例(base case)

  不依赖任何子问题的最小子问题

  确定这个问题之前，需要想清楚主要的问题是如何简化为更小的问题，以及在什么时候问题不能被进一步简化

  问题不能被简化的原因——其中一个参数由于问题的约束，变成了一个不可能的值，即确定边界条件

- 决定是要用迭代还是递归实现

- 添加缓存（自顶向下）

- 确定时间复杂度

  有一些简单的规则可以使计算动态编程问题的计算时间复杂性容易得多

#### 典型问题

通常我们会从斐波那契数列开始谈动态规划问题，但这次为了更深入的理解动态规划，我们来点不一样的，从最小编辑距离开始看吧。

- 最小编辑距离

  首先简单描述“编辑距离”，两个字符串，将a转化成b，采用插入、删除、和替换3种变换方式，最少的变化次数称为“编辑距离”，以下问题都是将a转化成b，且矩阵a的索引i作为行标，b的索引j作为列标，那么我们可以想象，一种操作次数最长的方式就是，从尾到头删除a，再从头到尾增加b。

  实际操作过程中，我们需要保存一个决策矩阵，矩阵元素的递推关系如下：

  ![](https://camo.githubusercontent.com/384008e2a31829248d01e4375399508555a2f900/68747470733a2f2f77696b696d656469612e6f72672f6170692f726573745f76312f6d656469612f6d6174682f72656e6465722f7376672f66306134386563666339383532633034323338326664633333633139653131613136393438653835)

$$
  
$$

- 斐波那契数列及其变体青蛙跳台阶问题
- 背包问题

**参考**

[Dynamic Programming: First Principles](http://www.flawlessrhetoric.com/Dynamic-Programming-First-Principles)

[利润最大化](https://lukasmericle.github.io/dynprotut/)





##### 递归和循环：

递归是在一个函数的内部调用这个函数自身，而循环则是通过设置计算的初始值及终止条件，在一个范围内重复运算。递归虽然简洁，但是需要更多的时间和空间资源，因而效率不如循环；另外，递归的本质就是把一个问题分解成许多个小问题。如果多个小问题存在相互重叠的部分，就存在重复计算。



通常应用动态规划解决问题时，都是用递归的思路分析问题，但由于递归分解的子问题中存在大量的重复，因此我们总是用**自下而上**的**循环**来实现代码。



##### 查找和排序

查找相对而言比较简单，不外乎顺序查找、二分查找、哈希表查找和二叉排序树查找。二分查找是必备知识点。



排序比查找要复杂一些，经常会要求比较插入排序、冒泡排序、归并排序、快速排序等不同算法的优劣，能够从额外空间消耗、平均时间复杂嘟和最差时间复杂嘟等比较有缺点。尤其对快速排序要特别精通



堆排序可视作提升的选择排序



### 树

> 一般指二叉树，由于树的结构特殊，所以基本树的算法都采用递归的方式实现，起始递归的实现方式只是分治思想的一种体现，因为关于树的很多问题其实都是可以相应的分解成类似的子问题。
>
> 如果子问题存在重复的计算过程，可以采用后序遍历的方式，自底向上的进行实现，例如判断一棵树是不是平衡的。
>
> 针对每一个递归问题的描述，作这样一个约定
>
> - 问题是如何分解成相同的一系列子问题
> - 递归结束条件

#### 树的表示

数组（list）

类

#### 树的遍历

深度优先

广度优先（从底向上逐层顺序输出树节点）

前中后序遍历的循环及递归算法实现

#### 平衡树

#### 二叉搜索树BST

> 二叉搜索树的中序遍历即对应顺序的数组

- 判断一棵树是否是二叉树

#### 线段树（segment tree)

是一棵完全二叉树

线段树应该支持的操作包括**updata**和**query**

- update

  更新输入数组中的某一个元素并对线段树做相应的改变。

  当输入数组中位于`i`位置的元素被更新时，我们只需从这一元素对应的叶子结点开始，沿二叉树的路径向上更新至更结点即可。显然，这一过程是一个`O(logn)`的操作。

- query

  用来查询某一区间对应的信息（如最大值，最小值，区间和等）



#### 树的应用

- 将二叉搜索树转换成一个排序的双向链表

  采用二叉树的中序遍历

- 最小(大)堆

- 寻找最小公共节点

- B树是不是A树的子结构

- 根据前序遍历和中序遍历序列重建二叉树

- 判断一棵树是不是平衡

  不仅要判断根节点是否平衡，还要判断子树本身是否平衡

- 判断一个序列是不是二叉搜索树的后序遍历序列

  思路：首先根据后序遍历的特性，序列的最后一个节点是当前树（或者子树）的根节点，根据根节点寻找到左子树和右子树序列（依据左子树序列的每一个元素都小于根节点，右子树序列的每一个元素都大于根节点，一旦原则上进入了右子树序列，出现比根节点小的元素，则可断定这不是后序遍历序列），对左右子树序列进行相同判断。（PS：递归停止条件在实现里描述）

  实现：定义起始指针left=0，确定左子序列，定义右子树序列指针right=left，如果当前序列只有一个节点，则会返回true，事实上除了根节点，是不会出现空序列进入递归，单元素序列会返回true，不会进一步递归。

  ```python
  def is_post_order(order: list):
       # 事实上除了最初序列为空，递归过程中不会出现空序列进入调用的情况，在单元素就会返回了，而且一定会返回True
      if order is None or len(order) == 0: 
          return False
      root = order[-1]
      left = 0
      # 如果只有一个元素的话，不会进入循环，因而不会出现数组越界，并且 left=0，right=left，返回true
      while order[left] < root:  
          left += 1
      right = left
      while right < len(order) - 1:
          if order[right] < root:
              return False
          right += 1
      is_left = True if left == 0 else is_post_order(order[:left])  # 左子序列为空, 因而left = 0，左子满足
      is_right = True if left == right else is_post_order(order[left: right])  # 右子树为空，因而right==left，右子满足
      return is_left and is_right
  ```

- 二叉树中和为某一值的所有路径

  > 路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。
  >
  > 在寻找所有路径的过程自然是要使用回溯的思想

  思路：首先判断该节点是不是叶节点，累加该节点的值，如果当前路径（该节点是叶节点）的累加和和预期值一致，那么保存该路径；如果不是叶节点，或者没有达到预期值，该节点还有左子和右子，那么继续递归访问左子和右子，访问结束后，回溯到该节点的父节点（回溯的具体操作为从累加和中去除该节点的值，从 当前“路径”【不是上面明确定义的路径，而是记录从根节点一路过来访问的所有节点】中去除该节点）。

  实现：参见下方

  ```python
  def find_path(bst_root, expected_sum):
      if bst_root is None:
          return
      path = []
      current_num = 0
      self._find_path(bst_root, expected_sum, path, current_num)
  
  def _find_path(bst_root, expected_sum, path, current_sum):
      is_leaf = bst_root.left is None and bst_root.right is None
      current_sum += bst_root.data
      path.append(bst_root.data)
      if current_sum == expected_sum and is_leaf:
          print(path[:])
  
      if bst_root.left is not None:
          _find_path(bst_root.left, expected_sum, path, current_sum)
      if bst_root.right is not None:
          _find_path(bst_root.right, expected_sum, path, current_sum)
  
      # cut current node from path and current value before returning to parent
      current_sum -= bst_root.data
      path.pop()
  ```

### 图

#### 如何用图思考

[原文](https://medium.freecodecamp.org/i-dont-understand-graph-theory-1c96572a1401)



#### 图的表示

邻接数组-静态图

邻接表-动态图

邻接矩阵

#### 图的遍历

> 相比广度优先使用队列存储未访问的邻接节点，深度遍历的循环实现方式使用栈存储当前节点的未访问邻接节点

##### 广度优先搜索

广度优先搜索从边上来说，都是从源点到图中任意顶点的最短路径的最短路径

##### 深度优先搜索

顶点访问顺序会改变计数器的值，所以需要注意临接节点的顺序，深度优先搜索计算出来的结果非常有用，包括**拓扑排序**，寻找**强连通部**，寻找网络中潜在的弱点。

深度优先搜索结束后，可以使用每个顶点存储的前序节点值找到一条从**任意顶点到原点s的路径**，当然，这条路径也许不是最短路径。

深度优先搜索仅仅依靠当前信息，是一种盲目的搜索，它没有一个明智的计划来快速达到目标顶点t。

##### 深度优先搜索的应用

###### 有向无环图中的拓扑排序

###### 有向图中计算强连通分量

##### 边的分类

根据在图G上进行深度优先搜索所产生的深度优先森林

- 树边(tree edge)
- 反向边(back edge)
- 正向边(forward edge)
- 交叉边(cross edge)



#### 最短路径

有向无环图

##### 单源最短路径

###### 非负边代价（Dijkstra）

###### 任意边代价（Bellman-Ford）

不可包含总权值为负值的环

##### 任意两个顶点间最短路径

动态规划Floyd-Warshall

#### 最小生成树

给定无向连通图$G=(V,E)$

#### 关于图的其他

- 欧拉回路(不重复经过所有边)和曼哈顿回路(不重复经过所有点)
- 



### 动态规划

- 最优二叉查找树

  - 包含n个健的二叉查找树的总数量等于第n个卡塔兰树
    $$
    当n>0时，c(n)=\cfrac{1}{n+1}[_n^{2n}],c(0)=1
    $$

    - 卡特兰数

      不能穿过对角线意味着任意时间向右的步数不能小于向左的步数，向右的步数记为左括号，向上的步数记为右括号

### 回溯

## 秣马厉兵

1. 假定图的邻接矩阵元素取值规则为：顶点之间存在边时对应元素值为1否则为0；下面关于图（顶点数大于1）的邻接矩阵描述正确的是

- 无向图的邻接矩阵对应特征值必然为非负数
- 无向图的邻接矩阵不一定可以对角化
- 有向无环图的邻接矩阵对应特征值必然为正数
- 无向完全图的邻接矩阵必然有大于0的特征值
- n顶点无向连通图对应邻接矩阵1的个数为2(n-1)则必然有环
- 以上描述都不正确

1. 对于矩阵的特征值和奇异值来说，以下说法正确的有
   - 奇异值分解只能作用于方针，而特征分解可以作用于任意矩阵
   - 特征分解只能作用于方阵，而奇艺值分解可以作用于任意矩阵
   - 奇异值分解和特征分解都可以作用于任意矩阵
   - 相比特征值，奇异值能更高效地压缩信息
   - 相比奇异值，特征值能更高效地压缩信息
   - 以上均不正确
2. 对数组[60,50,70,35,45,25]做从小到大排序（使用<堆排序>，建堆方式是从前往后建立最大堆例如[120，70，80，65]），从建堆到排序的过程中需要多少次元素交换？比如60，50，70，35，45，25->35,50,70,60,45,25,元素60和35交换记做一次元素交换

二元关联的相关度矩阵，连通图的最大生成子树，该子树的关联度加和



- [ ] 机器学习理解目标函数及变分推理blog
- [ ] 图和树算法，邻接矩阵的性质





20180906-算法引擎工程师

1. 提升处理器流水线并行度的方法有哪些
2. 从整个工程系统的角度，提升核心进程CPU分支预测准确率的方法有哪些
3. 在整体计算机体系中，有哪些带数据存储功能的单元？各自的数据访问时延是多少
4. c++程序中，变量会被分配在哪些区域？详细描述如何决定的？如何利用其提升程序性能？
5. 在软件工程体系中，进行分层设计的原因、目标与原则分别是什么？
6. 计算机体系结构中有哪些数据与指令cache
7. 河上有一个船，野人与传教士持续不定量到来，设计一个调度算法，要求每船4人，要么全是野人、要么全是传教士、要么数量相等。要求平均的等待+渡河时间越短越好
8. JVM的cms gc算法，是如何对内存进行压缩的
9. 如果让你来设计一个高性能、高稳定性的AI算法引擎，你会考虑哪些方面？会给系统设计哪些子模块？具体采用哪些技术点来实现
10. c++程序，如何减少开发阶段内存泄漏错误，如何排查与定位运行时内存泄漏错误。详述具体方法。
11. 纯手工实现一个函数，解析出淘宝搜索链接中的每一个元素，并作为一个完整结构体返回。函数传入的字符串可改。要求性能越高越好，不能调用第三方函数。
12. 有一个文本，2亿行，每行长度255个字符以内，长度不定，均为可见字符。在16core64G内存的机器上，对文件进行读入，排序（字典），输出成一个文件。要求完成整个工作的时间越短越好。



作者：慧大士

链接：https://www.zhihu.com/question/56029613/answer/147357122

来源：知乎

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

腾讯的数据挖掘或者说机器学习岗统称为基础研究，无论是bat还是其他的滴滴，今日头条，搜狗等等都需要大量数据挖掘岗，想要拿到offer，一般需要从两方面准备：

1  基础知识，涵盖两方面：基础数据结构算法，编程语言和数据库；机器学习主流算法原理及细节

1.1 基础数据结构算法，编程语言和数据库

 基础数据结构算法，这个没的说，要确保万无一失，需要掌握二叉树、链表、动态规划等等所有常考笔试面试题。

编程语言，无论平时用什么语言，c++和java必须掌握一个，需要能够使用常用的vector，map，set，queue，deque等等数据结构来解决一些常见面试编程题题。

数据库，数据库基本语法得会，我腾讯一面的时候直接上来就是用纸写一个sql的编程题，所以说常见sql编程题也必须会。

1.2 机器学习主流算法原理及细节

通过搜索往年的面经，可以发现机器学习或者数据挖掘面试常考的问题基本差不多，16年面试了百度，腾讯，滴滴，搜狗，蘑菇街等等公司，基本关于机器学习算法问的大同小异，从出现频率和重要程度排个序：

LR原理及公式推导，有哪些优化方法，梯度下降，牛顿法以及各种变种，L1、L2范数的区别，优缺点。（这个考的是最多的）

GBDT、XGBOOST原理异同，如何并行化等等（最好去xgboost官方看原始论文，讲得很清楚）

随机森林原理及细节。

如何解决数据不均衡问题。

SVM原理及细节，SVM和树模型的异同以及优缺点和局限性。

推荐系统，协同过滤原理，基于用户、物品等等。

深度学习中的一些小问题，比如relu是什么，如何解决梯度消失等等，这个最好了解下，毕竟深度学习这么火。

2 项目经历

项目这块，无论是数据挖掘竞赛还是一些项目，一定要自己把整个过程梳理一下，切记一问项目就是 ”提特征，跑模型，拿第一“，需要把项目的亮点体现出来，说白了就是你这个项目为什么牛逼，你做出了哪些厉害的贡献，这个需要技巧和准备。

其实这些企业再招聘的时候，如果你有不错的项目经历或者实习经历，是比较占优势的，毕竟面试时间就那么多，稍微聊一聊项目时间就消耗的差不多了，而且聊项目的时候主动权是在你的手里；但是如果你没有像样的项目，这就给了面试官提问的机会了，那就比较考验你的真正实力了。



## 算法和数据结构

### 典例精析

- 图广度优先搜索

  - Word Ladder2

    给定起始词和终止词以及字典表,要求每次只能修改一个字符，且中间词必须出现在字典表中，求出最短路径的所有可能

    ```python
    def _findLadders(start, end, dic):
        # dic.add(end)
        def construct(wordList):
            neis = {}
            for word in wordList:
                for i in range(len(word)):
                    s = word[:i] + "*" + word[i + 1:]
                    if s not in neis:
                        neis[s] = [word]
                    else:
                        neis[s].append(word)
            return neis
    
        level = {start}  # 逐层搜索邻居
        parents = collections.defaultdict(set)
        neis = construct(dic)
        while level and end not in parents:
            next_level = collections.defaultdict(set)
            for node in level:
                for i in range(len(node)):
                    s = node[:i] + '*' + node[i + 1:]
                    neighbors = neis.get(s, [])
                    for n in neighbors:
                        if n not in parents:
                            next_level[n].add(node)
            level = next_level
            parents.update(next_level)
            
        ret = [[end]]
        while ret and ret[0][0] != start:
            ret = [[p] + r for r in ret for p in parents[r[0]]]  # 如果不存在路径，ret=[]
        return ret
    
    ```

    - word ladder

      最短路径

      ```python
      from collections import deque, defaultdict
      class Solution(object):
          def ladderLength(self, beginWord, endWord, wordList):
              """
              :type beginWord: str
              :type endWord: str
              :type wordList: List[str]
              :rtype: int
              """
              def construct_word(word_list):
                  neighbor_list = defaultdict(list)
                  for word in word_list:
                      for i in range(len(word)):
                          s = word[:i] + '*' + word[i+1:]
                          neighbor_list[s].append(word)
                  return neighbor_list
              
              def bfs_word(beginWord, endWord, neighbor_list):
                  visited = set()
                  queue = deque()
                  
                  queue.append((beginWord, 1))
                  while queue:
                      word, step = queue.popleft()
                      if word not in visited:
                          visited.add(word)
                          if word == endWord:
                              return step
                          for i in range(len(word)):
                              s = word[:i] + '*' + word[i+1:]
                              neighbors = neighbor_list.get(s, [])
                              for neighbor in neighbors:
                                  if neighbor not in visited:
                                          queue.append((neighbor, step+1))
                  return 0
              neighbor_list = construct_word(wordList)
              return bfs_word(beginWord, endWord, neighbor_list)
      ```

- 数组和问题

  - 顺序顺时针打印m*n数组

    ```python
    def print_array(nums, m, n):
        start = 0
        result = []
        while start*2 < m and start*2 <n:
            print_circle(nums, m, n, start, result)
            start += 1
     
    def print_circle(nums, m, n, start, result):
        row = m - start*2
        col = n - start*2
        end_w = start + col - 1  # 当前循环最后一个数行索引
        end_h = start + row - 1  # 当前循环最后一个数列索引
        # left -> right
        for j in range(start, end_w+1):
            result.append(nums[start][j])
        # top -> bottom
        if start < end_h:
        	for i in range(start+1, end_h+1):
                result.append(nums[i][end_h])
        if start < end_h and start < end_w:
            for j in range(start, end_w)[::-1]:
                result.append(nums[end_h][j])
        if start < end_h -1  and start < end_w:
            for i in range(start+1, end_h)[::-1]:
                result.append(nums[i][start])
        
                
    ```

  - s2o面试题31 连续子数组的最大和

    ```python
    def max_sum(nums):
        ret = float('-inf') 
        curr = 0
        for num in nums:
            if curr < 0:
                curr = num
             else:
                curr += num
             ret = max(ret, curr)
    ```

  - 最长公共字串,输出最长子串长度并返回最长子串

    ```python
    
    ```

  - 有序数组。和为s的两个数字VS和为s的连续正数序列

    - 定义两个指针，start和end,分别当前序列的头和尾，curr记录当前序列的和，循环条件是start < (s+1)/2,否则start+end就已经会大于s了， 如果curr==s，则记录当前start和end，继续寻找，如果curr>s,从curr-start，并将头指针start后移



  ```python
  def seq_sum_s(nums, s):
      start = 1
      end = 2
      mid = (sum+1)//2
      curr = 0
      ret = []
      while start < mid:
          if curr == s:
              ret.append((start, end))
          elif curr > s:
              curr -= start
              start += 1
              continue
          end += 1
          curr += end
      return ret
  
  ```

  - 所有和为s的连续正数序列

  ```python
  def two_sum_s(nums, s, result=[]):
      found = False
      start = 0
      end = len(nums) - 1
      while start < end:
          curr = nums[start] + nums[end]
          if curr == s:
              found = True
              result.append(start)
              result.append(end)
              return found = False
          elif curr > s:
              end -= 1
          else
          	start += 1
      return found
  ```

  - 和为s的两个数

    两个指针分别记录头指针和尾指针，如果两个数满足则返回，若大于s，则尾指针后退，若小于s，则头指针前进

- 将字母按字母表位置重新排序，不能改变相同字母的相对顺序，其他字符不变

- ```python
  def is_letter(char):
      if 64 < ord(char) < 91 or 96 < ord(char) < 123:
          return True
      else:
          return False
  
  
  def sort(line):
      first = 0
      for m in range(len(line) - 1, -1, -1):
          if is_letter(line[m]):
              first = m
              break
      for k in range(len(line)):
          i = first
          for j in range(len(line) - 1, k-1, -1):
              if is_letter(line[j]):
                  if ord(line[i].lower()) < ord(line[j].lower()):
                      line[i], line[j] = line[j], line[i]
                  i = j
      return line
  ```

- 两个栈实现队列

  stack1存放队尾元素，stack2存放队首元素

  每次入队前检查stack2是否为空，不为空，则把stack2的元素都压入stack1中

  每次出队前检查stack1是否为空，不为空，将stack1中的元素压入stack2中

  ```python
  class QueueByStack(object):
      def __init__(self):
          self.stack1 = []
          self.stack2 = []
      
      def push(self, value):
          while self.stack2:
              self.stack1.append(self.stack2.pop())
          self.stack1.append(value)
      
      def pop(self):
          while self.stack1:
              self.stack2.append(self.stack1.pop())
          return self.stack2.pop() if self.stack2 else None
  ```


- 关于single numbe问题可以泛化成这样一类问题：数组中的数字都出现K次，只有一个除外，它出现了p次，且p%k != 0，那么我们需要选取m个数（k <= 2^m）来记录数组中数字的每一位状态（即统计每个数字每一位一出现的个数，由m位来记录），且需保证当每一位的计数达到K时，需要一个mask来将该位置0，该mask的取值与k有关

  **`mask = ~(y1 & y2 & ... & ym)`, where `yj = xj` if `kj = 1`, and `yj = ~xj` if `kj = 0` (`j = 1` to `m`).**

  以leetcode中三道single numbe问题进行说明：

  - k=2,p=1

    分析：2^1=2,即只需要一个32位数来记录所有数字每一位的状态,m=1,记录到2每一位自动置零，无需mask

    ```python
    def single_num1(nums: list):->int
        x1 = 0
        for item in nums:
            x1 ^=(item)
        return x1
    ```

  - k=3,p=1

    分析：2^2>=3,即需要两个32位数来记录所有数字每一位的状态,m=2，记录到3每一位需要mask置0，`mask=~(x1 & x2)`因为3的二进制表示为`11`,因为p=1，所以返回x1, 为了更具有一般性，稍后会以k=5,p=3进行说明

    ```python
    def single_num(nums: list):->int
        x1 = 0
        x2 = 0
        for item in nums:
            x2 ^= (x1 ^ item)
            x1 ^= (item)
            mask = ~(x1 & x2)  # for k= 3 0b11
            x2 = x2 & mask
            x1 = x1 & mask
        return x1  # for p=1 0b1
    ```

  - 第3题略有不同，数组中数字都出现2次，剩余两个不同的数都只出现一次

  思路是结合第一题，将这两个不同的数分到两个不同的组中

  ```python
  def _xor(nums):
      ret = 0
      for item in nums:
          ret ^= item
      return ret
  
  def single_num3(nums: list):->list
      if not nums:
          return -1
      xor = _xor(nums)
      mask = 1
      while (mask & xor) == 0:
          mask << 1
      a = 0
      b = 0
      for item in nums:
          if item & mask:
              a ^= item
          else:
              b ^= item 
      return [a, b]
     
  ```

  - k=5,p=3进一步说明m的选取及p!=1时应当返回哪个值

    因为2^3 >=5,m=3  p=3 0b011，k=5，二进制表示为0b101

    ```python
    def single_num_general(nums: list):->int
        x1 = 0
        x2 = 0
        x3 = 0
        for item in nums:
            x3 ^= (x2 & x1 & item)
            x2 ^= (x1 & item)
            x1 ^= (item)
            mask = ~(x3 & ~x2 & x1)  # for k 0b101
        	x3 &= mask
            x2 &= mask
            x1 &= mask
        return x1  # for p ob011,可以返回x1或x2, 1只能由出现p次的元素产生
    ```


- 任意序列的排列组合

很多问题其实都是要得到数字间的排列可能

组合：

```python
def group(nums: list):->list
    tmp = nums[:]
    ret = []
    for item in nums:
        ret.append([item])
        tmp.pop(0)
        for group in group(tmp):
            ret.append([item] + group)
    return ret
```

排列：

```python
def permutation(nums: list):->list
    if len(nums) == 1:
        return [nums]
    ret = []
    for index, item in enumerate(nums):
        tmp = nums[:]
        tmp.pop(index)
        for per in permutation(tmp):
            ret.append([item] + per)
    return ret
```

- 正则表达式匹配

  采用递归或动态规划$dp(i, j)$表示s[:i]和p[:j]是否匹配

  ```python
  def regular_expression_matching(s, p):->bool
      """
      s:sring
      p:pattern
      """
      mem = {}
      def dp(i, j):
          if (i, j) not in mem:
              if j == len(p):
                  ans = i == len(s)
              else:
                  first_match = i < len(s) and p[j] in {s[i], '.'}
                  if j+1 < len(p) and p[j+1] == '*':
                      ans = dp(i, j+2) or first_match and dp(i+1, j)
                  else:
                      ans = first_match and dp(i+1, j+1)
              mem[i, j] = ans
                      
          return mem[i, j]
  ```

- 栈的压入弹出序列

  ```python
  def is_pop_order(push_v, pop_v):->bool
      if not push_v or not pop_v or len(push_v) != len(pop_v):
          return False
      stack = []
      while pop_v:
          top_pop = pop_v[0]
      	if not stack and stack[-1] == top_pop:
              stack.pop()
              pop_v.pop(0)
          else:
              while push_v:
                  if push_v[0] != pop_v[-1]:
                      stack.append(push_v.pop(0))
                  else:
                      push_v.pop(0)
                      pop_v.pop(0) 
                      break
          if not push_v:
              while stack:
                  if stack.pop() != pop_v.pop(0):
                      return False
       return True
  ```


##### 递归和循环：

递归是在一个函数的内部调用这个函数自身，而循环则是通过设置计算的初始值及终止条件，在一个范围内重复运算。递归虽然简洁，但是需要更多的时间和空间资源，因而效率不如循环；另外，递归的本质就是把一个问题分解成许多个小问题。如果多个小问题存在相互重叠的部分，就存在重复计算。



通常应用动态规划解决问题时，都是用递归的思路分析问题，但由于递归分解的子问题中存在大量的重复，因此我们总是用**自下而上**的**循环**来实现代码。



##### 查找和排序

查找相对而言比较简单，不外乎顺序查找、二分查找、哈希表查找和二叉排序树查找。二分查找是必备知识点。



排序比查找要复杂一些，经常会要求比较插入排序、冒泡排序、归并排序、快速排序等不同算法的优劣，能够从额外空间消耗、平均时间复杂嘟和最差时间复杂嘟等比较有缺点。尤其对快速排序要特别精通



堆排序可视作提升的选择排序

### 排序

#### 讨论

- 稳定与否

  - 稳定：冒泡，插入，归并

- 排序算法时间复杂度

- 最坏时间复杂度是O(nlogn)的是

  |          |           |           |
  | -------- | --------- | --------- |
  | 选择排序 | shell排序 | 插入排序  |
  | 归并排序 | 快速排序  | 堆排序    |
  | 冒泡排序 | comb sort | -cocktail |



- 排序算法中，每经过一次交换产生新的逆序对的是：快排

  - 冒泡每交换一次，会减少一个逆序对，并不会产生新的逆序对。 

  - 简单插入排序，若插入到已排序序列的最后，则不会产生新的逆序对。 

  -  简单选择排序，每次将一个元素归位。无论由小到大归位，还是由大到小归位，都不会产生新的逆序对。 

  - 快排，是跳跃式交换，必然会产生新的逆序对。且不再产生逆序对时该轮终止。

- 排序算法可以分为这样几大类：交换排序（包括bubble sort， cocktail sort， comb sort, quicksort），选择排序（选择排序，堆排序），插入排序（插入排序，希尔排序），合并排序（合并排序）

|               | average                            | best                                                         | worst                                                        | 备注           |
| :------------ | ---------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------- |
| 选择排序      | О(*n*2) comparisons, О(*n*) swaps  | О(*n*2) comparisons, О(*n*) swaps                            | О(*n*2) comparisons, О(*n*) swaps                            |                |
| shell排序     | depends on gap sequence            | O(*n* log *n*)                                               | O(n2) (worst known gap sequence)<br/>O(*n* log2*n*) (best known gap sequence) | 插入排序的改进 |
| 插入排序      | О(*n*2) comparisons, swaps         | O(*n*) comparisons, O(*1*) swaps                             | О(*n*2) comparisons, swaps                                   |                |
| 归并排序      | O(*n* log *n*)                     | O(*n* log *n*)                                               | O(*n* log *n*)                                               |                |
| 快速排序      | O(*n* log *n*)                     | O(*n* log *n*)                                               | O(*n*2)                                                      |                |
| 堆排序        | O(n\log n)                         | Best-case performance	O(n\log n) (distinct keys)<br/> O(n) (equal keys) | O(nlog n)                                                    |                |
| 冒泡排序      | О(*n*2) comparisons, О(*n*2) swaps | О(*n*) comparisons, О(*1*) swaps                             | О(*n*2) comparisons, О(*n*2) swaps                           |                |
| comb sort     |                                    |                                                              |                                                              | 冒泡排序的改进 |
| Cocktail sort |                                    |                                                              |                                                              |                |
|               |                                    |                                                              |                                                              |                |



#### 常见排序算法描述及实现

冒泡排序：重复比较相邻的两个元素，如果存在错误顺序，则交换她们的位置

```python
def bubble_sort(nums):
    for i in range(len(nums)-1):
        for j in range(len(nums)-1-i):
            if nums[j] > nums[j+1]:
                nums[j+1], nums[j] = nums[j], nums[j+1]
```



选择排序：找到数组中最大活着最小的数，之后将其放在排序数组的正确位置

```python
def selection_sort(nums):
    for i in range(len(nums)):
        mini = i
        for j in range(i+1, len(nums)):
            if nums[j] < nums[i]:
                mini = j
        nums[i], nums[mini] = nums[mini], nums[i]
```

Shell排序：是直接选择排序的改进，选择一组gap值，则相应将间距为gap的元素分成了一组，组件元素内采用冒泡排序，直到gap为1，整个序列就排好了。

```python
def shell_sort(nums, step):
    gap = len(nums) // step
    while gap >0:
        for i in range(len(nums)-1, -1, -gap):
            maxi = i
           	for j in range(i, -1, -gap):
                if nums[j] > nums[maxi]:
                    maxi = j
        	nums[i], nums[maxi] = nums[maxi], nums[i]
        gap = gap // step
```



插入排序：从第二个数字开始依次与前面排好序的子数组序列进行比较

```python
def insert_sort(nums):
    for i in range(1, len(nums)):
        for j in range(i):
            if nums[i] > nums[j]:
                tmp = nums[i]
                nums[j+1, i+1] = nums[j:i]
                nums[j] = tmp
```

归并排序：将数组均分成两个子数组，对每一半都进行排序，之后合并

```python
def _merge(left, right)
	l = 0
    r = 0
    ret = []
    while l < len(left) and r < len(right):
        if left[l] <= right[r]:
            ret.append(left[l])
            l += 1
        else:
            ret.append(right[r])
            r += 1
    while l < len(left):
        ret.append(left[l])
        l += 1
    while r < len(right):
        ret.append(right[r])
        r += 1
    return ret

def merge_sort(nums):
    if len(nums) < 2:
        return nums
    mid = len(nums) // 2
    left = merge_sort(nums[:mid])
    right = merge_sort(nums[mid:])
    return _merge(left, right)
    
```



快速排序：相比归并排序，快排降低了空间复杂度，并且没有使用辅助空间。选择一个轴pivot，左边的元素比轴小，右边的元素比轴大。

通常我们使用数组的第一个元素为轴。

```python
def partition(nums, start, end):
    position = start + 1
    pivot_pos = start
    for j in range(start+1, end+1):
        if nums[j] < nums[pivot_pos]:
            nums[position], nums[j] = nums[j], nums[position]
            position += 1
    position -= 1  # 定位最后一个比pivot要小的数
    nums[position], nums[pivot] = nums[pivot], nums[position]
    return position

def quick_sort(nums, start, end):
    if start < end:
        pivot_pos = partition(nums, start, end)
        quick_sort(nums, start, pivot_pos-1)
        quick_sort(pivot_pos+1, start, end)
```



**reference**

https://www.hackerearth.com/zh/practice/notes/sorting-code-monk/

[9种排序算法的可视化及比较](http://k.sina.com.cn/article_1715118170_m663aa05a001002qyc.html)









### 树

> 一般指二叉树，由于树的结构特殊，所以基本树的算法都采用递归的方式实现，起始递归的实现方式只是分治思想的一种体现，因为关于树的很多问题其实都是可以相应的分解成类似的子问题。
>
> 如果子问题存在重复的计算过程，可以采用后序遍历的方式，自底向上的进行实现，例如判断一棵树是不是平衡的。
>
> 针对每一个递归问题的描述，作这样一个约定
>
> - 问题是如何分解成相同的一系列子问题
> - 递归结束条件



#### 树的表示

数组（list）

类

- 节点和度的关系

  节点树=总度数+1，除根节点外，每个节点都对应一个父节点的度，因而叶子节点数=总度数+1-带度节点数

- 二叉树度为2的节点和叶子节点的关系：

  `2*n2+n1+1 = n2+n1+n0` -> `n2+1=n0`



#### 树的遍历

深度优先

广度优先（从底向上逐层顺序输出树节点）

前中后序遍历的循环及递归算法实现

- Trie,前缀树

#### 二叉搜索树BST

> 二叉搜索树的中序遍历即对应顺序的数组

- 判断一棵树是否是二叉树

#### 线段树（segment tree)

是一棵完全二叉树

线段树应该支持的操作包括**updata**和**query**

- update

  更新输入数组中的某一个元素并对线段树做相应的改变。

  当输入数组中位于`i`位置的元素被更新时，我们只需从这一元素对应的叶子结点开始，沿二叉树的路径向上更新至更结点即可。显然，这一过程是一个`O(logn)`的操作。

- query

  用来查询某一区间对应的信息（如最大值，最小值，区间和等）

#### 平衡树

核心为四种旋转操作

- Left Rotation:左旋,右子树右子节点（均无论左右）

- Right Rotation:右旋,左子树左子节点

- Left-Right Rotation:先左旋再右旋,左子树右子节点

- Right-Left Rotation:先右旋再左旋,右子树左子节点

  话不多说，让我们用代码实现吧，我们将实现各种操作的代码定义在AVLNode中，node中包含4个主要变量,value,left,right和height

  ```python
  class AVLNode(object):
      """
      一旦节点的左右子树发生变更，就需要重新计算height，需要重新计算的节点的子树都不存在节点插入的情况，
      因而只要根据左右子树的高度就能计算出该节点的高度，使用compute_height()方法，但是在新节点的插入过程中，存在子树节点的变化
      """
  
      def __init__(self, value):
          self.value = value
          self.left = None
          self.right = None
          self.height = 0
  
      # 节点插入左子树左子节点
      def rotate_right(self):
          new_root = self.left
          self.left = new_root.right
          new_root.right = self
  
          # new_root会在外部重新计算高度，需要调整高度的只有当前节点self，
          self.compute_height()
  
          return new_root
  
      # 节点插入右子树右子节点
      def rotate_left(self):
          new_root = self.right
          self.right = new_root.left
          new_root.left = self
  
          # new_root会在外部重新计算高度，需要调整高度的只有当前节点self
          self.compute_height()
  
          return new_root
  
      # 节点插入左子树右子节点
      def rotate_left_right(self):
          # 首先以self.left为轴进行左旋转，构造插入左子树左节点的形状，之后再以self为轴进行右旋转
          # 新的root节点是self.left.right
          first_root = self.left
          new_root = first_root.right
          first_root.right = new_root.left
          new_root.left = first_root
  
          self.left = new_root.right
          new_root.right = self
  
          # new_root会在外部重新计算高度，需要调整高度的有当前节点self, 和first_root
          self.compute_height()
          first_root.compute_height()
          return new_root
  
      # 节点插入右子树左节点
      def rotate_right_left(self):
          # 首先以self.right为轴进行右旋转，构造插入右子树右子节点的形状，之后再以self为轴进行右旋转
          # 新的root节点是self.right.left
          first_root = self.right
          new_root = first_root.left
          first_root.left = new_root.right
          new_root.right = first_root
  
          self.right = new_root.left
          new_root.left = self
  
          # new_root会在外部重新计算高度，需要调整高度的有当前节点self, 和first_root
          self.compute_height()
          first_root.compute_height()
          return new_root
  
      def add_subtree(self, parent, value):
          if not parent:
              return AVLNode(value)
          parent = parent.insert(value)  # 插入完成后，root可能会发生变化，因而需要重新赋值
          return parent
  
      def insert(self, value):
          """
          判断新插入节点后到底属于哪种情况：
          挂载完新节点后，从新节点的第一个父节点开始判断，该节点是否平衡，如不平衡，判断属于哪种情况的不平衡，
          根据value插入的是哪棵子树，并比较子树根节点和value的大小：
          1. 左子树left，且value < left.value: rotate_right右旋转
          2. 左子树left，且value > left.value: rotate_left_right 先左旋转，再右旋转
          3. 右子树right，且value > right.value: rotate_left右旋转
          4. 右子树right，且value < right.value: rotate_right_left 先右旋转，再左旋转
          """
          new_root = self
          if self.compare_to(value) > 0:
              self.left = self.add_subtree(self.left, value)
              if self.left.dynamic_difference() > 1:
                  if self.left.compare_to(value) >= 0:
                      new_root = self.rotate_left()
                  else:
                      new_root = self.rotate_left_right()
          else:
              self.right = self.add_subtree(self.right, value)
              if self.right.dynamic_difference() < -1:
                  if self.right.compare_to(value) < 0:
                      new_root = self.rotate_left()
                  else:
                      new_root = self.rotate_right_left()
  
          # 左右子树的height均已重新计算过
          new_root.compute_height()
          return new_root
  
      def remove_from_parent(self, parent, value):
          if parent:
              return parent.remove(value)
          return None
  
      def remove(self, value):
          """
          1. 叶节点直接删除
          2. 左右子树有一边为空，返回剩余一边
          3. 左右均不为空 删除的节点左右子树均不为空，那么用左子树最大的元素a（必为叶节点）替换被删除节点，然后递归删除a节点
          """
          new_root = self
          compare = self.compare_to(value)
          if compare == 0:
              if self.left is None:
                  return self.right
              if self.right is None:
                  return self.left
              node = self.left
              while node:
                  node = node.right
              most_left_key = node.value
              self.value = most_left_key
              self.left = self.remove_from_parent(self.left, most_left_key)
              if self.dynamic_differnece() == -2:  # 只可能左子树低于右子树
                  if self.right.dynamic_differnece() < 0:
                      new_root = self.rotate_left()
                  else:
                      new_root = self.rotate_right_left()
          elif compare < 0:
              self.right = self.remove_from_parent(self.right, value)
              if self.dynamic_difference() == 2:  # 只可能左子树高于右子树
                  if self.left.dynamic_differnece() > 0:
                      new_root = self.rotate_right()
                  else:
                      new_root = self.rotate_left_right()
          else:
              self.left = self.remove_from_parent(self.left, value)
              if self.dynamic_difference() == -2:  # 只可能左子树低于右子树
                  if self.right.dynamic_differnece() < 0:
                      new_root = self.rotate_left()
                  else:
                      new_root = self.rotate_right_left()
          new_root.compute_height()
          return new_root
  
      def compare_to(self, target):
          tag = 0
          if self.value < target:
              tag = -1
          elif self.value > target:
              tag = 1
          return tag
  
      def compute_height(self):
          height = -1
          if self.left:
              height = max(height, self.left.height)
          if self.right:
              height = max(height, self.right.height)
  
          self.height = height + 1
  
      def dynamic_height(self):
          height = -1
          if self.left:
              height = max(height, self.left.dynamic_height())
          if self.right:
              height = max(height, self.right.dynamic_height())
  
          return height + 1
  
      def compute_difference(self):
          left_target = 0
          right_target = 0
          if self.left:
              left_target = 1 + self.left.height
          if self.right:
              right_target = 1 + self.right.height
  
          return left_target - right_target
  
      def dynamic_difference(self):
          left_target = 0
          right_target = 0
          if self.left:
              left_target = 1 + self.left.dynamic_height()
          if self.right:
              right_target = 1 + self.right.dynamic_height()
  
          return left_target - right_target
  
      def assert_avl_property(self):
          if abs(self.dynamic_difference()) > 1:
              return False
          if self.left:
              if not self.assert_avl_property(self.left):
                  return False
          if self.right:
              if not self.assert_avl_property(self.right):
                  return False
  
          return True
  
      def bfs(self):
          queue = deque()
          queue.append(self)
  
          while queue:
              node = queue.popleft()
              yield node.value
              if node.left:
                  queue.append(node.left)
              if node.right:
                  queue.append(node.right)
  
  
  class AVLTree(object):
  
      def __init__(self):
          self.root = None
  
      def insert(self, value):
          if not value:
              raise TypeError("value can't be None")
          if not self.root:
              self.root = AVLNode(value)
          else:
              self.root = self.root.insert(value)
  
      def remove(self, value):
          if not value:
              raise TypeError("value can't be None")
          if self.root:
              self.root = self.root.remove(value)
  
      def __contains__(self, target):
          if not target or not self.root:
              return False
          node = self.root
          while node:
              if node.compare_to(target) > 0:
                  node = node.left
              elif node.compare_to(target) < 0:
                  node = node.right
              else:
                  return True
          return False              
  ```


#### 红黑树

#### A*树

#### B系列

核心思想都是采用二分法和数据平衡策略来提升数据查找的速度。

##### B树

又名平衡多路查找树，数据库索引技术里大量使用者B树和B+树的数据结构

一棵m阶B树的特点：

- 每个节点最多有m个子节点
- 除跟节点和叶节点外，其他节点最少有ceil(m/2)个节点（ceil向上取整）
- 跟节点有两个子节点
- 所有的叶子节点都在同一层
- 有j个孩子的非叶节点有j-1个关键码，关键码按递增顺序排列

##### B+树

B+树是B树的一个升级版，相对于B树来说B+树更充分的利用了节点的空间，让查询速度更加稳定，其速度完全接近于二分法查找。

- 非叶节点不包含关键字指针
- 叶子节点包含父节点的所有关键字信息，且包含父节点关键字记录指针（即地址），且按从小到大链接
- 根节点关键字数量与其子节点的个数相同
- 非叶节点只存关键字的索引而不存地址，所有数据地址必须到叶节点才能查询到

##### B*树

B+树的变种

- 关键字个数限制不同，B*树ceil(2/3m),B+和B一样
- 节点可以向兄弟节点转移，减少分解次数

#### 树的应用

- 将二叉搜索树转换成一个排序的双向链表

  采用二叉树的中序遍历

- 最小(大)堆

- 寻找最小公共节点

- B树是不是A树的子结构

- 根据前序遍历和中序遍历序列重建二叉树

- 判断一棵树是不是平衡

  不仅要判断根节点是否平衡，还要判断子树本身是否平衡

- 判断一个序列是不是二叉搜索树的后序遍历序列

  思路：首先根据后序遍历的特性，序列的最后一个节点是当前树（或者子树）的根节点，根据根节点寻找到左子树和右子树序列（依据左子树序列的每一个元素都小于根节点，右子树序列的每一个元素都大于根节点，一旦原则上进入了右子树序列，出现比根节点小的元素，则可断定这不是后序遍历序列），对左右子树序列进行相同判断。（PS：递归停止条件在实现里描述）

  实现：定义起始指针left=0，确定左子序列，定义右子树序列指针right=left，如果当前序列只有一个节点，则会返回true，事实上除了根节点，是不会出现空序列进入递归，单元素序列会返回true，不会进一步递归。

  ```python
  def is_post_order(order: list):
       # 事实上除了最初序列为空，递归过程中不会出现空序列进入调用的情况，在单元素就会返回了，而且一定会返回True
      if order is None or len(order) == 0: 
          return False
      root = order[-1]
      left = 0
      # 如果只有一个元素的话，不会进入循环，因而不会出现数组越界，并且 left=0，right=left，返回true
      while order[left] < root:  
          left += 1
      right = left
      while right < len(order) - 1:
          if order[right] < root:
              return False
          right += 1
      is_left = True if left == 0 else is_post_order(order[:left])  # 左子序列为空, 因而left = 0，左子满足
      is_right = True if left == right else is_post_order(order[left: right])  # 右子树为空，因而right==left，右子满足
      return is_left and is_right
  ```

- 二叉树中和为某一值的所有路径

  > 路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。
  >
  > 在寻找所有路径的过程自然是要使用回溯的思想

  思路：首先判断该节点是不是叶节点，累加该节点的值，如果当前路径（该节点是叶节点）的累加和和预期值一致，那么保存该路径；如果不是叶节点，或者没有达到预期值，该节点还有左子和右子，那么继续递归访问左子和右子，访问结束后，回溯到该节点的父节点（回溯的具体操作为从累加和中去除该节点的值，从 当前“路径”【不是上面明确定义的路径，而是记录从根节点一路过来访问的所有节点】中去除该节点）。

  实现：参见下方

  ```python
  def find_path(bst_root, expected_sum):
      if bst_root is None:
          return
      path = []
      current_num = 0
      self._find_path(bst_root, expected_sum, path, current_num)
  
  def _find_path(bst_root, expected_sum, path, current_sum):
      is_leaf = bst_root.left is None and bst_root.right is None
      current_sum += bst_root.data
      path.append(bst_root.data)
      if current_sum == expected_sum and is_leaf:
          print(path[:])
  
      if bst_root.left is not None:
          _find_path(bst_root.left, expected_sum, path, current_sum)
      if bst_root.right is not None:
          _find_path(bst_root.right, expected_sum, path, current_sum)
  
      # cut current node from path and current value before returning to parent
      current_sum -= bst_root.data
      path.pop()
  ```

- 字符编码最短路径

  ```python
  def shortest_hfm_tree(line):->int
      cache = {}
      for char in line:
          if char in cache:
              cache[char] += 1
          else:
              cache[char] = 0
      values =list(cache.values())
      ret = 0
      while len(values)>1:
          values.sort()
          w = values[0] + values[1]
          ret += w
          values = values[2:]
          values.append(w)
      return ret
  ```


### 图

#### 如何用图思考

[原文](https://medium.freecodecamp.org/i-dont-understand-graph-theory-1c96572a1401)



#### 图的表示

邻接数组-静态图

邻接表-动态图

邻接矩阵

#### 图的遍历

> 相比广度优先使用队列存储未访问的邻接节点，深度遍历的循环实现方式使用栈存储当前节点的未访问邻接节点

##### 广度优先搜索

广度优先搜索从边上来说，都是从源点到图中任意顶点的最短路径的最短路径

##### 深度优先搜索

顶点访问顺序会改变计数器的值，所以需要注意临接节点的顺序，深度优先搜索计算出来的结果非常有用，包括**拓扑排序**，寻找**强连通部**，寻找网络中潜在的弱点。

深度优先搜索结束后，可以使用每个顶点存储的前序节点值找到一条从**任意顶点到原点s的路径**，当然，这条路径也许不是最短路径。

深度优先搜索仅仅依靠当前信息，是一种盲目的搜索，它没有一个明智的计划来快速达到目标顶点t。

##### 深度优先搜索的应用

###### 有向无环图中的拓扑排序

###### 有向图中计算强连通分量

##### 边的分类

根据在图G上进行深度优先搜索所产生的深度优先森林

- 树边(tree edge)
- 反向边(back edge)
- 正向边(forward edge)
- 交叉边(cross edge)



#### 最短路径

有向无环图

##### 单源最短路径

###### 非负边代价（Dijkstra）

###### 任意边代价（Bellman-Ford）

不可包含总权值为负值的环

##### 任意两个顶点间最短路径

动态规划Floyd-Warshall

#### 最小生成树

给定无向连通图$G=(V,E)$

#### 关于图的其他

- 欧拉回路(不重复经过所有边)和曼哈顿回路(不重复经过所有点)
- 



### 回溯

### 位运算

位运算的主要性质：

- a^0 = a, a^a=0

位运算的应用：

- 加法 

  a&b << 1 得到进位，a^b得到各位

  循环上面的操作，直到不产生进位

- 将整数第n位置为，或者清零

- 整数最右边的1 a&(a-1)

- 将a转化为b需要反转的次数

  c = a^b，统计c中非0元素的数目

  ```python
  def flip_num(a, b):
      c = a ^ b
      count = 0
      while c:
          count += c & 1
          c = c >> 1
      return count
  ```



137  Single Number II 在一个所有数字都出现3次，只有一个数字出现一次的数组中找出这个数,可以泛化为由k,p定义的更广泛的问题

关键点是两个相同的数的异或为0， 比如A^B^A = B

###### I -- Statement of our problem

"Given an array of integers, every element appears `k` (`k > 1`) times except for one, which appears `p` times (`p >= 1, p % k != 0`). Find that single one."

###### II -- Special case with 1-bit numbers

As others pointed out, in order to apply the bitwise operations, we should rethink how integers are represented in computers -- by bits. To start, let's consider only one bit for now. Suppose we have an array of **1-bit** numbers (which can only be `0` or `1`), we'd like to count the number of `1`'s in the array such that whenever the counted number of `1` reaches a certain value, say `k`, the count returns to zero and starts over (in case you are curious, this `k` will be the same as the one in the problem statement above). To keep track of how many `1`'s we have encountered so far, we need a **counter**. Suppose the counter has `m` bits in binary form: `xm, ..., x1` (from most significant bit to least significant bit). We can conclude at least the following four properties of the counter:

1. There is an initial state of the counter, which for simplicity is zero;
2. For each input from the array, if we hit a `0`, the counter should remain unchanged;
3. For each input from the array, if we hit a `1`, the counter should increase by one;
4. In order to cover `k` counts, we require `2^m >= k`, which implies `m >= logk`.



Here is the key part: how each bit in the counter (`x1` to `xm`) changes as we are scanning the array. Note we are prompted to use bitwise operations. In order to satisfy the second property, recall what bitwise operations will not change the operand if the other operand is `0`? Yes, you got it: `x = x | 0` and `x = x ^ 0`.

Okay, we have an expression now: `x = x | i` or `x = x ^ i`, where `i` is the scanned element from the array. Which one is better? We don't know yet. So, let's just do the actual counting.

At the beginning, all bits of the counter is initialized to zero, i.e., `xm = 0, ..., x1 = 0`. Since we are gonna choose bitwise operations that guarantee all bits of the counter remain unchanged if we hit `0`'s, the counter will be `0` until we hit the first `1` in the array. After we hit the first `1`, we got: `xm = 0, ...,x2 = 0, x1 = 1`. Let's continue until we hit the second `1`, after which we have: `xm = 0, ..., x2 = 1, x1 = 0`. Note that `x1` changed from `1` to `0`. For `x1 = x1 | i`, after the second count, `x1` will still be `1`. So it's clear we should use `x1 = x1 ^ i`. What about `x2, ..., xm`? The idea is to find the condition under which `x2, ..., xm`will change their values. Take `x2` as an example. If we hit a `1` and need to change the value of `x2`, what must be the value of `x1` right before we do the change? The answer is: `x1` must be `1` otherwise we shouldn't change `x2` because changing `x1` from `0` to `1` will do the job. So `x2` will change value only if `x1` and `i`are both `1`, or mathematically, `x2 = x2 ^ (x1 & i)`. Similarly `xm` will change value only when `xm-1, ..., x1` and `i` are all `1`: **`xm = xm ^ (xm-1 & ... & x1 & i)`**. Bingo, we've found the bitwise operations!

However, you may notice that the bitwise operations found above will count from `0` until `2^m - 1`, instead of `k`. If `k < 2^m - 1`, we need some **"cutting" mechanism to reinitialize the counter to `0` when the count reaches `k**`. To this end, we apply bitwise **AND** to ` xm,..., x1`  with some variable called   `mask`, i.e., `xm = xm & mask, ..., x1 = x1 & mask`. If we can make sure that ` mask ` will be `0` only when the count reaches `k` and be `1` for all other count cases, then we are done. How do we achieve that? Try to think what distinguishes the case with ` k ` count from all other count cases. Yes, it's the count of `1`'s! For each count, we have unique values for each bit of the counter, which can be regarded as its state. If we write  k in its binary form: `km,..., k1`, we can construct    mask as follows:

**`mask = ~(y1 & y2 & ... & ym)`, where `yj = xj` if `kj = 1`, and `yj = ~xj` if `kj = 0` (`j = 1` to `m`).**

Let's do some examples:

```
`k = 3: k1 = 1, k2 = 1, mask = ~(x1 & x2)`;

k = 5: k1 = 1, k2 = 0, k3 = 1, mask = ~(x1 & ~x2 & x3);
```

In summary, our algorithm will go like this (`nums` is the input array):

```python
for i in nums:
    xm ^= (xm-1 & ... & x1 & i);
    x(m-1) ^= (xm-2 & ... & x1 & i);
    .....
    x1 ^= i;
    
    mask = ~(y1 & y2 & ... & ym) where yj = xj if kj = 1, and yj = ~xj if kj = 0 (j = 1 to m).

    xm &= mask;
    ......
    x1 &= mask;
```

###### III -- General case with 32-bit numbers

Now it's time to generalize our results from 1-bit number case to 32-bit integers. One straightforward way would be **creating `32` counters for each bit in the integer**. You've probably already seen this in other posted [solutions](https://discuss.leetcode.com/topic/455/constant-space-solution/4). However, if we take advantage of bitwise operations, we may be able to manage all the `32` counters "collectively". By saying "collectively", we mean using `m` **32-bit** integers instead of `32` **m-bit** counters, where `m` is the minimum integer that satisfies `m >= logk`. The reason is that bitwise operations apply only to each bit so operations on different bits are independent of each other (kind obvious, right?). This allows us to group the corresponding bits of the `32` counters into one 32-bit integer. Here is a schematic diagram showing how this is done.



![0_1510941016426_137. Single Number II .png](https://discuss.leetcode.com/assets/uploads/files/1510941017203-137.single-number-ii-resized.png)



The top row is the 32-bit integer, where for each bit, we have a corresponding m-bit counter (shown by the column below the upward arrow). Since bitwise operations on each of the `32` bits are independent of each other, we can group, say the `m-th` bit of all counters, into one 32-bit number (shown by the orange box). All bits in this 32-bit number (denoted as `xm`) will follow the same bitwise operations. Since each counter has `m` bits, we end up with `m` 32-bit numbers, which correspond to `x1, ..., xm` defined in part `II`, but now they are 32-bit integers instead of 1-bit numbers. Therefore, in the algorithm developed above, we just need to regard `x1`to `xm` as 32-bit integers instead of 1-bit numbers. Everything else will be the same and we are done. Easy, hum?

###### IV -- What to return

The last thing is what value we should return, or equivalently which one of `x1` to `xm` will equal the single element. To get the correct answer, we need to understand what the `m` 32-bit integers `x1` to `xm` represent. Take `x1` as an example. `x1` has `32` bits and let's label them as `r` (`r = 1` to `32`). After we are done scanning the input array, the value for the `r-th` bit of `x1` will be determined by the `r-th` bit of all the elements in the array (more specifically, suppose the total count of `1` for the `r-th` bit of all the elements in the array is `q`, `q' = q % k` and in its binary form: `q'm,...,q'1`, then by definition the `r-th` bit of `x1` will be equal to `q'1`). Now you can ask yourself this question: what does it imply if the `r-th` bit of `x1` is `1`?



The answer is to find what can contribute to this `1`. Will an element that appears `k` times contribute? No. Why? Because for an element to contribute, it has to satisfy at least two conditions at the same time: the `r-th` bit of this element is `1` and the number of appearance of this `1` is not an integer multiple of `k`. The first condition is trivial. The second comes from the fact that whenever the number of `1` hit is `k`, the counter will go back to zero, which means the corresponding bit in `x1` will be reset to `0`. For an element that appears `k` times, it's impossible to meet these two conditions simultaneously so it won't contribute. At last, only the single element which appears `p` (`p % k != 0`) times will contribute. If `p > k`, then the first `k * [p/k]` (`[p/k]`denotes the integer part of `p/k`) single elements won't contribute either. So we can always set `p' = p % k` and say the single element appears effectively `p'` times.



Let's write `p'` in its binary form: `p'm, ..., p'1` (note that `p' < k`, so it will fit into `m` bits). Here I **claim the condition** for `xj` to equal the single element is `p'j = 1` (`j = 1` to `m`), with a quick proof given below.



If the `r-th` bit of `xj` is `1`, we can safely say the `r-th` bit of the single element is also `1` (otherwise nothing can make the `r-th` bit of `xj` to be `1`). We are left to prove that if the `r-th` bit of `xj` is `0`, then the `r-th` bit of the single element can only be `0`. Just suppose in this case the `r-th` bit of the single element is `1`, let's see what will happen. At the end of the scan, this `1` will be counted `p'` times. By definition the `r-th` bit of `xj` will be equal to `p'j`, which is `1`. This contradicts with the presumption that the `r-th` bit of `xj` is `0`. Therefore we conclude the `r-th` bit of `xj` will always be the same as the `r-th` bit of the single number as long as `p'j = 1`. Since this is true for all bits in `xj` (i.e., true for `r = 1` to `32`), we conclude `xj` will equal the single element as long as `p'j = 1`.

So now it's clear what we should return. Just express `p' = p % k` in its binary form and return any of the corresponding `xj` as long as `p'j = 1`. In total, the algorithm will run in `O(n * logk)` time and `O(logk)` space.

> **Side note**: There is a general formula relating each bit of `xj` to `p'j` and each bit of the single number `s`, which is given by `(xj)_r = s_r & p'j`, with (xj)_r and `s_r` denoting respectively the `r-th` bit of `xj` and the single number `s`. From this formula, it's easy to see that `(xj)_r = s_r` if `p'j = 1`, that is, `**xj = s as long as `p'j = 1`**, as shown above. Furthermore, we have `(xj)_r = 0` if `p'j = 0`, regardless of the value of the single number, that is, `xj = 0` as long as `p'j = 0`. So in summary we obtain: `xj = s` if `p'j = 1`, and `xj = 0` if `p'j = 0`. This implies the expression (`x1 | x2 | ... | xm`) will also be evaluated to the single number `s`, since the expression will essentially take the `OR` operations of the single number with itself and some `0`s, which boils down to the single number eventually.

------

###### V -- Quick examples

Here is a list of few quick examples to show how the algorithm works (you can easily come up with other examples):



1. `k = 2, p = 1`
   `k` is `2`, then `m = 1`, we need only one 32-bit integer (`x1`) as the counter. And `2^m = k` so we do not even need a mask! A complete python program will look like:

```python
def singleNumber(nums):
	x1 = 0;
    for i in nums:
    	x1 ^= i
    return x1
```

1. k = 3, p = 1

   k is 3, then `m = 2`, we need two 32-bit integers(`x2`, `x1`) as the counter. And `2^m > k` so we do need a mask. Write `k` in its binary form: `k = '11'`, then `k1 = 1`, `k2 = 1`, so we have `mask = ~(x1 & x2)`. A complete python program will look like:

```python
def singleNumber(nums):
    """
    k=11 :影响mask
    p=1  :影响返回值
    """
	if not nums:
        return -1
    x1 = 0
    x2 = 0
    mask = 0
    for num in nums:
        x2 ^= (x1 & num)
        x1 ^= num
        mask = ~(x1 & x2)
        x2 &= mask
        x1 &= mask
   
        return x1  # Since p = 1, in binary form p = '01', then p1 = 1, so we should return x1. 
                   # If p = 2, in binary form p = '10', then p2 = 1, and we should return x2.
                   # Or alternatively we can simply return (x1 | x2).

```

1. k = 5, p = 3

   k  is  5, then m = 3, we need three 32-bit integers(`x3`, `x2`, `x1`) as the counter. And `2^m > k` so we need a mask. Write `k` in its binary form: `k = '101'`, then `k1 = 1`, `k2 = 0`, `k3 = 1`, so we have `mask = ~(x1 & ~x2 & x3)`. A complete java program will look like:

```python
def singleNumber(nums):
    """
    k=101 :影响mask
    p=011  :影响返回值
    """
	if not nums:
        return -1
    x1 = 0
    x2 = 0
    x3 = 0
    mask = 0
    for num in nums:
        x3 ^= (x2 & x1 & num)
        x2 ^= (x1 & num)
        x1 ^= num
        mask = ~(x1 & ~x2 & x3)
        x2 &= mask
        x1 &= mask
   
    return x1  # Since p = 3, in binary form p = '011', then p1 = p2 = 1, so we can return either x1 or x2. 
                   # If p = 4, in binary form p = '100', only p3 = 1, which implies we can only return x3.
                   # Or alternatively we can simply return (x1 | x2 | x3).
```



## 剑指Offer面试题: Python实现

第2章 面试基础知识
2.2 编程语言
面试题2 使用Python实现单例模式
2.3 数据结构
面试题3 二维数组中的查找

从右上角逐列递减，逐行递增

面试题4 替换空格

逆向遍历，并入栈，面试题5 从尾到头打印单链表

面试题6 重建二叉树
面试题7 用两个栈实现队列
2.4 算法和数据操作
面试题8 旋转数组的最小数字
面试题9 斐波那契数列

- 变态青蛙递推：

  ```
  一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。
  ```

  F(n)代表斐波那契数列第n项

  f(n)代表跳上n阶台阶需要多少步



 $f(0)=F(1) = 1$

$f(1)=f(0)=F(1)=F(2)=1$
$\begin{split}f(2)&=f(1)+f(0)\\&=2\cdot f(1)\end{split}$

$\begin{split}f(3)&=f(2)+f(1)+f(0)\\&=F(3)+F(2)+F(1)\\&=(F(2)+F(1))+F(2)+F(1)\\&=2\cdot(F(2)+F(1))\\&=2\cdot f(2)\end{split}$



$\begin{split}f(4)&=f(3)+f(2)+f(1)+f(0)\\&=f(3)+f(3)\\&=2\cdot f(3)\end{split}$

$f(n) = 2*f(n-1) \ ,n\ge2$







面试题10 二进制中1的个数
第3章 高质量代码
3.3 代码的完整性
面试题11 数值的整数次方
面试题12 打印1到最大的n位数
面试题13 O(1)时间删除链表结点
面试题14 调整数组顺序使寄数位于偶数前面
3.4 代码的鲁棒性
面试题15 链表中倒数第k个结点
面试题16 反转链表
面试题17 合并两个排序的链表
面试题18 树的子结构
第4章 解决面试题思路
4.2 画图让抽象问题形象化
面试题19 二叉树的镜像
面试题20 顺时针打印矩阵
4.3 举例让抽象问题具体化
面试题21 包含min函数的栈
面试题22 栈的压入弹出序列
面试题23 从上往下打印二叉树
面试题24 二叉树的后序遍历序列
面试题25 二叉树中和为某一值的路径
4.4 分解让复杂问题简单化
面试题26 复杂链表的复制
面试题27 二叉搜索树与双向链表
面试题28 字符串的排列
第5章 优化时间和空间效率
5.2 时间效率
面试题29 数组中出现次数超过一半的数字
面试题30 最小的k个数
面试题31 连续子数组的最大和
面试题32 从1到n整数中1出现的次数
面试题33 把数组排成最小的数
5.3 时间效率与空间效率的平衡
面试题34 丑数
面试题35 第一个只出现一次的字符
面试题36 数组中的逆序对
面试题37 两个链表的第一个公共结点
第6章 面试能力
6.3 知识迁移能力
面试题38 数字在排序数组中出现的次数
面试题39 二叉树的深度
面试题40 数组中只出现一次的数字
面试题41 和为s的两个数字VS和为s的连续正数序列
面试题42 翻转单词顺序与左旋转字符串
6.4 抽象建模能力
面试题43 n个骰子的点数
面试题44 扑克牌的顺子
面试题45 圆圈中最后剩下的数字
6.5 发散思维能力
面试题46 求1+2...+n
面试题47 不用加减乘除做加法
面试题48 不能被继承的类
第7章 面试案例
7.1 案例一
面试题49 把字符串转化成整数
7.2 案例二
面试题50 树中两个结点的最低公共祖先

