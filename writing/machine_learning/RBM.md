> 如果作为一个深度学习研究者，知识库里缺少受限玻尔兹曼机(Restricted Boltzmann Machine,RBM)的话，一定会有很多内容是看不懂的，本文将从理论到实践，包括受限玻尔兹曼机的定义及结构，重建及学习方法，最后通过生成MNIST的概率分布的实例，帮助你理解受限玻尔兹曼机。
>
> 之后会再出一篇，以MNIST数据集为例，对比变分自编码器VAE，对抗生成网络GAN和受限玻尔兹曼机RBM。

受限波尔兹曼机是一种可用随机神经网络（stochastic neural network）来解释的概率图模型（probability graphical model）。是一类具有两层结构、对称连接且无自反馈的随机神经网络模型，层间全连接，层内无连接。RBM是一种有效的特征提取方法，用于初始化前馈神经网络可明显提高泛化能力，堆叠多个RBM组成的深度信念网络能提取更抽象的特征。RBM是由 Hopfield网络、BM（波尔兹曼机）演化而来。

![](/Users/changxin/Pictures/SceenShots/sc_32.png)



## 定义&结构

> 以《深度学习》教材里的定义为基础

**Hopfield网络**

Hopfield神经网络模型**可用作存储器**的互连网络，是一种循环神经网络，从输出到输入有反馈连接。当有输入之后，可以求取出Hopfield的输出，这个输出反馈到输入从而产生新的输出，这个反馈过程一直进行下去。对于一个Hopfield网络来说，关键是在于确定它在稳定条件下的权系数。

首先考虑这样实例来理解Hopfield神经网络，考虑用网络存储一张二值图片，假设用向量$X\in \mathbb R^n$代表我们要存放的图片，我们需要使用一个含有n各节点的网络来存储这张图片，那么节点与节点之间的权重可以用矩阵$W\in \mathbb R^{n\times n}$来表示。

网络的能量函数是：
$$
E= -\sum_{i,j}x_i x_j w_{ij} - \sum_i b_i x_i
$$
能量函数可以任意来定义，我们一般定义成这种形式是它可以更好的被解释，同时也可以能够清楚的得知在节点状态变化时，整个系统能量变化的方向。



**玻尔兹曼机**

玻尔兹曼机用来学习用来学习二值向量上的任意概率分布，在d维二值随机向量$x\in{\lbrace 0,1\rbrace}^d$上定义玻尔兹曼机，神经元的输出只有两种状态激活和未激活，分别用1和0表示。玻尔兹曼机是一种基于能量的模型，可以使用能量函数定义联合概率分布：
$$
p(\textbf{x})=\cfrac{\exp(-E(\textbf{x}))}{Z}\tag{x}
$$
其中E(x)是能量函数，Z是确保$\sum_zp(x)=1$的配分函数。如果对变量不加区分的话，能量函数如下给出：
$$
E(\textbf{x})=-\textbf{x}^T\textbf{U}\textbf{x}-\textbf{b}^T\textbf{x}\tag{x}
$$


其中U是模型参数的权重矩阵，b是偏置向量。正式的，我们将单元x分为两个子集：可见单元v和隐(或潜在)单元h，此时能量函数表达为：
$$
E(\textbf{v},\textbf{h})=-\textbf{v}^T\textbf{R}\textbf{v}-\textbf{v}^T\textbf{W}\textbf{h}-\textbf{h}^T\textbf{S}\textbf{h}-\textbf{b}^T\textbf{v}-\textbf{c}^T\textbf{h}\tag{x}
$$
式中$b,c,W,R,S$都是无约束、实值的可学习参数。



**受限玻尔兹曼机**

相比于一般的玻尔兹曼机可以具有任意连接，RBM的一个重要特点是在任意两个可见单元之间或任何两个隐藏单元之间没有直接的相互作用，因此称为受限，就像普通的玻尔兹曼机，RBM也是基于能量的模型，其联合概率分布由能量函数指定：
$$
p(\textbf{v}=v,\textbf{h}=h)=\cfrac{1}{Z}\exp(-E(\textbf{v},\textbf{h}))\tag{x}
$$


相应的其能量函数为：
$$
E(\textbf{v},\textbf{h})=-\textbf{v}^T\textbf{W}\textbf{h}-\textbf{b}^T\textbf{v}-\textbf{c}^T\textbf{h}\tag{x}
$$
式中$b,c,W,R,S$都是无约束、实值的可学习参数。

对RBM结构的限制产生了良好的属性，在给定可见层神经元的状态时，各隐层神经元的激活条件独立；反之，给定隐层神经元的状态，可见层神经元的激活条件也独立：
$$
p(\textbf{h}|\textbf{v})=\prod_ip(h_i|\textbf{v})\tag{x}
$$

