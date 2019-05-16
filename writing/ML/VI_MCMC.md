# 从VAE和GAN谈变分推理和马尔可夫蒙特卡罗采样

> 这不是一篇科普文章，用几个形象的比喻解释概念就结束了，做好准备去真正理解数学公式，不同方法间的联系和区别也就显而易见了。
>
> 另注变分推理用于贝叶斯估计和机器学习领域中的近似计算复杂积分则称变分贝叶斯推理(Variational Bayesian Inference)，它关注的如何求解一个近似后验概率分布。

- [ ] (全文对应不同分布的指代需要同步)
- [ ] 对数据驱动方法的模型整理

变分自编码器(Variation Autoencoder,VAE)和生成对抗网络(Generative Adversarial Network ,GAN)是典型的深度生成模型，这类**生成模型**通常被参数化为一个深度神经网络：

- VAE

  VAE假定一个显式的生成模型:
  $$
  x\sim p(x|z;\theta), z\sim p(z)
  $$
  $x$是从显式生成分布$p(x|z;\theta)$上采样得到，可以显式的计算$x$的似然度。

- GAN

  GAN假定一个隐式生成模型，通过下式生成一个样本：
  $$
  x=G(z;\theta), z\sim p(z)
  $$
  即首先从先验分布$p(z)$上采样$z$,然后通过神经网络$G(\theta)$将$z$进行变换，并输出最后的样本$x$,上式实际定义了对$x$的分布$p(x;\theta)$。

现在最基本的目标当然是两种生成模型的参数如何确定，先来看二者学习生成参数$\theta$的差异：

- VAE

  VAE额外的学习一个变分分布$q(z|x;\phi)$(编码器)，它是真实后验概率分布$p(z|x;\theta)$的近似，且正比于$p(x|z;\theta)p(z)$；最终的目标转化为训练来最小化变分分布和真实后验分布之间的KL散度：$KL(q(z|x;\phi)||p(z|x;\theta))$。

- GAN

  通过设置一个对抗博弈，将生成器$p(x;\theta)$和判别器$q(\phi_i)$($\phi$被固定为某个值$\phi_i$)配对，其中判别器被训练分辨真实数据和生成样本，而生成器被训练生成可以欺骗判别器的样本。

> $G(z;\theta)$表示函数关系，G的结果是由数值z和变量$\theta$共同确定，注意分号“;”的意义，分隔的数值和变量；
>
> $p(x;\theta)$表达分布，表示$x$的分布是由参数$\theta$确定的；
>
> 上一篇文章中曾经强调过符号"|"的意义,注意区分表达的是条件概率还是表达似然性，主要是看"|"后面的参数是不是随机变量(嗯，关于随机变量的说法，频率学派可能就不同意了，认为是固定参数)。

在得到优化参数目标函数的表达式之前，我们需要先了解变分推理，而变分推理本身和MCMC之间有着密切的关系。

像往常一样，我们从贝叶斯公式开始：
$$
p(z|x) = \cfrac{p(x|z)p(z)}{p(x)}\tag{1}
$$
引用一个很好的解释，如果通过$x \in R^D$上的概率分布对整个世界建模，其中$p(x)$表示$x$可能处于的状态。这个世界可能非常的复杂，我们无法知道$p(x)$的具体形式。为了解决这个问题，引入另一个变量$z\in R^d$来描述$x$的背景信息，这个变量使得我们可以将$p(x)$表示为一个无限混合模型：
$$
p(x) = \int p(x|z)p(z)dz\tag{2}
$$
对于$z$的任意可能，都引入另一个条件分布，并通过$z$的概率进行加权，最终得到$p(x)$取值。那么基于上面这样的设定，那么我们需要解决的问题就变成：给定$x$的观测值，隐变量$z$是什么，即贝叶斯公式的左边——后验概率分布$p(z|x)$。



