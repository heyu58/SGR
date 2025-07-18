描述泡沫现象的LPPL模型
===============

## 背景故事

LPPL模型（Log-Periodic Power Law，对数周期性幂律模型）是一种用于预测金融资产价格泡沫形成与破裂临界点的数学模型，由物理学家Didier Sornette等人提出。其核心思想源于统计物理中的**相变**理论，认为金融市场泡沫与地震等复杂系统的临界现象类似，价格在泡沫末期会呈现加速上涨和对数周期性振荡的双重特征，直至崩溃。

顺便来聊一聊Didier Sornette。他是一位统计物理学家和地球物理学家，目前在瑞士联邦理工学院苏黎世分校任金融学教授，主讲创业风险。他似乎并没有因外界对这种综合学科研究方法的热情有所减弱而感到烦恼。相反，他还在做自己大部分职业生涯一直在做的事：不仅在主要物理期刊上发表文章，还在领先的金融期刊上发表文章。

Sornette教授作为2004年出版的《股市为什么会崩盘》(Why Stock Markets Crash)一书的作者，希望更深刻地理解泡沫（Bubble）的形成和发展。在对复杂体系的分析中，他独自——或者是和极少数几个人一起——引领着三个并行领域：纯物理学、应用经济学和计量经济学，以及市场从业人员。

在《股市为什么会崩盘》一书中，Sornette教授全面分析了一个由其提出的预测市场泡沫的模型——对数周期幂律（LPPL）模型。该模型对之后许多次市场泡沫都进行了准确的预测，由于该模型由Johansen，Ledoit和Sornette共同提出并完善，因此也被称为JLS模型。

## LPPL模型理论推导

核心问题：对于泡沫的形成与崩溃过程，如何给出资产价格$p(t)$的显示表达式？


在金融数学领域，资产价格动态通常用随机微分方程（SDE）描述，由趋势项$\mu(t)$和波动项$\sigma(t)$组成，用$W(t)$表示维纳过程，则价格$p$的关于时间$t$的微分方程可以写为如下形式：

$$dp = \mu(t)p(t)dt+\sigma(t)p(t)dW(t)$$

LPPL模型理论同样建立在上式，如果定义泡沫破裂（Bubble Crash）的时间点$t_c$，考虑一个示性符号$j$满足：

$$j=\begin{cases} 0 & t<t_c\\ 1 &t>t_c\end{cases} $$

那么在泡沫形成期$t<t_c$时，资产价格的预期增长完全由漂移项驱动，趋势项占据主导地位，波动项作用较小（$<20\%$）。另一方面，泡沫破裂发生的时间$t_c$作为随机变量，拥有连续概率分布$q(t)=P(t_c=t)$，累计分布函数$Q(t)$是合理的。在$t_c$时刻，资产价格会下降$k\in(0,1)$。因此上式可在该模型下改写为：

$$dp = \mu(t)p(t)dt-kp(t)dj$$

我们定义$h(t)$表示在泡沫崩溃（Bubble Crash）尚未发生的情况下，每单位时间内崩盘发生的概率。即

$$h(t)=\frac{q(t)}{1-Q(t)}=\lim_{\Delta t\rightarrow 0}\frac{P(t\leq t_c<t+\Delta t|t_c\geq t)}{\Delta t}$$

    如果你学过生存分析的话，你可能会觉得这个式子很眼熟。这就是死亡风险率的定义。

基于金融领域的无套利假设和绝对理性预期：对未来价格的聚合预测将是截至时间 t 所揭示的信息的预期值，这将是时间$t$时的资产价格。
我们有$\mathbb{E}[p(\acute{t})]=p(t)，for\ all\ \acute{t}>t$

即$\mathbb{E}[dp]=0$，那么

$$\mathbb{E}[dp]=\mu(t)p(t)dt-kp(t)\Big[0\times P(dj=0)+1\times P(dj=1)\Big]$$

$$=\mu(t)p(t)dt-kp(t)\Big[P(t_c=t|t_c\geq t)\Big]$$

$$=\mu(t)p(t)dt-kp(t)\Big[h(t)dt\Big] = 0$$

这可以推导出$\mu(t)=kh(t)$

因此在$t<t_c$条件下（即泡沫破裂还未发生的情况下），$dj=0$，资产价格$p(t)$的微分方程简化为

$$dp=kh(t)p(t)dt（t<t_c）$$

对上式在$t_0\rightarrow t（t<t_c）$积分，可以推导出

$$ln\Big[\dfrac{p(t)}{p(t_0)}\Big]=k\int_{t_0}^{t}h(\acute{t})d\acute{t}$$

这个式子揭示了，当崩盘的可能性越高（即$h(t)越大$），价格增长的速度就越快，以补偿投资者投资面临的市场崩盘风险。风险率$h(t)$应该满足在泡沫增长的过程中不断增大的特点（超指数级别增长）

现在的问题是：在$t<t_c$的情况下，如何对风险率$h(t)$的进行合理的建模？

---------------------

我们找找文献，看看有什么启发。一种从宏观角度出发，根据统计力学的平均场理论（参见 Stanley （1971） 和 Goldenfeld （1992））将风险$h(t)$表示为以下形式：

$$\frac{dh}{dt}=Ch^{\delta}，C>0，\delta>1$$

