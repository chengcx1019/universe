> 文章分为两部分，首先是EM算法的引入及介绍，其次进一步介绍EM的一个重要的应用——高斯混合模型的参数估计，对于EM算法的收敛性不作具体介绍。

## EM算法引入及介绍

概率模型有时既含有观测变量，又含有隐变量，如果概率模型的变量都是观测变量，那么给定数据 ，可以直接使用极大似然估计法，或贝叶斯估计法估计模型参数，但是当模型含有隐变量时，就不能简单地使用这些估计方法，EM算法是一种迭代算法，用于含有隐变量的概率模型参数的极大似然估计，或极大后验概率估计。

EM(Exception Maximization)算法的每次迭代由两步组成：E步求期望；M步求极大。看了《深度学习》里对期望最大化的解释，感觉读懂了对这两个步骤描述的所有文字，却不知道到底做了什么，应而还是引用李航老师的《统计学习方法》中的三硬币模型来理解EM算法。

- 摘录三硬币模型描述如下：

  假设有3枚硬币，分别记作A、B、C，这些硬币正面出现的概率分别为$\pi,p$和$q$(3个概率均为未知)，进行如下掷硬币实验：先投掷硬币A，根据其结果选出硬币B或硬币C，正面选硬币B，反面选硬币C；然后投掷选出的硬币，掷硬币的结果，出现正面记作1，出现反面记作0；独立重复进行n次重复试验（本例n=10），观测结果如下：1,1,0,1,0,0,1,0,1,1，假设只能观测到掷硬币的结果，不能观测掷硬币过程，那么该如何估计三硬币正面出现的概率，即三硬币模型的参数。

- EM算法的一般描述

  - 选择参数的初始值$\theta^{(0)}$，开始迭代；

  - E步：记$\theta^{(i)}$为第i次迭代参数$\theta$的估计值，第i+1次迭代的E步，计算
    $$
    \begin{split}
    Q(\theta,\theta^{(i)})
    &=E_z[\log p(Y,Z|\theta)|Y,\theta^{(i)}] \\\\
    &= \sum_zp(Y,Z|\theta)p(Z|Y,\theta^{(i)})
    \end{split}
    \tag{1}
    $$
    式中$Q(\theta,\theta^{(i)})$称为Q函数，是完全数据的对数似然函数$\log p(Y,Z|\theta)$关于在给定观测数据Y和当前的参数估计$\theta^{(i)}$下对未观测数据Z的条件概率分布$p(Z|Y,\theta^{(i)})$的期望，$p(Z|Y,\theta^{(i)})$是给定观测数据Y和当前的参数估计$\theta^{(i)}$下隐变量Z的条件概率分布。

  - M步：求使$Q(\theta,\theta^{(i)})$极大化的$\theta$，确定第i+1次迭代的参数估计值$\theta^{(i+1)}$:
    $$
    \theta^{(i+1)}=argmax_{\theta}Q(\theta,\theta^{(i)})
    $$

  - 重复E步和M步，直到收敛(收敛条件可以是两次参数之差或者Q函数小于某个阈值)

  > M步主要的作用是更新参数，那么如何得出参数迭代更新式呢，Q函数对各参数求偏导数，并令偏导数等于0，得到参数迭代更新的表达式。