$$
p(\textbf{v}|\textbf{h})=\prod_ip(v_i|\textbf{h})\tag{x}
$$

能量函数本身只是参数的线性函数，很容易获取能量函数的导数：
$$
\cfrac{\partial}{\partial W_{i,j}}E(\textbf{v},\textbf{h})=-v_ih_j\tag{x}
$$
高效的Gibbs采样和导数计算，使训练过程变得非常方便，训练模型可以得到数据$\textbf{v}$的表示$\textbf{h}$,经常使用$\mathbb E_{\textbf{h}-p(\textbf{h}|\textbf{v})}[h]$作为一组描述$\textbf{v}$的特征，由矩阵参数化层之间的高效相互作用来完成**表示学习**。





## 定义解毒

> 对，没有错别字

<!--《深度学习》 有这样一个“毛病”，它的文字里洋溢着“这些特性使得xx过程变得显而易见”这样的意思，而且从来都不做进一步解释。-->

**基于能量的模型**

1. 能量模型的定义

   能量模型是什么，先来看这样一个例子，把一个表面粗糙又不太圆的小球，放到一个表面也比较粗糙的碗里，就随便往里面一扔，看看小球停在碗 的哪个地方。一般来说停在碗底的可能性比较大，停在靠近碗底的其他地方也可能；能量模型把小球停在哪定义为一种状态，每种状态都对应着一个能量，这个能量由能量函数来定义，小球处在某种状态的概率（如停在碗底的概率跟停在碗口的概率当然不一 样）可以通过这种状态下小球具有的能量来定义，表示成p=f(E)，其中E是能量函数。

   我们从中引出两个概念，一是概率分布，二是能量函数。

   能量函数是描述整个系统状态的一种测度，系统越有序或者概率分布越集中，系统的能量越小，反之，系统越无序或者概率分布越趋向于均匀分布，则系统的能量越大。能量函数的最小值，对应于系统最稳定的状态。

2. 能量模型的作用

   统计力学的结论表明，任何概率分布都可以转变成基于能量的模型，而且很多的分布都可以利用能量模型的特有的性质和学习过程，有些甚至从能量模型中找到了通用的学习方法。

   以RBM为例，RBM网络的目的是学习得到输入数据的概率分布，但是如果对数据的分布没有任何先验的话，是无从下手的，这时候就需要采用能量模型这样一种通用的学习方法。

   由此可以得出能量模型的两个重要作用：一是全局解的度量，即目标函数，二是能量最小时对应的各个参数的取值，即目标解。能量模型使得学习一个数据的分布变得可行。



## 重建

当RBM以一种无监督的方式通过自身来重建数据 ，这使得在不涉及更深层网络的情况下，可见层和第一个隐层间会存在数次前向和反向传播。

给定初始状态$\textbf{x}$，经过一次前向传递，会得到隐层节点的激活值$\textbf{a}$，在重建(Reconstruction)阶段，隐层的激活状态变成反向传递过程中的输入，与相同的权重相乘并加上偏置向后，就得到重建的输出。

正如你所看到的，在前向传递过程中，给定权重的情况下 RBM 会使用输入来预测节点的激活值，或者输出的概率$\textbf{a}:p(\textbf{a}|\textbf{x}; W)$.

但是在反向传播的过程中，当激活值作为输入并输出原始数据的重建或者预测时，RBM 尝试在给定激活值 a 的情况下估计输入 x 的概率，它具有与前向传递过程中相同的权重参数。这第二个阶段可以被表达为$\textbf{x}:p(\textbf{x}|\textbf{a}; W)$.

这两个概率估计将共同得到关于输入 $\textbf{x}$ 和激活值 $\textbf{a}$的联合概率分布，或者$p(\textbf{x},\textbf{a})$。重建与回归有所不同，也不同于分类。回归基于很多输入来估计一个连续值，分类预测出离散的标签以应用在给定的输入样本上，而**重建是在预测原始输入的概率分布**。



## 学习方法

- [ ] 结合这些属性可以得到高效的块吉布斯采样，它在同时采样所有$\textbf{h}$和同时采样所有$\textbf{v}$间交替。

  ...稍等片刻，马上更新



## 实例解析

> 代码实现参考[github实例](https://github.com/meownoid/tensorfow-rbm)

这份代码实现了Bernoulli-Bernoulli RBM和Gaussian-Bernoulli RBM

- Use BBRBM for Bernoulli distributed data. Input values in this case **must** be in the interval from `0` to `1`.
- Use GBRBM for normal distributed data with `0` mean and `sigma` standard deviation. If it's not, just normalize it.

## reference

https://www.jiqizhixin.com/articles/2018-05-07-7

http://imonad.com/rbm/restricted-boltzmann-machine/

http://www.cnblogs.com/tuhooo/p/5440473.html

https://www.zhihu.com/question/56994540/answer/191053894