无论是计算后验概率分布$p(z|x)$还是边缘概率分布$p(x)$都需要计算式$(2)$，但是我们需要知道，由于$x$和$z$的取值空间都非常大，通常情况我们认为是无法直接计算的，因而需要采用近似推理进行估计。



事实上，我们有几种近似计算复杂积分的技术可以对$p(x)$进行估计，这里我们重点介绍和对比两种方式，变分推理(**VI**，varitational inference)和马尔可夫蒙特卡罗采样(**MCMC**，Marokv Chain Monte Carlo)。

## VI和MCMC

在复杂的统计模型中，模型一般包括三类变量：观测变量(observed variables, data)，未知参数（parameters），和隐变量（latent variables）

### 变分推理 vs MCMC

不管是变分推理还是MCMC方法，最终本质上都是解决一个优化问题：
$$
q_\phi^*(z|x)={arg\ min}_{q_\phi(z|x)\in D}KL({q_\phi(z|x)}||{p_\theta(z|x)})
$$
VI通过优化来近似概率分布，广泛应用于估计**使用贝叶斯推理估计后验概率分布时**出现的难以计算的概率,比如上面提到的$p(x)​$。

通常我们也用MCMC方法来近似，但是当面对大数据集或者复杂模型，MCMC需要采样大量的样本来进行可靠的估计，但这在效率上是不可接受的。

MCMC产生与目标分布中渐进的精确样本，适合于精确推理，但是计算量大，计算缓慢；VI计算更快，在探索多场景和大数据集下更实用。

### 变分推理

> 定义：从关于**隐变量的分布族中**找到一个与真实分布(p,参数用$\theta$标记)差异最小的分布，这个分布的参数用𝝓来标记，即通过寻求最可能接近真实分布的近似分布$q$来逼近真实分布$p$。

相比EM算法（隐变量的条件分布在迭代过程中是可以计算的），**VI则用于隐变量的条件概率无法计算的模型**。

使用KL散度描述两个概率分布p和q的相似程度:
$$
KL(q(z|x;\phi)||p(z|x;\theta)) = E_{q(z|x;\phi)}[log \cfrac{q(z|x;\phi)}{p(z|x;\theta)}]
$$


通过最小化KL散度就可以推导出变分推理的ELOB(Evidence Lower Bound)：
$$
\begin{split}
ELOB(q(z|x;\phi)) 
&= E_{q(z|x;\phi)}[log \cfrac{p(x,z;\theta)}{q(z|x;\phi)}]\\\\
&=E_{q(z|x;\phi)}[log\ {p(x,z;\theta)}] - \\\\
&E_{q(z|x;\phi)}[{log\ q(z|x;\phi)}]
\end{split}
$$
可以得到：
$$
log\ p(x;\theta) = KL(q(z|x;\phi)||p(z|x;\theta)) + ELOB(q(z|x;\phi))
$$

考虑隐变量$z$，那么左边是与$z$无关的常量，由于目标是最小化两个分布的KL散度，因此最小化$KL(q||p)$散度等价于**最大化**$ELOB(q)$，由于KL散度是非负的（参见附录信息论部分），因此$ELOB(q)$是$logp(x;\theta)$的下界。

现在需要关心的是如何最大化$ELOB(q)$，







既然我们已经用$ELOB(q)$指定了变分目标函数，那么现在需要选取分布的变分族，从中选择近似变分分布。一个比较通用的变分族是平均场变分族(mean-field variational family)。平均场变分族的变分分布，**隐变量间相互独立**，而且**每一个变量都受一个不同的因素支配**，平均场变分族的一般成员可表示如下：
$$
q(z) = \prod_{j=1}^m q_j(z_j)
$$
我们已经指定了目标函数和分布的变分族，那么现在我们需要使用优化算法选择从变分族中选择合适的分布。**坐标上升平均场变分推理**(Coordinate ascent mean-field variational inference, CAVI)通过固定变分分布的其他变分因子，迭代优化每一个变分因子，CAVI算法描述如下图：

