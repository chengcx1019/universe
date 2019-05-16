## 动态规划

动态规划实质上是一种优化技术，动态规划的思想为：**建立详细的最优解表**。先从简单的子问题开始。通过较小问题的最优解创建越来越大问题的最优解。

### 动态规划和分治：

动态规划是分治范式的延伸，只有当问题具有明确限制和先决条件时，才能对问题采用动态规划方法。

这两个条件是：

- 问题的最优解由子问题的最优解构成
- 问题可以被分解为子问题，这些子问题可以重复使用或者可以使用递归算法解决而不会产生新的子问题

如果满足这两个限制，那么分治问题就可以采用动态规划的方法完成。

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

- 包含n个健的二叉查找树的总数量等于第n个卡塔兰树
  $$
  当n>0时，c(n)=\cfrac{1}{n+1}[_n^{2n}],c(0)=1
  $$

  - 卡特兰数

    不能穿过对角线意味着任意时间向右的步数不能小于向左的步数，向右的步数记为左括号，向上的步数记为右括号

### 典型问题

通常我们会从斐波那契数列开始谈动态规划问题，但这次为了更深入的理解动态规划，我们来点不一样的，从最小编辑距离开始看吧。

- 最小编辑距离

  首先简单描述“编辑距离”，两个字符串，将a转化成b，采用插入、删除、和替换3种变换方式，最少的变化次数称为“编辑距离”，以下问题都是将a转化成b，且矩阵a的索引i作为行标，b的索引j作为列标，那么我们可以想象，一种操作次数最长的方式就是，从尾到头删除a，再从头到尾增加b

  实际操作过程中，我们需要保存一个决策矩阵，矩阵元素存在一个递推关系


