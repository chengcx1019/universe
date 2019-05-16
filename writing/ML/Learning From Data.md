Learning From Data

1 The Learning Problem

学习模型=假设集+算法

学习问题，目标函数和数据集则是由问题本身决定的。

1.1 Problem  Setup

1.2 Types of Learning

1.3 Is Learning Feasible

取样之后，算法根据样本习得的模型的性能与原始样本是无关的，至于学习得到的模型能不能很好的估计样本，就得通过别的指标去判定。



**probability to the rescure**

取样数N

罐子里的红色小球的概率是u

从一个样本无限大的罐子里有放回的抽取N个小球，观察到的红色小球的概率是v

霍夫丁不等式告诉我们：

$P[|𝞶-𝞵|>𝟄]\leq 2e^{-2N𝟄^2}$

u尽管未知，却是一个常量

E_in 和E_out

**feasibility of learning**

E_in接近E_out的同时，E_in要尽可能小（考虑到不能总要求它等于0吧）

$P[|E_{in}-E_{out}|>𝟄]\leq 2Me^{-2N𝟄^2}$

M可视为Hsets的复杂度，H的复杂度问题将在第二章深入讨论。

目标函数f越复杂，就越难以拟合它的数据集，因而E_in也会相对更大

1.4 Error and Noise

**error measures**

根据不同的场景，赋予不同的错误衡量

**noisy targets**

1.5 Problems

公式
$$
(1.1)h(x)=sign((\sum_{i=1}^dw_ix_i)+b)\\
(1.2)h(x)=sign(w^Tx)\\
(1.3)w(t+1)=w(t)+y(t)\\
(1.4)P[|𝞶-𝞵|>𝟄]\leq 2e^{-2N𝟄^2},for\ any \ 𝟄>0\\
(1.5)P[|E_{in}-E_{out}|>𝟄]\leq 2e^{-2N𝟄^2},for\ any \ 𝟄>0\\
(1.6)P[|E_{in}(g)-E_{out}(g)|>𝟄]\leq 2Me^{-2N𝟄^2},M为H的复杂度
$$


2 Training versus Testing



2.1 Theory of Generalization

由公式2-1，当M无穷大的时候，这个不等式久没有任何意义了。

$B_m是使|E_{in}(h_m)-E_{out}(h_m)|\geq \epsilon 的(Bad) event  .$

当坏事件的重叠非常多的时候，约束就会非常宽松，需要在H中找到那些有效的h

**effective number of hypotheses**

growth function形式化有效假设的数量，将使用生长函数来替代M；那么什么是成长函数，成长函数是不是也应该有一个上界，如何用成长函数来替代M。

感知机模型中，3个点时虽然存在不能二分的情况，但是还是存在3个点的组合可以被二分（三点共线时不能，但是三点呈三角形分布时就可以

**bounding the grouth function**

**the vc dimension**

几个关于d_vc的描述

1. N个点的集合可以被H scatter，则d_vc>=N
2. N个点的任意集合都可以被H shatter，则d_vc>=N
3. 有**某个**N个点的集合不能被H shatter，这句话不能确定任何事情
4. 没有N个点的集合可以被H shatter，d_vc<N 

d_vc可以用来表达自由变量的维度。

**the VC grneralization bound**



2.2 Interpreting the Generalization Bound

2.3 Approximation-Generalization Tradeoff

2.4 Problems

公式
$$
(2.1)E_{out}(g)\leq E_{in}(g)+\sqrt{\frac1{2N}\ln \frac{2M}\delta } \\
(2.3)H(x_1,...,x_N)=\{h(x_1),...,h(x_N)|h\in H\}\\
m_H(N)=\max_{x_1,x_2,…,x_N\in X }|H(x_1,x_2,…,x_n)|,|.|表示集的势\\
(2.4)B(N,k)=𝛂+2𝛃\\
(2.5)𝛂+𝛃\leq B(N-1,k)\\
(2.6)𝛃\leq B(N-1,k-1)\\
(2.7)B(N,k)\leq B(N-1,k)+B(N-1,k-1)\\
(2.8)m_H(N)\leq \sum_{i=0}^{k-1}(_{\ i}^N)\\
(2.9)m_H(N)\leq \sum_{i=0}^{d_{vc}}(_{\ i}^N)\\
(2.12)E_{out}(g)\leq E_{in}(g)+\sqrt{\frac8{N}\ln \frac{4m_H(2N)}\delta } ,概率大于(1-\delta)
$$


3 The Linear Model

3.1 Linear Classification

3.2 linear Regression

3.3 Logistic Regression

3.4 Nonlinear Transformation

3.5 Problems



4 Overfitting

4.1 When Does Overfitting Occur

4.2 Regularization

4.3 Validation

4.4 Problems



5 Three Learning Principles

5.1 Occam's Razor

5.2 Sampling Bias

5.3 Data Snooping

5.4 Problems