其中$C$是常数，而$\delta$则表示了交易者之间的平均交互次数。从$t\rightarrow t_c$对上式积分，只考虑主要项，可以得到

$$h(t)\propto(t_c-t)^{-\alpha}，\alpha=\frac{1}{1-\delta}$$

这种形式很好的满足了在$t\rightarrow t_c$过程中，风险率$h(t)$增大的特点。事实上，这种形式恐怕让物理学家Didier Sornette想起了Ising模型中的结果。

接下来就是物理学家Didier Sornette发挥的部分了。他引入了**Ising（易辛）模型**来刻画市场中的投资者行为。易辛模型是一种描述物质铁磁性的经典物理模型，假定单个原子的磁矩只有两种可能的状态，即+1（自旋向上）或-1（自旋向下）。这些原子按照一定的规则排列，并通过交互作用影响彼此的自旋方向。在该模型中，相邻原子之间的相互作用是决定系统磁性行为的关键因素。

Didier Sornette将投资行为类比为磁矩状态。对第$i$个投资者，买入定义为$s_i=+1$，卖出定义为$s_i=-1$，记集合$N(i)$表示与该投资者相互影响的其他投资者。Didier Sornette用下面的马尔科夫过程模拟投资者之间的相互影响。

$$s_i(t+1)=sign\Big(K\sum_{j\in N(i)}s_j(t)+\sigma\epsilon_i\Big)，\ \epsilon_i\overset{iid}{\sim}N(0,1)$$

模型中，常数$K$控制了交易者的模仿趋势，$\sigma$则控制了每个投资者的个人判断强度。当$K$增大时，每个投资者更倾向于模仿其他投资者，交互网络中的投资者的行为趋于统一。而当$\sigma$增大时，交互网络中各投资者的行为更独立无序。

上式中临界值$K_c$：当$K<K_c$时，无序就占主导地位，对一个小的全球影响力的敏感性很低。当模仿力 K 增长接近 Kc 时，就会形成一个集体行动并具有相同立场的代理人群体的层次结构。结果，市场对小的全球干扰变得极其敏感。最后，对于较大的模仿力，以至于 K > Kc，模仿的趋势是如此强烈，以至于存在一种状态/位置的强烈优势。

一个封闭的系统是无意义的，引入全局影响力G（外部力量），对投资者行为产生影响。则投资者行为模型补充为

$$s_i(t+1)=sign\Big(K\sum_{j\in N(i)}s_j(t)+\sigma\epsilon_i+G\Big)，\ \epsilon_i\overset{iid}{\sim}N(0,1)$$

从个人交易状态出发，定义市场平均状态为个人投资者行为随机变量的均值，即

$$M=\frac{1}{I}\sum_{i=1}^Is_i$$

当$G=0$时，由行为的对称性可以得到$E(M)=0$，并且$G>0$时$M>0$，$G<0$时$M<0$。对这样一个交互体系，可以定义系统敏感性$\chi$（可以理解为系统态对外部力量原点处的一阶导数）

$$\chi=\dfrac{dE(M)}{dG}\Big|_{G=0}$$

在Ising模型中，已经有关于不同交互系统敏感性的计算结果。一种常见的网络结构**分层菱形格子hierarchical diamond lattice**，也可以叫“钻石网络”

$$\chi\approx A_0^/(K_c-K)^{-\gamma}+A_1^/(K_c-K)^{-\gamma}\cos\Big(\omega\ln(K_c-K)+\psi\Big)$$

Didier Sornette认为在非理性群体互相模仿投资行为导致资产价格快速上升的过程中，风险率函数符号Ising模型下系统在临界点附近的敏感性函数。可以将风险率函数$h(t)$写成类似的形式

$$h(t)=B_0(t_c-t)^{-\alpha}+B_1(t_c-t)^{-\alpha}\cos\Big(\omega\ln(K_c-K)+\psi\Big)$$

-----------------------

至此我们已经完成了LPPL模型中最终重要的两部分，结合下面两个公式，作粗拟合：

$$ln\Big[\dfrac{p(t)}{p(t_0)}\Big]=k\int_{t_0}^{t}h(\acute{t})d\acute{t}$$

$$h(t)=B_0(t_c-t)^{-\alpha}+B_1(t_c-t)^{-\alpha}\cos\Big(\omega\ln(t_c-t)+\psi\Big)$$

可以得到LPPL对泡沫产生过程中（$t<t_c$）资产价格$p(t)$的近似模拟公式

$$\ln[p(t)]\approx\ln[p(t_0)]-\frac{k}{\beta}\Big\{B_0(t_c-t)^{\beta}+B_1(t_c-t)^{\beta}\cos\Big(w\ln(t_c-t)+\phi\Big)\Big\}$$

如果记$A=\ln[p(t_0)]$表示对数尺度下资产的正常价格，更为常见的简化形式可以写成：

$$\ln[p(t)]\approx A+B(t_c-t)^{\beta}\Big\{1+C\cos[\omega\ln(t_c-t)+\phi]\Big\}$$

至此，对数周期幂律公式LPPL简略推导结束，是Didier Sornette建模来描述泡沫崩盘前价格时间增长的基本方程。


## 关于LPPL模型理论的讨论


## 如何求解LPPL模型