## 动态规划

动态规划实质上是一种优化技术，动态规划的思想为：**建立详细的最优解表**。先从简单的子问题开始。通过较小问题的最优解创建越来越大问题的最优解。

### 动态规划和分治：

动态规划是分治范式的延伸，只有当问题具有明确限制和先决条件时，才能对问题采用动态规划方法。

这两个条件是：

- 问题的最优解由子问题的最优解构成
- 问题可以被分解为子问题，这些子问题可以重复使用或者可以使用递归算法解决而不会产生新的子问题

如果满足这两个限制，那么分治问题就可以采用动态规划的方法完成，而与之不同，分治是将问题划分为互不相交的子问题

> 二分查找是一个典型的分治问题，但二分查找没有重叠子问题，不是动态规划问题。

动态规划的两种技术(**memoization** and **tabulation**) 

- memoization(自顶向下)

  **Memoization (top-down cache filling)** refers to the technique of caching and reusing previously computed results

  ```python
  def fib(n, mem: dict):
      if n = 0 or n = 1:
          return n
      if n not in mem:
          mem[n] = fib(n - 1, mem) + fib(n - 2, mem)
      return mum[n]
  ```

  简单理解就是递归实现，从问题本身开始，分解为子问题

- tabulation(自底向上)

  **Tabulation (bottom-up cache filling)** is similar but focuses on filling the entries of the cache. Computing the values in the cache is easiest done iteratively. 

  ```python
  def fib(n, mem: dict):
      mem[0] = 0
      mem[1] = 1
      for i in range(2, n+1):
          mem[i] = mem[i-1] + mem[i-2]
      return mem[n]
  ```

  循环实现，从最小的子问题的解开始，循环迭代到需要解决的最终问题

缓存有效的原因是分治问题里有重叠的子问题

### 解决动态规划问题的步骤

> [参考](http://blog.refdash.com/dynamic-programming-tutorial-example/)

- 识别动态规划问题

- 确定问题变量

- 清楚的表达递归关系

  假定已经完成了所有的子问题之后，如何解决主要的问题

- 确定基案本例(base case)

  不依赖任何子问题的最小子问题

  确定这个问题之前，需要想清楚主要的问题是如何简化为更小的问题，以及在什么时候问题不能被进一步简化

  问题不能被简化的原因——其中一个参数由于问题的约束，变成了一个不可能的值，即确定边界条件

- 决定是要用迭代还是递归实现

- **添加缓存（自顶向下）**

- 确定时间复杂度

  有一些简单的规则可以使计算动态编程问题的计算时间复杂性容易得多(whar are they)

### reference

最优二叉查找树

- 包含n个健的二叉查找树的总数量等于第n个卡特兰数
  $$
  当n>0时，c(n)=\cfrac{1}{n+1}[_n^{2n}],c(0)=1
  $$

  > 卡特兰数:不能穿过对角线意味着任意时间向右的步数不能小于向上的步数，向右的步数记为左括号，向上的步数记为右括号



### 典型问题

通常我们会从斐波那契数列开始谈动态规划问题，但这次为了更深入的理解动态规划，我们来点不一样的，从最小编辑距离开始看吧。

- 最小编辑距离

  首先简单描述“编辑距离”，两个字符串，将a转化成b，采用插入、删除、和替换3种变换方式，最少的变化次数称为“编辑距离”，以下问题都是将a转化成b，且矩阵a的索引i作为行标，b的索引j作为列标，那么我们可以想象，一种操作次数最长的方式就是，从尾到头删除a，再从头到尾增加b,实际操作过程中，我们需要保存一个决策矩阵，矩阵元素存在一个递推关系



- 斐波那契数列及其变体青蛙跳台阶问题
- 背包问题(#jump)

在算法导论中的入门题目钢条切割，对于该问题，

**参考**

[Dynamic Programming: First Principles](http://www.flawlessrhetoric.com/Dynamic-Programming-First-Principles)

[利润最大化](https://lukasmericle.github.io/dynprotut/)



