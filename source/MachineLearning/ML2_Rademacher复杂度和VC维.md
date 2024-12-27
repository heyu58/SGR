Rademacher复杂度和VC维
===============================

在上一节中，我们给出了PAC学习框架内容，讨论了在有限假设集$\mathcal{H}$下，一致与不一致情况中的泛化误差界。一个直观的问题出现：当假设集$\mathcal{H}$是无穷集时，我们能否对有限的样本进行有效的学习。

本节中，我们将进一步泛化样本复杂度界的结果，并得到关于无限假设集的学习保证。

为实现这一目标，一般的做法是将无限假设集约简为有限假设集，再按之前的流程处理。事实上，有各式各样将无限假设集约简的方法，但每一个都依赖与针对具体假设类的关于复杂度的概念。

我们将要介绍的第一个复杂度概念是**Rademacher复杂度**。这个概念通过一些基于**McDiarmid不等式**的简单证明，能够帮助我们得到学习保证和高质量的界。然而对某些无线假设集来说，经验Rademacher复杂度的计算是一个NP-难的问题，为此引入两个纯粹的组合概念**生长函数**和**VC维**。我们将建立Rademacher复杂度和生长函数的联系，然后依据VC维给出生长函数的界。


Rademacher复杂度
--------------------------------
我们重新回忆一下之前使用的符号系统：**概念**$c:\mathcal{X}\rightarrow\mathcal{Y}$指的是从样本集合$\mathcal{X}$到标签集合$\mathcal{Y}$的一个映射，
$\mathcal{H}$表示假设集（是一个固定的，由所有可能的概念组成的集合），$\mathcal{H}$与$\mathcal{C}$不是必须一致的。

本节的大部分结论都是一般性结论，并针对任意损失函数$L:\mathcal{Y}\times\mathcal{Y}\rightarrow\mathbb{R}$都成立。在下文中$\mathcal{G}$将被一般化解释为**关于$\mathcal{H}$的损失函数族**，用将$\mathcal{Z}=\mathcal{X}\times\mathcal{Y}$映射到$\mathbb{R}:$
$$\mathcal{G}=\{g:(x,y)\rightarrow L(h(x),y):h\in\mathcal{H}\}$$
一般地，函数族$\mathcal{G}$可以将任意输入空间$\mathcal{Z}$映射到$\mathbb{R}$

Rademacher复杂度通过衡量一个假设集拟合噪声的程度，来捕获函数族的丰富度。（*The Rademacher complexity captures the richness of a family of functions by
measuring the degree to which a hypothesis set can fit random noise.*）

---------------------------------
**经验Rademacher复杂度**：
$$\hat{\mathfrak{R}}_S(G)=\underset{\sigma}{E}\left[\underset{g\in G}{sup}\frac{1}{m}\sum_{i=1}^m\sigma_ig(z_i)\right]$$

**Rademacher复杂度**