- 斐波那契数列及其变体青蛙跳台阶问题
- 背包问题(#jump)

### LeetCode动态规划专题



| seq  | #                                                            |      | Title                                                        | Acceptance | Difficulty |      |
| ---- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ---------- | ---------- | ---- |
| 1    | <a href='#Longest Valid Parentheses'>   32</a>               |      | [Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses) | 23.80%     | Hard       | -    |
| 2    | <a href='#Unique Paths II'>63</a>                            |      | [Unique Paths II](https://leetcode.com/problems/unique-paths-ii) | 32.60%     | Medium     | -    |
| 3    | <a href='#Minimum Path Sum'>64</a>                           |      | [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum) | 42.90%     | Medium     | -    |
| 4    | <a href='#Edit Distance'>72</a>                              |      | [Edit Distance](https://leetcode.com/problems/edit-distance) | 34.20%     | Hard       | -    |
| 5    | <a href='#Scramble String'>87</a>                            |      | [Scramble String](https://leetcode.com/problems/scramble-string) | 30.30%     | Hard       | -    |
| 6    | <a href='#Unique Binary Search Trees II'>95</a>              |      | [Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii) | 33.30%     | Medium     |      |
| 7    | <a href='#Interleaving String'>97</a>                        |      | [Interleaving String](https://leetcode.com/problems/interleaving-string) | 26.00%     | Hard       |      |
| 8    | <a href='#Distinct Subsequences'>115</a>                     |      | [Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences) | 33.10%     | Hard       |      |
| 9    | <a href='#Triangle'>120</a>                                  |      | [Triangle](https://leetcode.com/problems/triangle)           | 36.40%     | Medium     |      |
| 10   | <a href='#Best Time to Buy and Sell Stock   III'>123</a>     |      | [Best Time to Buy and Sell   Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii) | 31.40%     | Hard       |      |
| 11   | <a href='#Palindrome Partitioning II'>132</a>                |      | [Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii) | 25.70%     | Hard       |      |
| 12   | <a href='#Maximum Product Subarray'>152</a>                  |      | [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray) | 27.50%     | Medium     |      |
| 13   | <a href='#Best Time to Buy and Sell Stock   IV'>188</a>      |      | [Best Time to Buy and Sell   Stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv) | 25.30%     | Hard       |      |
| 14   | <a href='#House Robber'>198</a>                              |      | [House Robber](https://leetcode.com/problems/house-robber)   | 40.40%     | Easy       |      |
| 15   | <a href='#House Robber II'>213</a>                           |      | [House Robber II](https://leetcode.com/problems/house-robber-ii) | 34.70%     | Medium     |      |
| 16   | <a href='#Paint House'>256</a>                               |      | [Paint House](https://leetcode.com/problems/paint-house)     | 46.90%     | Easy       |      |
| 17   | <a href='#Ugly Number II'>264</a>                            |      | [Ugly Number II](https://leetcode.com/problems/ugly-number-ii) | 34.20%     | Medium     |      |
| 18   | <a href='#Paint House II'>265</a>                            |      | [Paint House II](https://leetcode.com/problems/paint-house-ii) | 39.30%     | Hard       |      |
| 19   | <a href='#Paint Fence'>276</a>                               |      | [Paint Fence](https://leetcode.com/problems/paint-fence)     | 35.40%     | Easy       |      |
| 20   | <a href='#Perfect Squares'>279</a>                           |      | [Perfect Squares](https://leetcode.com/problems/perfect-squares) | 38.80%     | Medium     |      |
| 21   | <a href='#Longest Increasing Subsequence'>300</a>            |      | [Longest Increasing   Subsequence](https://leetcode.com/problems/longest-increasing-subsequence) | 39.30%     | Medium     |      |
| 22   | <a href='#Range Sum Query - Immutable'>303</a>               |      | [Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable) | 34.10%     | Easy       |      |
| 23   | <a href='#Range Sum Query 2D - Immutable'>304</a>            |      | [Range Sum Query 2D -   Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable) | 28.80%     | Medium     |      |
| 24   | <a href='#Best Time to   Buy and Sell Stock with Cooldown'>309</a> |      | [Best Time to Buy and Sell   Stock with Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown) | 42.70%     | Medium     |      |
| 25   | <a href='#Create Maximum Number'>321</a>                     |      | [Create Maximum Number](https://leetcode.com/problems/create-maximum-number) | 24.90%     | Hard       |      |
| 26   | <a href='#Coin Change'>322</a>                               |      | [Coin Change](https://leetcode.com/problems/coin-change)     | 27.30%     | Medium     |      |
| 27   | <a href='#Counting Bits'>338</a>                             |      | [Counting Bits](https://leetcode.com/problems/counting-bits) | 62.90%     | Medium     |      |
| 28   | <a href='#Integer Break'>343</a>                             |      | [Integer Break](https://leetcode.com/problems/integer-break) | 46.70%     | Medium     |      |
| 29   | <a href='#Maximal Rectangle'>85</a>                          |      | [Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle) | 30.80%     | Hard       |      |
| 30   | <a href='#Maximal Square'>221</a>                            |      | [Maximal Square](https://leetcode.com/problems/maximal-square) | 31.20%     | Medium     |      |
| 31   | <a href='#Best Time to Buy and Sell Stock'>121</a>           |      | [Best Time to Buy and Sell   Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock) | 44.30%     | Easy       |      |
| 32   | <a href='#Dungeon Game'>174</a>                              |      | [Dungeon Game](https://leetcode.com/problems/dungeon-game)   | 25.20%     | Hard       |      |
| 33   | <a href='#Unique Binary Search Trees'>96</a>                 |      | [Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees) | 43.30%     | Medium     |      |
| 34   | <a href='#Decode Ways'>91</a>                                |      | [Decode Ways](https://leetcode.com/problems/decode-ways)     | 21.00%     | Medium     |      |
| 35   | <a href='#Regular Expression Matching'>10</a>                |      | [Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching) | 24.40%     | Hard       |      |
| 36   | <a href='#Unique Paths'>62</a>                               |      | [Unique Paths](https://leetcode.com/problems/unique-paths)   | 44.40%     | Medium     |      |
| 37   | <a href='#Maximum Subarray'>53</a>                           |      | [Maximum Subarray](https://leetcode.com/problems/maximum-subarray) | 41.20%     | Easy       |      |
| 38   | <a href='#Climbing Stairs'>70</a>                            |      | [Climbing Stairs](https://leetcode.com/problems/climbing-stairs) | 42.00%     | Easy       |      |
| 39   | <a href='#Word Break II'>140</a>                             |      | [Word Break II](https://leetcode.com/problems/word-break-ii) | 25.30%     | Hard       |      |
| 40   | <a href='#Russian Doll Envelopes'>354</a>                    |      | [Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes) | 32.80%     | Hard       |      |
| 41   | <a href='#Android Unlock Patterns'>351</a>                   |      | [Android Unlock Patterns](https://leetcode.com/problems/android-unlock-patterns) | 44.70%     | Medium     |      |
| 42   | <a href='#Count Numbers with Unique   Digits'>357</a>        |      | [Count Numbers with Unique   Digits](https://leetcode.com/problems/count-numbers-with-unique-digits) | 46.20%     | Medium     |      |
| 43   | <a href='#Bomb Enemy'>361</a>                                |      | [Bomb Enemy](https://leetcode.com/problems/bomb-enemy)       | 41.20%     | Medium     |      |
| 44   | <a href='#Max Sum of Rectangle No Larger Than   K'>363</a>   |      | [Max Sum of Rectangle No   Larger Than K](https://leetcode.com/problems/max-sum-of-rectangle-no-larger-than-k) | 34.20%     | Hard       |      |
| 45   | <a href='#Largest Divisible Subset'>368</a>                  |      | [Largest Divisible Subset](https://leetcode.com/problems/largest-divisible-subset) | 34.00%     | Medium     |      |
| 46   | <a href='#Guess Number Higher or Lower II'>375</a>           |      | [Guess Number Higher or Lower   II](https://leetcode.com/problems/guess-number-higher-or-lower-ii) | 36.30%     | Medium     |      |
| 47   | <a href='#Wiggle Subsequence'>376</a>                        |      | [Wiggle Subsequence](https://leetcode.com/problems/wiggle-subsequence) | 36.30%     | Medium     |      |
| 48   | <a href='#Combination Sum IV'>377</a>                        |      | [Combination Sum IV](https://leetcode.com/problems/combination-sum-iv) | 43.20%     | Medium     |      |
| 49   | <a href='#Is Subsequence'>392</a>                            |      | [Is Subsequence](https://leetcode.com/problems/is-subsequence) | 45.00%     | Medium     |      |
| 50   | <a href='#Burst Balloons'>312</a>                            |      | [Burst Balloons](https://leetcode.com/problems/burst-balloons) | 44.50%     | Hard       |      |
| 51   | <a href='#Frog Jump'>403</a>                                 |      | [Frog Jump](https://leetcode.com/problems/frog-jump)         | 33.50%     | Hard       |      |
| 52   | <a href='#Partition Equal Subset Sum'>416</a>                |      | [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum) | 38.70%     | Medium     |      |
| 53   | <a href='#Sentence Screen Fitting'>418</a>                   |      | [Sentence Screen Fitting](https://leetcode.com/problems/sentence-screen-fitting) | 28.90%     | Medium     |      |
| 54   | <a href='#Split Array Largest Sum'>410</a>                   |      | [Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum) | 40.20%     | Hard       |      |
| 55   | <a href='#Wildcard Matching'>44</a>                          |      | [Wildcard Matching](https://leetcode.com/problems/wildcard-matching) | 21.60%     | Hard       |      |
| 56   | <a href='#Arithmetic Slices'>413</a>                         |      | [Arithmetic Slices](https://leetcode.com/problems/arithmetic-slices) | 54.50%     | Medium     |      |
| 57   | <a href='#Arithmetic Slices II -   Subsequence'>446</a>      |      | [Arithmetic Slices II -   Subsequence](https://leetcode.com/problems/arithmetic-slices-ii-subsequence) | 28.00%     | Hard       |      |
| 58   | <a href='#Can I Win'>464</a>                                 |      | [Can I Win](https://leetcode.com/problems/can-i-win)         | 26.00%     | Medium     |      |
| 59   | <a href='#Unique Substrings in Wraparound   String'>467</a>  |      | [Unique Substrings in   Wraparound String](https://leetcode.com/problems/unique-substrings-in-wraparound-string) | 33.20%     | Medium     |      |
| 60   | <a href='#Count The Repetitions'>466</a>                     |      | [Count The Repetitions](https://leetcode.com/problems/count-the-repetitions) | 27.20%     | Hard       |      |
| 61   | <a href='#Encode String with Shortest   Length'>471</a>      |      | [Encode String with Shortest   Length](https://leetcode.com/problems/encode-string-with-shortest-length) | 43.70%     | Hard       |      |
| 62   | <a href='#Ones and Zeroes'>474</a>                           |      | [Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes) | 38.70%     | Medium     |      |
| 63   | <a href='#Concatenated Words'>472</a>                        |      | [Concatenated Words](https://leetcode.com/problems/concatenated-words) | 32.00%     | Hard       |      |
| 64   | <a href='#Word Break'>139</a>                                |      | [Word Break](https://leetcode.com/problems/word-break)       | 32.60%     | Medium     |      |
| 65   | <a href='#Target Sum'>494</a>                                |      | [Target Sum](https://leetcode.com/problems/target-sum)       | 44.00%     | Medium     |      |
| 66   | <a href='#Predict the Winner'>486</a>                        |      | [Predict the Winner](https://leetcode.com/problems/predict-the-winner) | 45.60%     | Medium     |      |
| 67   | <a href='#Longest Palindromic Subsequence'>516</a>           |      | [Longest Palindromic   Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence) | 43.50%     | Medium     |      |
| 68   | <a href='#Super Washing Machines'>517</a>                    |      | [Super Washing Machines](https://leetcode.com/problems/super-washing-machines) | 36.30%     | Hard       |      |
| 69   | <a href='#Continuous Subarray Sum'>523</a>                   |      | [Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum) | 23.40%     | Medium     |      |
| 70   | <a href='#Remove Boxes'>546</a>                              |      | [Remove Boxes](https://leetcode.com/problems/remove-boxes)   | 36.90%     | Hard       |      |
| 71   | <a href='#Freedom Trail'>514</a>                             |      | [Freedom Trail](https://leetcode.com/problems/freedom-trail) | 39.60%     | Hard       |      |
| 72   | <a href='#Student Attendance Record II'>552</a>              |      | [Student Attendance Record II](https://leetcode.com/problems/student-attendance-record-ii) | 31.60%     | Hard       |      |
| 73   | <a href='#Maximum Vacation Days'>568</a>                     |      | [Maximum Vacation Days](https://leetcode.com/problems/maximum-vacation-days) | 36.10%     | Hard       |      |
| 74   | <a href='#Out of Boundary Paths'>576</a>                     |      | [Out of Boundary Paths](https://leetcode.com/problems/out-of-boundary-paths) | 30.70%     | Medium     |      |
| 75   | <a href='#Non-negative   Integers without Consecutive Ones'>600</a> |      | [Non-negative Integers without   Consecutive Ones](https://leetcode.com/problems/non-negative-integers-without-consecutive-ones) | 31.70%     | Hard       |      |
| 76   | <a href='#K Inverse Pairs Array'>629</a>                     |      | [K Inverse Pairs Array](https://leetcode.com/problems/k-inverse-pairs-array) | 27.90%     | Hard       |      |
| 77   | <a href='#Shopping Offers'>638</a>                           |      | [Shopping Offers](https://leetcode.com/problems/shopping-offers) | 46.20%     | Medium     |      |
| 78   | <a href='#Decode Ways II'>639</a>                            |      | [Decode Ways II](https://leetcode.com/problems/decode-ways-ii) | 24.20%     | Hard       |      |
| 79   | <a href='#Maximum Length of Pair Chain'>646</a>              |      | [Maximum Length of Pair Chain](https://leetcode.com/problems/maximum-length-of-pair-chain) | 47.30%     | Medium     |      |
| 80   | <a href='#Palindromic Substrings'>647</a>                    |      | [Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings) | 54.50%     | Medium     |      |
| 81   | <a href='#2 Keys Keyboard'>650</a>                           |      | [2 Keys Keyboard](https://leetcode.com/problems/2-keys-keyboard) | 45.90%     | Medium     |      |
| 82   | <a href='#4 Keys Keyboard'>651</a>                           |      | [4 Keys Keyboard](https://leetcode.com/problems/4-keys-keyboard) | 49.80%     | Medium     |      |
| 83   | <a href='#Coin Path'>656</a>                                 |      | [Coin Path](https://leetcode.com/problems/coin-path)         | 25.80%     | Hard       |      |
| 84   | <a href='#Strange Printer'>664</a>                           |      | [Strange Printer](https://leetcode.com/problems/strange-printer) | 34.90%     | Hard       |      |
| 85   | <a href='#Number of   Longest Increasing Subsequence'>673</a> |      | [Number of Longest Increasing   Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence) | 32.00%     | Medium     |      |
| 86   | <a href='#Maximum Sum   of 3 Non-Overlapping Subarrays'>689</a> |      | [Maximum Sum of 3   Non-Overlapping Subarrays](https://leetcode.com/problems/maximum-sum-of-3-non-overlapping-subarrays) | 42.00%     | Hard       |      |
| 87   | <a href='#Knight Probability in   Chessboard'>688</a>        |      | [Knight Probability in   Chessboard](https://leetcode.com/problems/knight-probability-in-chessboard) | 40.80%     | Medium     |      |
| 88   | <a href='#Stickers to Spell Word'>691</a>                    |      | [Stickers to Spell Word](https://leetcode.com/problems/stickers-to-spell-word) | 36.00%     | Hard       |      |
| 89   | <a href='#Partition to K Equal Sum   Subsets'>698</a>        |      | [Partition to K Equal Sum   Subsets](https://leetcode.com/problems/partition-to-k-equal-sum-subsets) | 38.70%     | Medium     |      |
| 90   | <a href='#Minimum   ASCII Delete Sum for Two Strings'>712</a> |      | [Minimum ASCII Delete Sum for   Two Strings](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings) | 52.00%     | Medium     |      |
| 91   | <a href='#Best Time to   Buy and Sell Stock with Transaction Fee'>714</a> |      | [Best Time to Buy and Sell   Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee) | 47.70%     | Medium     |      |
| 92   | <a href='#Maximum Length of Repeated   Subarray'>718</a>     |      | [Maximum Length of Repeated   Subarray](https://leetcode.com/problems/maximum-length-of-repeated-subarray) | 42.40%     | Medium     |      |
| 93   | <a href='#Minimum Window Subsequence'>727</a>                |      | [Minimum Window Subsequence](https://leetcode.com/problems/minimum-window-subsequence) | 32.80%     | Hard       |      |
| 94   | <a href='#Count   Different Palindromic Subsequences'>730</a> |      | [Count Different Palindromic   Subsequences](https://leetcode.com/problems/count-different-palindromic-subsequences) | 36.50%     | Hard       |      |
| 95   | <a href='#Cherry Pickup'>741</a>                             |      | [Cherry Pickup](https://leetcode.com/problems/cherry-pickup) | 24.80%     | Hard       |      |
| 96   | <a href='#Delete and Earn'>740</a>                           |      | [Delete and Earn](https://leetcode.com/problems/delete-and-earn) | 44.20%     | Medium     |      |
| 97   | <a href='#Min Cost Climbing Stairs'>746</a>                  |      | [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs) | 44.00%     | Easy       |      |
| 98   | <a href='#Number Of Corner Rectangles'>750</a>               |      | [Number Of Corner Rectangles](https://leetcode.com/problems/number-of-corner-rectangles) | 59.00%     | Medium     |      |
| 99   | <a href='#Largest Plus Sign'>764</a>                         |      | [Largest Plus Sign](https://leetcode.com/problems/largest-plus-sign) | 41.30%     | Medium     |      |
| 100  | <a href='#Longest Palindromic Substring'>5</a>               |      | [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring) | 25.60%     | Medium     |      |
| 101  | <a href='#Domino and Tromino Tiling'>790</a>                 |      | [Domino and Tromino Tiling](https://leetcode.com/problems/domino-and-tromino-tiling) | 33.70%     | Medium     |      |
| 102  | <a href='#Cheapest Flights Within K Stops'>787</a>           |      | [Cheapest Flights Within K   Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops) | 30.30%     | Medium     |      |
| 103  | <a href='#Minimum   Swaps To Make Sequences Increasing'>801</a> |      | [Minimum Swaps To Make   Sequences Increasing](https://leetcode.com/problems/minimum-swaps-to-make-sequences-increasing) | 32.20%     | Medium     |      |
| 104  | <a href='#Soup Servings'>808</a>                             |      | [Soup Servings](https://leetcode.com/problems/soup-servings) | 34.30%     | Medium     |      |
| 105  | <a href='#Largest Sum of Averages'>813</a>                   |      | [Largest Sum of Averages](https://leetcode.com/problems/largest-sum-of-averages) | 42.10%     | Medium     |      |
| 106  | <a href='#Race Car'>818</a>                                  |      | [Race Car](https://leetcode.com/problems/race-car)           | 29.60%     | Hard       |      |
| 107  | <a href='#New 21 Game'>837</a>                               |      | [New 21 Game](https://leetcode.com/problems/new-21-game)     | 26.00%     | Medium     |      |
| 108  | <a href='#Push Dominoes'>838</a>                             |      | [Push Dominoes](https://leetcode.com/problems/push-dominoes) | 41.40%     | Medium     |      |
| 109  | <a href='#Shortest Path Visiting All   Nodes'>847</a>        |      | [Shortest Path Visiting All   Nodes](https://leetcode.com/problems/shortest-path-visiting-all-nodes) | 43.10%     | Hard       |      |
| 110  | <a href='#Minimum Number of Refueling   Stops'>871</a>       |      | [Minimum Number of Refueling   Stops](https://leetcode.com/problems/minimum-number-of-refueling-stops) | 25.90%     | Hard       |      |
| 111  | <a href='#Length of   Longest Fibonacci Subsequence'>873</a> |      | [Length of Longest Fibonacci   Subsequence](https://leetcode.com/problems/length-of-longest-fibonacci-subsequence) | 41.90%     | Medium     |      |
| 112  | <a href='#Stone Game'>877</a>                                |      | [Stone Game](https://leetcode.com/problems/stone-game)       | 56.60%     | Medium     |      |
| 113  | <a href='#Profitable Schemes'>879</a>                        |      | [Profitable Schemes](https://leetcode.com/problems/profitable-schemes) | 31.90%     | Hard       |      |
| 114  | <a href='#Super Egg Drop'>887</a>                            |      | [Super Egg Drop](https://leetcode.com/problems/super-egg-drop) | 22.20%     | Hard       |      |
| 115  | <a href='#Bitwise ORs of Subarrays'>898</a>                  |      | [Bitwise ORs of Subarrays](https://leetcode.com/problems/bitwise-ors-of-subarrays) | 29.20%     | Medium     |      |
| 116  | <a href='#Numbers At Most N Given Digit   Set'>902</a>       |      | [Numbers At Most N Given Digit   Set](https://leetcode.com/problems/numbers-at-most-n-given-digit-set) | 25.10%     | Hard       |      |
| 117  | <a href='#Valid Permutations for DI   Sequence'>903</a>      |      | [Valid Permutations for DI   Sequence](https://leetcode.com/problems/valid-permutations-for-di-sequence) | 38.70%     | Hard       |      |

以下题目仅描述递归的解法：

##### Longest Valid Parentheses

"(()"-2

 ")()())"-4

额外使用一个数组d记录序列每一个位置当前的最大有效长度，以“()(())”为例

| 0    | 1    | 2    | 3    | 4    | 5    |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 0    | 2    | 0    | 0    | 2    | 4    |

从序列的第2个元素开始，直到遇到'')''记此时索引为c，这时会有两种情况：

- 当c-1是‘(’，那么可以确定这一组有效的，且如果从c>2的话，那么就是$d[c]=d[c-2]+2$，否则就是2
- 当c-1是‘)’，我们就需要排除掉c-1所对应的有效序列d[c-1]之前的那一位是不是'(',如果不是，就取0，如果是
  - 如果$c-d[c-1]-2 >0$,则$d[c] = d[c-1] + d[c-d[c-1]-2] + 2$
  - 否则$d[c] = d[c-1] + 2$

```python
class Solution(object):	
    def method3(self, s):
        """
        dp方法，是我想要能想到的方法
        创建数组记录每一个位置当前的最大有效长度
        """
        if not s:
            return 0
        d = [0] * len(s)
        ansmax = 0
        for c in range(1, len(s)):
            if s[c] == ')':
                if s[c-1] == '(':
                    d[c] = (d[c-2] if c > 2 else 0) + 2
                elif c - d[c-1] > 0 and s[c-d[c-1]-1] == '(':
                    d[c] = d[c-1] + ((d[c-d[c-1]-2]  if c-d[c-1]-2 > 0 else 0 ) + 2)
                ansmax=max(ansmax, d[c])
        return ansmax
```



##### Unique Paths II

和机器人收集硬币是一类问题，采用二维数据存储实时状态，将有障碍的位置初始为0，[c,h]的更新依赖于[c-1,h]和[c,h-1]的和

```python
class Solution(object):
    def method2(self, obstacleGrid):
        """
        :type obstacleGrid: List[List[int]]
        :rtype: int
        用例：[[1, 0]],[[0, 1]]
        """
        if not obstacleGrid:
            return 0
        height = len(obstacleGrid)
        width = len(obstacleGrid[0])
        paths = [[0]*width for _ in range(height)]
        if obstacleGrid[0][0] != 1:
            paths[0][0] = 1
        
        for c in range(height):
            for h in range(width):
                if obstacleGrid[c][h] != 1:
                    if c > 0:
                        paths[c][h] += paths[c-1][h] 
                    if h > 0:
                        paths[c][h] += paths[c][h-1]
        return paths[-1][-1]

```



##### Minimum Path Sum

和机器人收集纪念币同样思路

```python
class Solution:
    def minPathSum(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        if not grid:
            return 0
        height = len(grid)
        width = len(grid[0])
        sums = [[0]*width for _ in range(height)]
        sums[0][0] = grid[0][0]
        for c in range(height):
            for h in range(width):
                if c > 0 and h > 0:
                    sums[c][h] = min(sums[c-1][h], sums[c][h-1]) + grid[c][h]
                elif c > 0:
                    sums[c][h] = sums[c-1][h] + grid[c][h]
                elif h > 0:
                    sums[c][h] = sums[c][h-1] + grid[c][h]
        return sums[-1][-1]
```



##### Edit Distance

我们定义3种操作，插入，删除和替换，对于将word1转化为word2，我一般习惯把word2放在列位置，记位置标记[c, h]，那么对于[c, h-1]执行的操作是添加，[c-1， h]执行的操作是删除，对于[c-1, h-1]，根据word1[c-1]和word2[h-1]是否相同添加相应的操作

```python
class Solution:
    def minDistance(self, word1, word2):
        """
        :type word1: str
        :type word2: str
        :rtype: int
        """
        return self.method1(word1, word2)
        
        
    def method1(self, word1, word2):
        """
        对于将word1转化为word2，我一般习惯把word2放在列位置，记位置标记[c, h]
        那么对于[c, h-1]执行的操作是添加，[c-1， h]执行的操作是删除，
        """
        width = len(word2) + 1
        height = len(word1) + 1
        margin = [[0]*width for _ in range(height)]
        for c in range(width):
            margin[0][c] = c
        for c in range(1, height):
            margin[c][0] = c
        # 按行遍历
        # for c in range(1, height):
        #     for h in range(1, width):
        #         indicator = 0 if word1[c-1] == word2[h-1] else 1
        #         margin[c][h] = min(margin[c-1][h] + 1,  # deletion
        #                            margin[c][h-1] + 1,  # insertion
        #                            margin[c-1][h-1] + indicator)
        # 按列遍历
        for c in range(1, width):
            for h in range(1, height):
                indicator = 0 if word1[h-1] == word2[c-1] else 1
                margin[h][c] = min(margin[h-1][c]+1,
                                  margin[h][c-1]+1,
                                  margin[h-1][c-1]+indicator)
        return margin[-1][-1]
```

以将Sartuday 转化为Sunday为例(按列遍历,这样对于任意的列字符，都可以自底向前追溯操作过程，更好理解)：

|      |      | S     | u     | n     | d     | a     | y     |
| ---- | ---- | ----- | ----- | ----- | ----- | ----- | ----- |
|      | 0    | 1     | 2     | 3     | 4     | 5     | 6     |
| S    | 1    | **0** | 1     | 2     | 3     | 4     | 5     |
| a    | 2    | **1** | 1     | 2     | 3     | 4     | 5     |
| t    | 3    | **2** | 2     | 2     | 3     | 4     | 5     |
| u    | 4    | 3     | **2** | 3     | 3     | 4     | 5     |
| r    | 5    | 4     | 3     | **3** | 4     | 4     | 5     |
| d    | 6    | 5     | 4     | 4     | **3** | 4     | 5     |
| a    | 7    | 6     | 5     | 5     | 4     | **3** | 4     |
| y    | 8    | 7     | 6     | 6     | 5     | 4     | **3** |
|      |      |       |       |       |       |       |       |



##### Scramble String

对于方法一，动态规划的自顶向下递归方法，分为两类情况：

- 从前向后将s1和s2分别切成两段，分别比较这两段是否是Scramble String
- s1从后向前分割，s2仍从前往后分割，分别比较这两段是否是Scramble String

对于方法二，动态规划的自底向上的循环实现，递推关系如下：

dp\[l\]\[c\]\[h\] = (dp\[y\]\[c\]\[h\] and dp\[l-y\]\[c+y\][h+y]) or (dp\[y\]\[c+l-y\][h] and dp\[l-y\]\[c\][h+y])

dp\[l\]\[c\]\[h\]表示s1和s2分别从c和h开始的长度为l的子串是否是Scramble String

```python
class Solution:
    def __init__(self):
        self.mem = {}
    
    def isScramble(self, s1, s2):
        """
        :type s1: str
        :type s2: str
        :rtype: bool
        """
        return self.method2(s1, s2)
    
    def method1(self, s1, s2):
        """
        动态规划的recursive + cache模式
        没有sorted(s1) != sorted(s2)判断超时
        192 / 283 test cases passed.
        "abcdefghijklmn"
        "efghijklmncadb"
        """
        if not s1 or not s2:
            return False
        if len(s1) != len(s2) or sorted(s1) != sorted(s2):
            self.mem[(s1, s2)] = False
            return False
        if s1 == s2:
            self.mem[(s1, s2)] = True
            return True
        if (s1, s2) in self.mem:
            return self.mem[(s1, s2)]
        ret = False
        for c in range(1, len(s1)):
            if (self.method1(s1[:c], s2[:c]) and self.method1(s1[c:], s2[c:])) or  (self.method1(s1[:c], s2[-c:]) and self.method1(s1[c:], s2[:-c])):
                ret = True
                self.mem[(s1, s2)] = ret
                return True
        self.mem[(s1, s2)] = ret
        return False
    
    def method2(self, s1, s2):
        if not s1 or not s2:
            return False
        if len(s1) != len(s2) or sorted(s1) != sorted(s2):
            return False
        size = len(s1)
        dp = [ [[False]*size for _ in range(size)] for _ in range(size+1)]
        for c in range(size):
            for h in range(size):
                dp[1][c][h] = (s1[c] ==s2[h])
        for l in range(2, size+1):
            for c in range(size-l+1):
                for h in range(size-l+1):
                    for y in range (1, l):
                        if not dp[l][c][h]:
                            dp[l][c][h] = (dp[y][c][h] and dp[l-y][c+y][h+y]) or (dp[y][c+l-y][h] and dp[l-y][c][h+y])
        return dp[size][0][0]

```



##### Unique Binary Search Trees II

与树有关的问题很明确是动态规划问题，左右子树的处理和整个序列的处理是一样的，唯一需要注意的是对于每一个跟节点，需要分别求出左右子树可能的集合再做拼接

另外也需要注意基本案例，即start>end和start==end的情况

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
        
    def generateTrees(self, n):
        """
        :type n: int
        :rtype: List[TreeNode]
        """
        if n ==0:
            return []
        return self.method1(1, n)
    
    def method1(self, start, end):
        if start > end:
            return [None]
        if start == end:
            root = TreeNode(end)
            return [root]
        ret = []
        for c in range(start, end+1):
           
            left = self.method1(start, c-1)
            right = self.method1(c+1, end)
            
            for node1 in left:
                for node2 in right:
                    root = TreeNode(c)
                    root.left = node1
                    root.right = node2
                    ret.append(root)
        return ret

```



##### Interleaving String

和机器人收集硬币及编辑距离是一类问题，二阶动态规划

```python
class Solution:
    def isInterleave(self, s1, s2, s3):
        """
        :type s1: str
        :type s2: str
        :type s3: str
        :rtype: bool
        """
        return self.method2(s1, s2, s3)
    
    def method2(self, s1, s2, s3):
        if not s1 and not s2 and not s3:
            return True
        if len(s3) != len(s1) + len(s2):
            return False
        
        height = len(s1) + 1
        width = len(s2) + 1
        
        dp = [[False]*width for _ in range(height)]
        
        for c in range(height):
            for h in range(width):
                if c == 0 and h == 0:
                    dp[c][h] = True
                elif c == 0 and h > 0:
                    dp[c][h] = dp[c][h-1] and s2[h-1] == s3[c+h-1]
                elif h == 0 and c > 0:
                    dp[c][h] = dp[c-1][h] and s1[c-1] == s3[c+h-1]
                else:
                    dp[c][h] = dp[c-1][h] and s1[c-1] == s3[c+h-1] or dp[c][h-1] and s2[h-1] == s3[c+h-1]
        return dp[-1][-1]
```



##### Distinct Subsequences

s->t

额外使用一个数组记录t中每一个字符出现的次数，数组初始化为0

匹配的规则是：对于s中的每一个字符，从后向前对t进行匹配，匹配的递推关系为：

$prefix[c]=prefix[c] + (prefix[c-1] if c>0 else 1)$

```python
class Solution:
    def numDistinct(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: int
        """
        return self.method1(s, t)
    
    def method1(self, s, t):
        if not s or not t:
            return 0
        if s == t:
            return 1
        
        prefix = [0]*len(t)
        for item in s:
            for c in range(len(t))[::-1]:
                if item == t[c]:
                    prefix[c] = prefix[c] + (prefix[c-1] if c > 0 else 1)
        return prefix[-1]
```

| b    | a    | g     |      |
| ---- | ---- | ----- | ---- |
| 0    | 0    | 0     |      |
| 1    | 0    | 0     | b    |
| 1    | 1    | 0     | a    |
| 2    | 1    | 0     | b    |
| 2    | 1    | 1     | g    |
| 3    | 1    | 1     | b    |
| 3    | 4    | 1     | a    |
| 3    | 4    | **5** | g    |

输出5

##### Triangle

dfs，对层数进行递归，并需要根据当前层元素的index索引限制下层元素的索引index和index+1

递归表达式

$ total = min(total, curr + self.minimum_total(triangle, level+1, c))$

c是当前层元素索引

```python
import sys
class Solution:
    
    def __init__(self):
        self.mem = {}
        
    def minimumTotal(self, triangle):
        """
        :type triangle: List[List[int]]
        :rtype: int
        """
        return self.minimum_total(triangle, 0, 0)
        
    def minimum_total(self, triangle, level, index):
        """
        未加缓存会在42/43超时
        """
        if level > len(triangle)-1:
            return 0
        if (level, index) not in self.mem:
            total = sys.maxsize
            for c, item in enumerate(triangle[level]):
                if c >= index and c < index + 2:
                    curr = item
                    total = min(total, curr + self.minimum_total(triangle, level+1, c))
            self.mem[(level, index)] = total
        return self.mem[(level, index)]


```



##### Best Time to Buy and Sell Stock II

采用动态规划dp(i,j)表示i次操作在第j天的最大收益

dp(i,j) = max(dp(i, j-1), max[dp(i-1, k) + prices[i]-pricesp[k] for k from 0 to i]),dp(0, j)=0, dp(i,0)=0

```python
def max_profit(prices):
    if not prices:
        return 0
    k = 2
    dp = [[0]* len(prices) for _ in range(k+1)]
    ret = 0
    for i in range(1, k+1):
        temp_max = dp[i][0] - prices[0]
        for j in range(1, len(prices)):
            dp[i][j] = max(dp[i][j-1], temp_max + prices[j])
            temp_max = max(dp[i-1][j] - prices[j], temp_max)
            ret = max(ret, dp[i][j])
    return ret
```

上面的结果在k=2时有很多重复计算，因而可以先计算一次操作的每天最大收益，然后从后往前推两次操作可以达到的最大收益

```python
def max_profit(prices):
    if not prices:
        return 0
    curr_min = prices[0]
    max_profits = 0
    profits = []
    for price in prices:
        curr_min = min(curr_min, price)
        max_profits = max(max_profits, price - curr_min)
        profits.append(max_profits)
        
    total = 0
    curr_max = prices[-1]
    max_profits = 0
    for i in range(len(prices)-1, -1, -1):
        curr_max = max(curr_max, prices[i])
        max_profits = max(max_profits, curr_max-prices[i])
        total = max(total, profits[i] + max_profits)
    return total  
```



##### Palindrome Partitioning II

##### Maximum Product Subarray

##### Best Time to Buy and Sell Stock IV

##### House Robber

##### House Robber II

##### Paint House

##### Ugly Number II

##### Paint House II

##### Paint Fence

##### Perfect Squares

##### Longest Increasing Subsequence

##### Range Sum Query - Immutable

##### Range Sum Query 2D - Immutable

##### Best Time to Buy and Sell Stock with Cooldown

##### Create Maximum Number

##### Coin Change

##### Counting Bits

##### Integer Break

##### Maximal Rectangle

##### Maximal Square

##### Best Time to Buy and Sell Stock

##### Dungeon Game

##### Unique Binary Search Trees

##### Decode Ways

##### Regular Expression Matching

##### Unique Paths

##### Maximum Subarray

##### Climbing Stairs

##### Word Break II

##### Russian Doll Envelopes

##### Android Unlock Patterns

##### Count Numbers with Unique Digits

##### Bomb Enemy

##### Max Sum of Rectangle No Larger Than K

##### Largest Divisible Subset

##### Guess Number Higher or Lower II

##### Wiggle Subsequence

##### Combination Sum IV

##### Is Subsequence

##### Burst Balloons

##### Frog Jump

##### Partition Equal Subset Sum

##### Sentence Screen Fitting

##### Split Array Largest Sum

##### Wildcard Matching

##### Arithmetic Slices

##### Arithmetic Slices II - Subsequence

##### Can I Win

##### Unique Substrings in Wraparound String

##### Count The Repetitions

##### Encode String with Shortest Length

##### Ones and Zeroes

##### Concatenated Words

##### Word Break

##### Target Sum

##### Predict the Winner

##### Longest Palindromic Subsequence

##### Super Washing Machines

##### Continuous Subarray Sum

##### Remove Boxes

##### Freedom Trail

##### Student Attendance Record II

##### Maximum Vacation Days

##### Out of Boundary Paths

##### Non-negative Integers without Consecutive Ones

##### K Inverse Pairs Array

##### Shopping Offers

对于每一次给定的条件，需要先计算不用优惠的价格，以及使用不同优惠券的价格，在每次使用完优惠券之后，得到新的needs,将当前优惠的价格和新needs需要的价格相加得到用券后的价格，与原始价格比较取较小值

递推关系如下：
$$
cost = min(cost, s[-1] + self.method1(price, special, needs))
$$
同时不妨加入缓存

```python
class Solution:
    
    def __init__(self):
        self.mem = {}
        
    def shoppingOffers(self, price, special, needs):
        """
        :type price: List[int]
        :type special: List[List[int]]
        :type needs: List[int]
        :rtype: int
        """
        return self.method1(price, special, needs)
    
    def cost_sum(self, price, needs):
        cost = 0
        for c, need in enumerate(needs):
            cost += need * price[c]
        return cost
        
    
    def method1(self, price, special, needs):
        """
        未加缓存Runtime: 128 ms
        之后Runtime: 76 ms，40%非常客观的提升
        
        """
        if tuple(needs) in self.mem:
            return self.mem[tuple(needs)]
        if not price or not special or not needs:
            return 0
        cost = self.cost_sum(price, needs)
        for s in special:
            clone = needs[:]
            c = 0
            while c < len(clone):       
                if s[c] > clone[c]:
                    break
                clone[c] -= s[c]
                c += 1
            if c == len(clone):
                cost = min(cost, s[-1] + self.method1(price, special, clone))
        self.mem[tuple(needs)] = cost
        return self.mem[tuple(needs)]

```



##### Decode Ways II

##### Maximum Length of Pair Chain

##### Palindromic Substrings

##### 2 Keys Keyboard

##### 4 Keys Keyboard

##### Coin Path

##### Strange Printer

##### Number of Longest Increasing Subsequence

##### Maximum Sum of 3 Non-Overlapping Subarrays

##### Knight Probability in Chessboard

##### Stickers to Spell Word

##### Partition to K Equal Sum Subsets

##### Minimum ASCII Delete Sum for Two Strings

##### Best Time to Buy and Sell Stock with Transaction Fee

##### Maximum Length of Repeated Subarray

##### Minimum Window Subsequence

##### Count Different Palindromic Subsequences

##### Cherry Pickup

##### Delete and Earn

##### Min Cost Climbing Stairs

##### Number Of Corner Rectangles

##### Largest Plus Sign

##### Longest Palindromic Substring

##### Domino and Tromino Tiling

##### Cheapest Flights Within K Stops

##### Minimum Swaps To Make Sequences Increasing

##### Soup Servings

##### Largest Sum of Averages

##### Race Car

##### New 21 Game

##### Push Dominoes

##### Shortest Path Visiting All Nodes

##### Minimum Number of Refueling Stops

##### Length of Longest Fibonacci Subsequence

##### Stone Game

##### Profitable Schemes

##### Super Egg Drop

##### Bitwise ORs of Subarrays

##### Numbers At Most N Given Digit Set

##### Valid Permutations for DI Sequence

**参考**

[Dynamic Programming: First Principles](http://www.flawlessrhetoric.com/Dynamic-Programming-First-Principles)

[利润最大化](https://lukasmericle.github.io/dynprotut/)



