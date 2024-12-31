Rademacher复杂度和VC维
===============================

在上一节中，我们给出了PAC学习框架内容，讨论了在有限假设集$\mathcal{H}$下，一致与不一致情况中的泛化误差界。一个直观的问题出现：当假设集$\mathcal{H}$是无穷集时，我们能否对有限的样本进行有效的学习。

本节中，我们将进一步泛化样本复杂度界的结果，并得到关于无限假设集的学习保证。

为实现这一目标，一般的做法是将无限假设集约简为有限假设集，再按之前的流程处理。事实上，有各式各样将无限假设集约简的方法，但每一个都依赖与针对具体假设类的关于复杂度的概念。

我们将要介绍的第一个复杂度概念是**Rademacher复杂度**。这个概念通过一些基于**McDiarmid不等式**的简单证明，能够帮助我们得到学习保证和高质量的界。然而对某些无线假设集来说，经验Rademacher复杂度的计算是一个NP-难的问题，为此引入两个纯粹的组合概念**生长函数**和**VC维**。我们将建立Rademacher复杂度和生长函数的联系，然后依据VC维给出生长函数的界。


Rademacher复杂度
--------------------------------
我们重新回忆一下之前使用的符号系统：**概念** $c:\mathcal{X}\rightarrow\mathcal{Y}$指的是从样本集合$\mathcal{X}$到标签集合$\mathcal{Y}$的一个映射，
$\mathcal{H}$表示假设集（是一个固定的，由所有可能的概念组成的集合），$\mathcal{H}$与$\mathcal{C}$不是必须一致的。

本节的大部分结论都是一般性结论，并针对任意损失函数$L:\mathcal{Y}\times\mathcal{Y}\rightarrow\mathbb{R}$都成立。在下文中
$\mathcal{G}$将被一般化解释为**关于$\mathcal{H}$的损失函数族**，用将$\mathcal{Z}=\mathcal{X}\times\mathcal{Y}$映射到$\mathbb{R}:$

$$\mathcal{G}=\{g:(x,y)\rightarrow L(h(x),y):h\in\mathcal{H}\}$$

一般地，函数族$\mathcal{G}$可以将任意输入空间$\mathcal{Z}$映射到$\mathbb{R}$

Rademacher复杂度通过衡量一个假设集拟合噪声的程度，来捕获函数族的丰富度。（*The Rademacher complexity captures the richness of a family of functions by
measuring the degree to which a hypothesis set can fit random noise.*）

---------------------------------
**经验Rademacher复杂度**：*令$\mathcal{G}$表示能够将输入空间$\mathcal{Z}$映射到$\mathbb{R}$的某一函数族，设$S=(Z_1,Z_2,...Z_m)$为包含$\mathcal{Z}$中元素且有固定样本规模$m$的样本集。则$\mathcal{G}$关于$S$的经验Rademacher复杂度定义为*

$$\hat{\mathfrak{R}}_S(G)=\underset{\sigma}{E}\left[\underset{g\in G}{sup}\frac{1}{m}\sum_{i=1}^m\sigma_ig(z_i)\right]$$

*其中$\mathbf{\sigma}=(\sigma_1,...,\sigma_m)^T$，$\sigma_i$是取值为$\{+1,-1\}$的独立同分布随机变量。这些随机变量$\sigma_i$被称作RAdemacher变量。*

如果我们令$\mathbf{g}_s=[g(z_1),g(z_2),...,g(z_m)]^T$表示函数$g$在样本集$S$上的取值，则经验Rademacher复杂度可以重新定义为
$$\hat{\mathfrak{R}}_S(\mathcal{G})=\underset{\sigma}{E}\left[\underset{g\in\mathcal{G}}{sup}\frac{\mathbf{\sigma}\cdot\mathbf{g}_s}{m}\right]$$
显然期望里的内积$\mathbf{\sigma}\cdot\mathbf{g}_s$衡量了$\mathbf{g}_s$和随机噪声向量$\mathbf{\sigma}$的关联度，而上确界$\underset{g\in\mathcal{G}}{sup}\dfrac{\mathbf{\sigma}\cdot\mathbf{g}_s}{m}$则给出了函数族$\mathcal{G}$与随机噪声$\mathbf{\sigma}$在样本集$S$上关联程度的度量。这样的经验RAdemacher复杂度描述了函数族$\mathcal{G}$的丰富度：更丰富或更复杂的函数族可以产生更多的$\mathbf{g}_s$，在平均意义下将会更好地与随机噪声相关联。

**Rademacher复杂度**：令$\mathcal{D}$表示样本分布，对于任意整数$m\geq 1$，函数族$\mathcal{G}$的Rademacher复杂度定义为所有规模为$m$、依据分布$\mathcal{D}$得到的样本集的经验Rademacher复杂度的期望，即

$$\mathfrak{R}_m(\mathcal{G})=\underset{S\sim\mathcal{D}^m}{\mathbb{E}}[\hat{\mathfrak{R}_s}(\mathcal{G})]$$

------------------------------------
**生长函数**：*假设集$\mathcal{H}$的生长函数$\prod_\mathcal{H}:\mathbb{N}\rightarrow\mathbb{N}$，定义为*

$$\forall m\in\mathbb{N}，\prod_{\mathcal{H}}(m)=\underset{\{x_1,...,x_m\}\subseteq X}{max}|\{(h(x_1),...,h(x_m)):h\in\mathcal{H}\}|$$
换言之，$\prod_{\mathcal{H}}(m)$表示运用假设集$\mathcal{H}$内的元素能够将$m$个点完成分类的最大方式数。这提供了另一种衡量假设集$\mathcal{H}$丰富度的方式。然而与Ramdemacher复杂度不一样的是，这一度量并不依赖与样本分布，这意味值它只是一个纯粹的组合测量概念。

------------------------------------
**VC维**：*一个假设集$\mathcal{H}$的VC-维是指它能完全打散的最大集合的大小：*
$$VCdim(\mathcal{H})=max\{m:\prod_\mathcal{H}(m)=2^m\}$$

VC维同样是一个纯粹的组合测量概念，但是它往往比生长函数（或Ramdemacher复杂度）更便于计算。我们来解释一下上面的$\prod_\mathcal{H}(m)=2^m$：对于一个由$m\geq 1$个元素的集合$S$，假设集$\mathcal{H}$实现了$S$所有可能的分裂，称之为$\mathcal{H}$打散了$S$