![sc_8](/Users/changxin/Pictures/SceenShots/sc_8.png)

进一步理解优化过程可参阅附录[VI求解高斯混合模型](#VI求解混合高斯模型)。





定义关于参数$\theta$的后验概率分布：
$$
\begin{split}
p(\theta|X,Y,\alpha) 
&= \cfrac{p(\theta|\alpha)p(Y|X,\theta)}{\int_{\hat \theta}p(\hat\theta|\alpha)p(Y|X,\hat \theta)d\hat \theta}\\\\
&=\cfrac{p(\theta,Y|X,\alpha)}{p(Y|X,\alpha)}
\end{split}
$$
其中$\alpha$是$\theta$的先验分布,同时记真实分布为$q(\theta|\phi)$,则使用KL Divergence来衡量近似分布和真实后验概率分布的话，可以表达为：
$$
\begin{split} 
D_{KL}(q(\theta|\phi)||p(\theta|X,Y,\alpha) )
&=E_{q(\theta|\phi)}[log\cfrac{q(\theta|\phi)}{p(\theta|X,Y,\alpha) }]\\\\
&= F(D,\phi)+log\ p(Y|X,\alpha)
\end{split}
$$

- [ ] $F(D,\phi)​$,变分自由能量的意义是什么

许多概率模型由未归一化的概率分布$\hat p(x;\theta)$定义，必须通过除以配分函数来归一化$\hat p$,以获得一个有效的概率分布。

> 配分函数：是未归一化概率所有状态的积分$\int\hat p(x)dx$（连续变量）或求和$\sum_x\hat p(x)$（离散变量）,而对于很多有趣的模型来说，以上积分或求和难以计算。

通过最大似然学习无向模型特别困难的原因在于配分函数的计算依赖于参数。



### MCMC

> 马尔可夫链蒙特卡罗指从概率分布中抽样以构建一个收敛到目标分布(数据真实分布)的最大可能分布的一类方法。
>
> 方法分为两部分，蒙特卡罗指的是使用重复随机样本获得数值解的一般性技术，蒙特卡罗可以被视为进行了若干次实验，其中每次都对模型中的参数进行改变并观察其响应；马尔可夫链是一个随机过程，其中次态仅依赖于当前状态。
>
> 综合马尔可夫链和蒙特卡罗的思想，MCMC是一种基于当前值重复绘制某一分布参数随机值的方法。每个值的样本(参数的取值)都是随机的，但是值的选择受限于当前状态和假定的参数先验分布。MCMC 可以被认为是一种随机游走，在这个过程中逐渐收敛到真实分布。



当无法精确计算和或者积分（例如，和具有指数数量个项，且无法被精确简化）时，通常可以使用蒙特卡罗采样来近似它。这种想法把和或者积分视作某分布下的期望，然后通过估计对应的平均值来近似这个期望。令$s=\sum_x p(x)f(x)=E_p[f(x)]$或者$s=\int p(x)f(x)dx=E_p[f(x)]$为我们需要估计的和或者积分，写成期望的形式，p是一个关于随机变量x的概率分布（求和时）或者概率密度函数（求积分时）。回到我们之前最早要讨论的问题，分解出相应的$p(x)$和$f(x)$分别为$p(z)$和$p(x|z)$,即：
$$
p(x)=\sum_{z}p(z)p(x|z)=E_z[p(x|z)]
$$

**两个阶段**

1. 磨合过程，从运行马尔可夫链开始到其达到均衡分布的过程
2. 从均衡分布中抽取样本序列

通常无法通过表达状态序列，转移矩阵，转移矩阵特征值来判断马尔可夫链是否已经混合成功，只能运行一段足够长的时间并通过启发式的方法判断是否混合成功（包括手动检查样本或者衡量前后样本间的相关性）。

- [ ] MCMC只适用于小规模问题，variational Bayes and expectation propagation的出现使得MCMC可以应用于较大规模问题

  这么看来二者不是对立只能选其一，而是可以相互结合的

**收敛**

MCMC的理论可以证明，经过一定次数的迭代之后，本方法一定会收敛的。在一定的迭代次数后所得到的稳定分布会十分接近目标寻求的联合分布。

Burn-in：很明显的最初的一些迭代得到的分布会和目标的后验分布差距很远，因而前N轮的迭代基本是可以直接剔除的。

> 马尔可夫链及其平稳分布
>
> 数学定义：
> $$
> \begin{split}
> \Pr(X_{{n+1}}=x\mid X_{1}
> =x_{1},X_{2}=x_{2},\ldots ,X_{n}=x_{n})\\\\
> =\Pr(X_{{n+1}}=x\mid X_{n}=x_{n})
> \end{split}
> $$
> 即状态转移的概率只依赖于前一个状态，这种“无记忆性”称为马尔可夫性质。
>
> 一般而言，马尔可夫链的状态转移对模型参数进行更新，这一过程和假定模型参数服从某一分布并从中采样更新参数的目的是相似的

#### 几种常用的MCMC采样方式

##### 重要采样：

> 符合下面这类分解方式的就是重要采样

针对复杂的求和或者积分分解出恰当的$p(x)$和$f(x)$，二者不存在唯一分解，因为它总是可以被写成：
$$
p(x)f(x)=q(x)\cfrac{p(x)f(x)}{q(x)}
$$
那么就可以理解为从q分布中采样，然后估计$\cfrac{pf}{q}$在此分布下的均值。



学习的最好的方式是有一个典型的重要采样的例子：

Metropolis_Hastings

##### Gibbs采样

Gibbs采样是最常用的MCMC采样方法

- [ ] 对每一个随机变量产生一个后验条件分布
- [ ] 从目标后验联合分布中模拟后验样本，对每一个随机变量从其它变量固定为当前值的后验条件分布中重复地采样

 **Gibbs采样的局限性**：

1. 即使得到完全的后验联合密度函数，很多情况下很是难以得到每一个随机变量的条件分布概率；
2. 再退一步，即使得到每一个变量的后验条件分布，可能也不是某种已知的分布形式，无法从中直接进行采样；
3. GIbbs采样可能不适合某些应用场景，效率非常低。

**Gibbs采样分析**
$$
E[f(s)]_p\approx = \cfrac{1}{N}\sum_{i=1}^Nf(s^{(i)})
$$
式中p是我们目标需要得到的后验概率分布，f(s)是目标期望，$f(s^{(i)})$是从p中模拟采样的第$i^{th}$个样本。但是关键是我们如何从后验分布中获得样本？

GIbbs采样的思想是固定其他变量，从这些固定参数确定的条件分布中采样。



Gibbs采样算法：

$Initialize\ x^{(0)}\sim q(x)$

for iteration i = 1,2,...do

$x_1^{(i)}\sim p(X_1=x_1|X_2=x_2^{(i-1)},X_3=x_3^{(i-1)},...,X_D=x_D^{(i-1)})$

$x_2^{(i)}\sim p(X_2=x_2|X_1=x_1^{(i)},X_3=x_3^{(i-1)},...,X_D=x_D^{(i-1)})$

...

$x_D^{(i)}\sim p(X_D=x_D|X_1=x_1^{(i)},X_2=x_2^{(i-1)},...,X_{D-1}=x_{D-1}^{(i)})$

end for (until convergence)

针对每一个变量都给定一个先验分布

**从一个实例理解Gibbs采样算法**：

假设有一个观测序列，前n个数据类似 ，后面n～N类似，假设数据服从Possion分布：


$$
Possion(x;\lambda)=e^{-\lambda}\cfrac{\lambda^x}{x!}=\exp(x\log \lambda - \lambda -\log(x!))
$$
分布的均值$\lambda$服从Gamma分布：
$$
Gamma(\lambda;a,b)=\cfrac{1}{\tau(a)}b^a\lambda^{a-1}\exp(-b\lambda)
$$
整个生成模型可以定义如下：

$n\sim Uniform(1,2,3,...N)$

$\lambda_i\sim Gamma(\lambda_i;a,b)$

$x_i\sim\lbrace_{Possion(x_i;\lambda_2) n < i \le N}^{Possion(x_i;\lambda_2)1\le i \le n}$

对于隐变量$n,\lambda_1,\lambda_2$的后验概率分布可以用Bayes公式求解：
$$
p(n,\lambda_1,\lambda_2|x_{1:N})\propto p(x_{1:n}|\lambda_1)p(x_{n+1:N}|\lambda_2)p(\lambda_1)p(\lambda_2)p(n)
$$

$$
x_i\sim\{_{Possion(x_i;\lambda_2) n < i \le N}^{Possion(x_i;\lambda_2)1\le i \le n}
$$

代码实现：

代码绘制出图形



## VAE和GAN优化目标及参数估计

是否还记得我们最初的目标，好了，从VI和MCMC的世界回来吧。

根据重要采样(importance sampling)，当我们需要对原始（名义分布$p(z)$）概率密度分布(pdf)估算一个期望值时，IS使得我们可以从另一个不同的概率分布（建议分布）中抽样，然后将这些样本对名义分布求期望

$q_\phi(z|x)$表示建议分布，参数$\phi$由参数为$\phi\in\Phi$的神经网络确定，我们可以得到：
$$
\begin{split}
p_\theta(x) 
&= \int p( z)p_\theta(x|z)d z\\\\
&=E_{p(z)}[p_\theta(x|z)]\\\\
&=E_{p(z)}[\cfrac{q_\phi(z|x)}{q_\phi(z|x)}p_\theta(x|z)]\\\\
&=E_{q_\phi(z|x)}[\cfrac{p_\theta(x|z)p(z)}{q_\phi(z|x)}]
\end{split}
$$
PS😕

- [ ] 这里有一个问题还没有理清楚

其实还有另一种后验概率的表达方式 

如果说上面表达的z是隐变量，那么这里需要求解的$\theta$怎么表达
$$
p(\theta|x,t) = \cfrac{p(t|x,\theta)p(\theta)}{\int p(t|x,\theta)p(\theta)d\theta}
$$
MLE的角度，基于观测数据t，得到似然函数$\mathbb L(\theta|t,x)$最大时$\theta$的取值，对于回归问题，t是概率分布的自变量（自变量带入**由x和$\theta$共同确定**的概率分布得到概率的值），对于分类问题，t是是否正确分类的标记，如果是正确分类，则取正确分类所对应的概率代入似然函数中。

- [ ] 然而对比本文对隐变量Z的设定，$\theta$瞬间失去了在原本体系中的位置，先验$p(z)$和先验$p(\theta)$各自该如何自处。本这二者合并为z进行理解或许可行,$z = \theta+pre\_z$
- [ ] 当然了，还有EM算法，自难相忘



## 附录

### 信息论

> 应用数学分支，研究对一个信号包含信息的多少进行量化。最初被发明用来研究在一个含有噪声的信道上用离散的字母表来发送信息，在机器学习中主要使用信息论中的一些关键思想来描述概率分布或者量化概率分布之间的相似性。

本小节主要回顾信息论的关键定义。

1. 自信息
   $$
   I(x)=-log\ P(x)
   $$
   自信息只处理单个信息的输出，需要量化整个概率分布中的不确定性总量的话，使用香农熵。

2. 香农熵
   $$
   H(x)=\mathbb E_{x\sim P}[I(x)]=-\mathbb E_{x\sim P}[log\ P(x)]
   $$
   亦记作$H(P)$.

3. KL散度
   $$
   D_{KL}(P||Q)=\mathbb E_{x\sim P}[log\cfrac{P(x)}{Q(x)}]=E_{x\sim P}[log\ P(x)-log\ Q(x)]
   $$
   在离散型变量的情况下，KL散度衡量的是，当我们使用一种被设计成能够使得概率分布Q产生的消息队列长度最小的编码，发送包含由概率分布P产生的符号的消息时(在数学表达$D_{KL}(P||Q)$中，在前的分布P编码信道)，所需要的额外信息量。简单表达衡量的是两个分布之间的差异。

   KL散度是非负的，当且仅当P和Q在离散型随机变量的情况下是相同的分布，或者在连续型变量的情况下几乎处处相同。

4. 交叉熵
   $$
   H(P,Q)=H(p)+D_{KL}(P||Q)=-\mathbb E_{x\sim P}log\ Q(x)
   $$














这一小节核心的内容在这里:smile:,Kl散度不是对称的，因而不能称作两个分布间的距离，那么这种不对称性如何体现的。

假定我们有一个确定性分布P，我们希望用另一个分布Q来近似P。

考虑下面两式的不同含义
$$
q^* = argmin_{q}KL(p||q)
$$

$$
q^* = argmin_{q}KL(q||p)
$$

![sc_10](/Users/changxin/Pictures/SceenShots/sc_10.png)

在左图所示的优化目标中，q作分母，因而q会避免取很小的值；在右图中的优化目标p作分母，因而在p取小值的时候，q也要相应的取小值，因而q的值只能集中于某一个峰。

>  在写这篇文章的同时，有一篇论文提出深度生成模型的统一框架《On Unifying Deep Generative Models》，改写了GAN关于$\theta$的表达，$KL( p_{\theta} ||Q_{\phi_0} ) - JSD_θ$，将$p_{\theta}$看作变分分布,$Q_{\phi_0}$看作真实后验分布。
>
>  在最小化推断分布和对应真实后验分布的KL散度时，生成模型参数$\theta$在KL散度中的位置不同，效果不同：
>
>  GAN中，KL散度倾向于驱使生成模型分布集中集中到真实后验分布的少数几个高密度区域，这使得 GAN 通常能够生成清晰图像，但是缺少样本多样性
>
>  对比来看，VAE 中的 KL 散度($KL(q(z|x;\phi)||p(z|x;\theta))$)会驱使生成模型分布覆盖真实后验分布的所有区域，包括真实后验分布的低密度区域，这通常导致生成模糊图像。

## 混合高斯模型

[缺点](http://sklearn.apachecn.org/cn/stable/modules/mixture.html)：

- 奇异性

  当每个混合模型没有足够多的点时，估算协方差变难，同时算法会发散并且找具有无穷大似然函数值的解，除非认为的进行正则化。

- 分量的数量

  这个算法将会总是用所有它能用的分量，所以在没有外部线索的情况下需要留存数据或者用信息理论标准来决定用多少分量。

单变量，多变量

## EM算法

> 是变分推理的基础

![sc_11](/Users/changxin/Pictures/SceenShots/sc_11.png)

![sc_12](/Users/changxin/Pictures/SceenShots/sc_12.png)



### VI求解混合高斯模型

K个独立的正态分布混合，给出观测数据{x_n},n=1,2,...,N,我需要估计的隐变量有K个混合成分个各自的均值$\boldsymbol\mu=\mu_{1:K}$和观测的来源分布$\boldsymbol c=\mu_{1:n}$,问题可以定义如下：
$$
\begin{split}
\boldsymbol{\mu} = \{\mu_1, ..., \mu_K\}\hspace{2cm}  \\\\
\mu_k \sim \mathcal{N}(0, \sigma^{2}), \hspace{1cm} k = 1, ..., K \\\\
c_i \sim Categorical(\frac{1}{K}, ..., \frac{1}{K}), \hspace{1cm} i = 1, ..., n \\\\
x_i \vert c_i, \boldsymbol{\mu} \sim \mathcal{N}(c_i^T\boldsymbol{\mu}, 1), \hspace{1cm} i = 1, ..., n 
\end{split}
$$
观测和隐变量的联合分布可以表达为：
$$
p(\boldsymbol{\mu, c, x}) = p(\boldsymbol{\mu})\boldsymbol{\prod_{i=1}^{n}}p(c_i)p(x_i \vert c_i,\boldsymbol{\mu})
$$
证据$p(x)$可以表达为：
$$
p(\boldsymbol{x}) = \int d\boldsymbol{\mu} p(\boldsymbol{\mu})\prod_{i=1}^n \sum_{c_i} p(c_i) p(x_i \vert c_i, \boldsymbol{\mu})
$$
下面我们从平均场分布族中选取分布近似关于隐变量的后验概率分布：
$$
q(\boldsymbol{\mu, c}) = \boldsymbol{\prod_{k=1}^K} q(\mu_k;m_k, s_k^2)\boldsymbol{\prod_{i=1}^n}q(c_i;w_i)
$$
隐变量$\mu,c$分别有不同的变分因子，其中$\mu$由自己的均值和方差决定，代入ELBO可得：
$$
ELBO(q) = E[(log(p(z,x))] - E[log(q(z))] \\\\
\implies ELBO(\boldsymbol{m,s^2,w}) = \sum_{k=1}^{K}E[log(p(\mu_k));m_k,s_k^2] \\\\
+ \sum_{i=1}^n(E[log(p(c_i));w_i] + E[log(p(x_i \vert c_i,\boldsymbol{\mu}));w_i,\boldsymbol{m,s^2}]) \\\\
- \sum_{i=1}^nE[log(q(c_i;w_i))] - \sum_{k=1}^KE[log(q(\mu_k;m_k,s_k^2))]
$$
优化迭代过程：

困于此整一周



1. 估计$\boldsymbol c$：
   $$
   q^*(c_i;w_i) \propto \exp\{log(p(c_i)) + E_{-z_i}[log(p(x_i \vert c_i,\boldsymbol{\mu}));\boldsymbol{m,s^2}]\}
   $$
   公式右边指数的第一项是c的先验$log(p(c_i)) = log(\frac{1}{K})$，第二项是第c_i个高斯密度的对数期望：
   $$
   p(x_i \vert c_i,\boldsymbol{\mu}) = \prod_{k=1}^Kp(x_i \vert \mu_{k})^{c_{ik}}
   $$
   则：
   $$
   \begin{split}
   E[log(p(x_i \vert c_i,\boldsymbol{\mu}))] &= \sum_kc_{ik}E[log(p(x_i \vert \mu_k));m_k,s_k^2] \\\\
   &= \sum_kc_{ik}E[-(x_i-\mu_k)^2/2;m_k,s_k^2] + const \\\\
   &=\sum_kc_{ik}(E[\mu_k;m_k,s_k^2]x_i - E[\mu_k^2;m_k,s_k^2]/2) + const
   \end{split}
   $$

2. 估计$\boldsymbol \mu​$:

   

## reference

- [https://am207.github.io/2017/wiki/VI.html#elbo---evidence-lower-bound---objective-function-to-be-optimized](https://am207.github.io/2017/wiki/VI.html#elbo---evidence-lower-bound---objective-function-to-be-optimized)
- [http://akosiorek.github.io/ml/2018/03/14/what_is_wrong_with_vaes.html](http://akosiorek.github.io/ml/2018/03/14/what_is_wrong_with_vaes.html)
- [https://www.jeremyjordan.me/variational-autoencoders/](https://www.jeremyjordan.me/variational-autoencoders/)
- [http://www.zhuanzhi.ai/document/86e444230724b62b3cd9442fb8bccab0](http://www.zhuanzhi.ai/document/86e444230724b62b3cd9442fb8bccab0)





待参阅的资料：

blei的LDA论文的附录，其中便是用mean-field做变分推理的，并且提供了一个完整的LDA变分EM的算法推导。Latent Dirichlet Allocation （LDA）- David M.Blei



# prml

## 绪论

$$
\begin{split}
var[f]&=E[(f(x)-E[f(x)])^2] \\\\
&=E[f(x)^2]-E[f(x)]^2
\end{split}
$$

高斯分布：



![sc_14](/Users/changxin/Pictures/SceenShots/sc_14.png)
$$
N(x|\mu,\sigma^2)=\cfrac{1}{(2\pi\sigma^2)^{1/2}}exp^{-\cfrac{1}{2\sigma^2}(x-\mu)^2}
$$
似然函数：



对数似然函数：
$$
<Empty \space Math \space Block>
$$
![sc_15](/Users/changxin/Pictures/SceenShots/sc_15.png)

最大似然的迁移问题是我们在多项式曲线拟合问题中遇到的过拟合问题的核心

贝叶斯信息准则





针对数据的建模过程，基于概率分布的建模过程，都是最后解释成了统计机器学习，发挥的淋漓尽致的就是graphic model

既然都有统计解释，为什么不直接从统计上直接建模，生成模型



- [ ] 回归分析，假设检验，p-value
- [ ] AIC，最小信息准则 
- [ ] 文本补缺
- [ ] 推荐算法
- [ ] 最小一乘，LDA
- [ ] 为什么说机器学习不单单是一个拟合问题
- [ ] 方差的倒数-精度



亟待解决的问题

- [ ] 学习率选择技巧，确保算法收敛

- [ ] 正则化系数选择技巧

- [ ] 恢复部分参数

- [ ] tensorboard使用技巧

- [ ] 添加测试误差

- [ ] 使用分布式tensorflow

- [ ] momentum

- [ ] 汇总5道tensorflow训练题

- [ ] name_scope

- [ ] I make a mistake：训练时的loss计算就有问题，loss计算的应当是训练数据之后test数据的值

- [ ] 既然已经知道后验概率正比于极大似然估计与先验分布的乘积，且与先验分布具有相同的形式，那么使用这个先验无疑可以有效的降低得到相同准确度所需要的数据量。

  这个结论的验证过程是假设t服从高斯分布推导到和添加正则化项的均方误差是相同的形式

- [ ] 简单的线性模型说明贝叶斯学习

  第一行展示的是在观察到任何数据之前的状况

  ![sc_24](/Users/changxin/Pictures/SceenShots/sc_24.png)

- [ ] 3.3等价核，说不上来哪些点不清楚

- [ ] 第7章 相关向量机

- [ ] 多gpu手工分组的话训练结果汇总

- [ ] 如何缩小训练误差和测试误差的差距

  单纯重新回去训练似乎与降低测试误差的目标没有更进一步的改进呢

- [ ] 练习汇总

  - 9-12
  - 10-9
  - 11-8
  - 11-9
  - 11-10



Tensorflow训练技巧



Gamma函数：
$$
\Gamma(x)\equiv\int_{0}^{\propto}u^{x-1}e^{-u}du
$$


后验=先验+数据（似然估计）



假设t服从与w和x有关的高斯分布，那么自然t取高斯分布概率最高时的取值，即高斯分布的均值处；

如何推广到更一般的分布



分类问题，目标变量的条件概率分布就是伯努利分布，考虑一个由独立的观测组成的训练集，负对数似然函数的给出的误差函数就是交叉熵误差函数





## 另辟蹊径

### 流形学习

PCA在数据有非线性关系时表现并不好

集中主要的流形学习方法：

multidimensional scaling (MDS)

locally linear embedding (LLE)

isometric mapping (IsoMap)





![sc_25](/Users/changxin/Pictures/SceenShots/sc_25.png)

![sc_26](/Users/changxin/Pictures/SceenShots/sc_26.png)



![sc_27](/Users/changxin/Pictures/SceenShots/sc_27.png)



降维方法汇总



师姐：

	跑实验没啥进展，最近在看变分自编码器，找找特征学习和结构设计的方向。
	
	代码传了两份，一是未简化的版本，二是简化实验版。

20180807