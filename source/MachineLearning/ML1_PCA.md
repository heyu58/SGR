The PAC Learning Framework
===============================

在设计和分析针对样本的学习算法时，需要考虑一下几个基本的问题：
- What can be learned efficiently?
- What is inherently hard to learn?、
- How many examples are needed to learn successfully?
- Is there a general model of learning?

为解决这些问题，我们可以通过**概率近似正确（Probably Approximately Correct）** 学习框架对上述问题形式化并给出答案。PAC框架借助样本复杂度和学习算法的时间空间复杂度来定义可学习的概念类。

框架内容
-------------------------
**概念类**：我们可能想要学习的概念构成的集合，记为$\mathcal{C}$

我们假定所有的经验（样本）是独立同分布的，并且服从的某个固定但未知的分布$\mathcal{D}$。在这样的假定下，机器学习问题可以如下形式化：

学习器的任务是：利用带标签的样本集$S$，从所有可能的概念集合$\mathcal{H}$（假设集）中选择一个假设$h_S\in\mathcal{H}$，使其关于待学习的目标概念$c\in\mathcal{C}$有尽可能小的**泛化误差**

**泛化误差**：*给定一个假设$h\in\mathcal{H}$，一个目标概念$c\in\mathcal{C}$，以及一个潜在分布$\mathcal{D}$，则$h$的泛化误差或风险定义为*

$$R(h)=\underset{x\sim\mathcal{D}}{\mathbb{P}}[h(x)\ne c(x)]=\underset{x\sim\mathcal{D}}{\mathbb{E}}[\mathbf{1}_{h(x)\ne c(x)}]$$


然而我们根据定义可以知道，对于学习器而言，一个假设的泛化误差并不是直接可得到的，由于分布$\mathcal{D}$和目标概念$c$均是未知的。我们可以在有标签的样本集$S$上度量一个假设的**经验误差**

**经验误差**：*给定一个假设$h\in\mathcal{H}$，一个目标概念$c\in\mathcal{C}$，以及一个样本集$S=(x_1,x_2,...x_m)$，则$h$的经验误差定义为*

$$\hat{R}_S(h)=\frac{1}{m}\sum_{i=1}^m\mathbf{1}_{h(x_i)\ne c(x_i)}$$

因此总结一下，$h\in\mathcal{H}$的经验误差是在样本集$S$上的平均误差，而$h\in\mathcal{H}$的泛化误差则是在分布$\mathcal{D}$上的期望误差。不加证明的结论是，在某些一般性的假设下，这两个误差关于近似程度的保证有较高的概率成立。

注意到这样一个事实（只要你学过概率论）:对于一个固定的$h\in\mathcal{H}$，在独立同分布样本集上经验误差的期望等于泛化误差。换句话说，经验误差是泛化误差的无偏估计量。

$$\underset{S\sim\mathcal{D}^m}{\mathbb{E}}[\hat{R}_S(h)]=R(h)$$



**************************************

**PAC学习**：*如果存在一个算法$\mathcal{A}$以及一个多项式函数$poly()$，使得对于任意$\epsilon>0$以及$\delta>0$，如果所有在$X$上的分布$D$以及任意目标概念$c\in C$，对于满足$m\geq poly(\frac{1}{\epsilon},\frac{1}{\delta},n,size(c))$的任意样本规模$m$均有下式成立，那么概念类$C$是PAC可学习的*

$$\underset{S\sim\mathcal{D}^m}{\mathbb{P}}[R(h_S)\leq\epsilon]\geq1-\delta$$

*进一步地，若算法$\mathcal{A}$的运行复杂度在$poly(\frac{1}{\epsilon},\frac{1}{\delta},n,size(c))$内，则概念类$C$是高效PAC可学习的*


PAC之所以称为概率近似正确，可以作如下理解：如果输入到一个算法的样本点的书目对于$1/\epsilon$和$1/\delta$是多项式的，并且由该算法基于这些样本点得到的假设是以高概率（至少$1-\delta$）近似正确（误差至多为$\epsilon$）的，则概念类$\mathcal{C}$是PAC可学习的。

关于PAC，有两点需要澄清与强调：
- PAC学习框架是**不依赖分布的模型**，指的是对产生样本的分布$\mathcal{D}$没有特别的假设
- PAC学习框架考虑的是概念类$\mathcal{C}$的可学习性，而不是一个特殊概念的可学习性。对于学习算法，概念类$\mathcal{C}$是已知的，而需要学习的目标概念$c\in\mathcal{C}$是未知的。


一个例子：超矩形的PAC学习
-------------------------------------
一个在$\mathbb{R}^n$中平行与坐标轴的超矩形可表示为$[a_1,b_1]\times...\times[a_n,b_n]$。请证明平行于坐标轴的超矩形是PAC可学习的。（这就是决策树模型）

从简单的$n=2$的平面来理解上面这个问题

![平面图形](../source/_static/rectangle.png"图片title")










附记
-------------------------------
PAC学习框架首先由Valiant[1984]提出