- 三硬币模型解答

  三硬币模型可以表达如下：
  $$
  \begin{split}
  p(y|\theta)
  &= \sum_zp(y,z|\theta) \\\\
  &=\sum_zp(z|\theta)p(y|z,\theta) \\\\
  &=\pi p^y(1-p)^{1-y}+(1-\pi)q^y(1-q)^{1-y}
  \end{split}
  \tag{2}
  $$
  式(2)中y表示观测变量(一次试验的结果是1或者0)；随机变量z是隐变量，表示未观测到的硬币A的投掷结果；$\theta=(\pi,p,q)$是模型参数。

  如果将观测数据表达为$Y=(y_1,y_2,...,y_n)^T$,那么观测数据的似然函数可以表达为：
  $$
  p(Y|\theta)=\prod_{j=1}^n[\pi p^{y_j}(1-p)^{1-{y_j}}+(1-\pi)q^{y_j}(1-q)^{1-{y_j}}]\tag{3}
  $$
  考虑求模型参数$\theta=(\pi,p,q)$的极大似然估计，即
  $$
  \hat{\theta}={argmax_{\theta}}logP(Y|\theta)\tag{4}
  $$
  下面我们使用EM算法来求解这个具体问题：

  - 首先选取参数的初值

  - E步：针对三硬币模型Q函数表达为
    $$
    \begin{split}
    Q(\theta,\theta^{(i)})
    &=\sum_{j=1}^n\sum_z p(z|y_j,\theta^{(i)})\log p(y_j,z|\theta)\\\\
    &= \sum_{j=1}^n [\mu_j\log \pi p^{y_j}(1-p)^{1-{y_j}}+(1-\mu_j)\log (1-\pi)q^{y_j}(1-q)^{1-{y_j}}]
    \end{split}
    \tag{5}
    $$
    式中$\mu_j$是在模型参数$\theta^{(j)}$下观测数据$y_i$来自掷硬币B的概率：
    $$
    \mu_j^{(i+1)}=\cfrac{\pi^{(i)} (p^{(i)})^{y_j}(1- (p^{(i)}))^{1-{y_j}}}{\pi^{(i)} (p^{(i)})^{y_j}(1- (p^{(i)}))^{1-{y_j}}+(1-\pi^{(i)})(q^{(i)})^{y_j}(1-q^{(i)})^{1-{y_j}}}\tag{6}
    $$
    相应的观测数据$y_i$来自掷硬币C的概率为$(1-\mu^{(i+1})$.

  - M步：最大化Q函数以更新模型参数

    要最大化Q函数，需要对参数$\theta=(\pi,p,q)$分别求偏导数，以$\pi$为例：
    $$
    \begin{split}
    \frac{\partial Q}{\partial \pi} 
    &= (\frac{\mu_1}{\pi} - \frac{1-\mu_1}{1-\pi})+\cdot \cdot \cdot + (\frac{\mu_N}{\pi} - \frac{1-\mu_n}{1-\pi}) \\\\
    &= \frac{\mu_1-\pi}{\pi(1-\pi)} + \cdot \cdot \cdot + \frac{\mu_n-\pi}{\pi(1-\pi)} \\\\
    &= \frac{\sum _{j=1} ^n\mu_j-n\pi}{\pi(1-\pi)}
    \end{split}\tag{7}
    $$
    同理可得Q函数对p的偏导数：

    	先以某一项j为例，根据导数的乘法法则：
    $$
    \begin{split}
    \frac{\partial Q}{\partial p_j} 
    &=\mu_j (\cfrac{y_jp^{y_j-1}(1-p)^{1-y_j}}{p^{y_j}(1-p)^{1-{y_j}}} + \cfrac{-(1-y_j)(1-p)^{-y_j}p^{y_i}}{p^{y_j}(1-p)^{1-{y_j}}}) \\\\
    &= \mu_j(\cfrac{y_j}{p} - \cfrac{1-y_j}{1-p} )\\\\
    &= \cfrac{\mu_j(y_j-p)}{p(1-p)}
    \end{split}\tag{8}
    $$
    	将观察数据从$j=1...N$累加，则：
    $$
    \begin{split}
    \frac{\partial Q}{\partial p} 
    &= \frac{\mu_1(y_1-p)}{p(1-p)} + \cdot \cdot \cdot + \frac{\mu_n(y_n-p)}{p(1-p)} \\\\
    &= \frac{\sum _{j=1} ^n\mu_jy_j-\sum _{j=1} ^n \mu_jp}{p(1-p)}
    \end{split}\tag{9}
    $$
    Q函数对q的偏导数：
    $$
    \begin{split}
    \cfrac{\partial Q}{\partial q} 
    &= \cfrac{(1-\mu_1)(y_1-q)}{q(1-q)} + \cdot \cdot \cdot + \cfrac{(1-\mu_n)(y_n-q)}{q(1-q)} \\\\
    &= \cfrac{\sum _{j=1} ^n(1-\mu_j)y_j-\sum _{j=1} ^n (1-\mu_j)q}{q(1-q)}
    \end{split}\tag{10}
    $$
    分别令偏导等于0，得到参数$\theta=(\pi,p,q)$的更新表达式：
    $$
    \pi^{(i+1)}=\cfrac{\sum _{j=1} ^n\mu_j^{(i+1)}}{n}\tag{11}
    $$

    $$
    p^{(i+1)}=\cfrac{\sum _{j=1} ^n\mu_j^{(i+1)}y_j}{\sum _{j=1} ^n\mu_j^{(i+1)}}\tag{12}
    $$

    $$
    q^{(i+1)}=\cfrac{\sum _{j=1} ^n(1-\mu_j^{(i+1)})y_j}{\sum _{j=1} ^n(1-\mu_j^{(i+1)})}\tag{13}
    $$

  - 重复E步和M步，直到收敛

至于为什么迭代求解会得到更优的参数，可以参阅李航老师《统计学习方法》书中的证明或者吴恩达机器学习课程EM算法的讲义，这点就不要质疑了。



## 高斯混合模型的参数估计

核心是Q函数的表达，剩下的就是最大化Q函数时获得迭代更新参数的表达式。