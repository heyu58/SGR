## Peng Ding's Book（Chapter2,3 and Appendix A）

一部分课后习题的答案，以及拓展题目（辛普森悖论模拟）
#### 2.2 Nonlinear causal estimates

1.  Example:

    $\delta_1=\delta_2:\{Y_i(1)\}=1,1,1\ and\ \{Y_i(0)\}=0,0,0$

    $\delta_1>\delta_2:\{Y_i(1)\}=1,0,1\ and\ \{Y_i(0)\}=0,0,1$

    $\delta_1<\delta_2:\{Y_i(1)\}=0,0,1\ and\ \{Y_i(0)\}=0,1,1$

2.  基于上面的例子会认为$\delta_2$对因果作用的估计会更加稳健。因此我认为$\delta_2$非线性因果估计量更有意义。通过下面的例子也可以看出$:\delta_1$会更加敏感的表现因果作用

    $\{Y_i(1)\}=1,0,0,1,1,1\ and\ \{Y_i(0)\}=0,0,0,0,0,1$

    $\delta_1=1>\delta_2=0.5$

    $\{Y_i(1)\}=1,0,0,0,0,0\ and\ \{Y_i(0)\}=1,1,0,1,1,0$

    $\delta_1=-1<\delta_2=-0.5$

#### 3.4 Fisher's exact test

在完全随机化实验中，$n_1,n_0$是确定的（与实验设计的treatment分组有关）

|       | $Y=1$    | $Y=0$    | Total |
|-------|----------|----------|-------|
| $Z=1$ | $n_{11}$ | $n_{10}$ | $n_1$ |
| $Z=0$ | $n_{01}$ | $n_{00}$ | $n_0$ |

在$H_{0F}：Y_i=Y_i(1)=Y_i(0)$强零假设下，无论个体属于$Z=0$组或$Z=1$组，其结果变量$Y$的实现均不会改变，因此记$n_{.1}=n_{11}+n_{10}，n_{.0}=n_{10}+n_{00}$，显然$n_{.1},n_{.0}$均不变

那么任何检验统计量$T(n_{11},n_{10},n_{01},n_{00})=T(n_{11},n_1-n_{11},n_{.1}-n_{11},n_{.0}+n_{11}-n_1)=f(n_1)$都只与$n_{11}$的取值有关

显然根据Fisher‘s exact test理论，记$N=n_1+n_0$可以得到

$$
P(n_{11}=k)=\dfrac{C_{n_1}^{k}C_{n_0}^{n_{.1}-k}}{C_N^{n_{.1}}}
$$

#### 3.5

$$
P(X=k)=\begin{cases}
\dfrac{1}{70},k=0\\
\dfrac{16}{70},k=1\\
\dfrac{36}{70},k=2\\
\dfrac{16}{70},k=3\\
\dfrac{1}{70},k=4
\end{cases}
$$

#### 3.6 Covariate-adjusted FRT

```{r}
library(Matching)
data(lalonde)
z=lalonde$treat#treatment
y=lalonde$re78#outcome
model = lm(y~lalonde$age+
             lalonde$educ+
             lalonde$black+
             lalonde$hisp+
             lalonde$married+
             lalonde$nodegr+
             lalonde$re74+
             lalonde$re75+
             lalonde$u74+
             lalonde$u75)
re = model$residuals
#基于残差的检验统计量
tauhat =t.test(re[z == 1],re[z == 0],var.equal=TRUE)$statistic
student = t.test(re[z == 1],re[z == 0],var.equal=FALSE)$statistic
W = wilcox.test(re[z == 1],re[z == 0])$statistic
D = ks.test(re[z == 1],re[z == 0])$statistic
```

```{r}
#检验p值
asym.pv = c(
  tauhat.pv = t.test(re[z==1],re[z==0],var.equal = TRUE)$p.value,
  student.pv = t.test(re[z==1],re[z==0],var.equal = FALSE)$p.value,
  W.pv = wilcox.test(re[z == 1],re[z == 0])$p.value,
  D.pv = wilcox.test(re[z == 1],re[z == 0])$p.value
)
#查看单边检验p值
round(0.5*asym.pv , 3)
```

```{r}
model_treatment = lm(y~z+#treatment
                       lalonde$age+
                       lalonde$educ+
                       lalonde$black+
                       lalonde$hisp+
                       lalonde$married+
                       lalonde$nodegr+
                       lalonde$re74+
                       lalonde$re75+
                       lalonde$u74+
                       lalonde$u75)
lm.pv = c( lm.pv = 0.5*summary(model_treatment)$coef[2,4])
round(lm.pv , 3)
```

通过上面的分析可以看到，基于协变量矩阵$X$和处理变量$Z$的检验统计量$T(Z,Y,X)$依然有足够的显著性水平可以拒绝sharp strong null Hypothesis

$$
H_{0F}:Y_i(0)=Y_i(1),\ for\ all\ units
$$

因为在该强零假设下，我们已经证明FRT的检验本质上与两样本的t检验一致

因此考虑以outcome为响应变量，treatment为决策变量的策略二，本质上是考虑在treatment变量系数的显著性（即是否在不同取值有明显不同结果），这正是$lm.pv=0.005$所说明的

考虑策略一，我们将outcome对协变量矩阵$X$做回归分析，基于拟合的残差对不同treatment进行两样本的t检验，本质上依然是考虑outcome是否在不同分组显著不同（只不过此处的outcome变为已有的协变量无法说明的残差部分），这正是$re[z==1]\ and\ re[z==0]$的意义

#### 3.8 An algebraic detail

由于$\hat{\overline{Y}(1)}=n_1^{-1}\sum_{Z_i=1}Y_i$ , $\hat{\overline{Y}(0)}=n_0^{-1}\sum_{Z_i=0}Yi$ , $\hat{\tau}=\hat{\overline{Y}(1)}-\hat{\overline{Y}(0)}$

那么

$$
\sum_{Z_i=1}[Y_i-\hat{\overline{Y}(1)}]^2+\sum_{Z_i=0}[Y_i-\hat{\overline{Y}(0)}]^2+\dfrac{n_1n_0}{n}\hat{\tau}^2
$$

$$
=\sum_{Z_i=1}\left[Y_i^2-\dfrac{2}{n_1}Y_i\sum_{Z_i=1}Y_i+\dfrac{1}{n_1^2}(\sum_{Z_i=1}Y_0)^2\right]+\sum_{Z_i=0}\left[Y_i^2-\dfrac{2}{n_0}Y_i\sum_{Z_i=0}Y_i+\dfrac{1}{n_0^2}(\sum_{Z_i=0}Y_i)^2\right]+\dfrac{n_0}{nn_1}(\sum_{Z_i=1}Y_i)^2+\dfrac{n_1}{nn_0}(\sum_{Z_i=0}Y_i)^2-\dfrac{2}{n}(\sum_{Z_i=1}Y_i)(\sum_{Z_i=0}Y_i)
$$

$$
=\sum{Y_i^2}-\dfrac{1}{n_1}(\sum_{Z_i=1}Y_i)^2-\dfrac{1}{n_0}(\sum_{Z_i=1}Y_i)^2+\dfrac{n-n_1}{nn_1}(\sum_{Z_i=1}Y_i)^2+\dfrac{n-n_0}{nn_0}(\sum_{Z_i=0}Y_i)^2-\dfrac{2}{n}(\sum_{Z_i=1}Y_i)(\sum_{Z_i=0}Y_i)
$$

$$
=\sum{Y_i}^2-\dfrac{1}{n}(\sum_{Z_i=1}Y_i)^2-\dfrac{1}{n}(\sum_{Z_i=0}Y_i)^2-\dfrac{2}{n}(\sum_{Z_i=1}Y_i)(\sum_{Z_i=0}Y_i)
$$

$$
=\sum{Y_i}^2-\dfrac{2}{n}\left(\sum_{Z_i=1}Y_i+\sum_{Z_i=0}{Y_i}\right)^2+\dfrac{1}{n}\left(\sum_{Z_i=1}Y_i+\sum_{Z_i=0}{Y_i}\right)^2
$$

$$
=\sum{Y_i}^2-2\overline{Y}\sum{Y_i}+n\overline{Y}^2=\sum{(Y_i-\overline{Y})^2}=(n-1)s^2\ \ \ \ \ \ QED.
$$

#### A.1 Independent but not IID Data

$$
无偏性：E\hat{\mu}=E(\frac{1}{n}\sum_{i=1}^n Y_i)=\frac{1}{n}\sum_i{EY_i}=\frac{1}{n}\sum_i\mu_i=\mu
$$

$$
统计量的方差:Var(\hat{\mu})=E(\hat{\mu}^2)-[E\hat{\mu}]^2=\frac{1}{n^2}E(\sum_i Y_i\sum_i Y_i)-\mu^2=\frac{1}{n^2}E[\sum_i Y_i^2+2\sum_{i,j}Y_iY_j]-\mu^2=\frac{1}{n^2}[\sum_i(\sigma_i^2+\mu_i^2)+2\sum_{i,j}EY_iEY_j]-\mu^2
$$

$$
=\frac{1}{n^2}\sum_i\sigma_i^2+\frac{1}{n^2}\sum_i\mu_i^2+\frac{2}{n}\sum_{i,j}\mu_i\mu_j-\mu^2=\frac{1}{n^2}\sum_i\sigma_i^2+\frac{1}{n^2}(\sum_i\mu_i)(\sum_j\mu_j)-\mu^2=\frac{1}{n^2}\sum_i\sigma_i^2
$$

$$
E\hat{v}=E\left[\frac{1}{n(n-1)}\sum_i(Y_i-\hat{\mu})^2\right]=\frac{E\left[\sum(Y_i-\mu+\mu-\hat{\mu})^2\right]}{n(n-1)}=\frac{E\left[\sum(Y_i-\mu)^2-n(\mu-\hat{\mu})^2\right]}{n(n-1)}=\frac{E\left[\sum(Y_i-\mu_i+\mu_i-\mu)^2-n(\mu-\hat{\mu})^2\right]}{n(n-1)}
$$

$$
=\frac{1}{n(n-1)}[\sum\sigma_i^2+\sum(\mu_i-\mu)^2+\frac{1}{n}\sum\sigma_i^2]\Rightarrow E\hat{v}-Var(\hat{\mu})=...
$$

$$
如果记\hat{\mu}=\overline{Y},等价于证明\frac{1}{n(n-1)}E\left[\sum_i(Y_i-\overline{Y})^2\right]=\frac{1}{n(n-1)}\sum_i(\mu_i-\mu)^2+\frac{1}{n^2}\sum_i\sigma_i^2
$$

#### A.4 Product of two indepdent Normals

已知$X\sim N(\mu_x,\sigma_x^2),Y\sim N(\mu_y,\sigma_y^2),X,Y\ iid.$

$$
Var(XY)=\sigma_X^2\sigma_Y^2+\mu_X^2\sigma_Y^2+\mu_Y^2\sigma_X^2=Var(X)Var(Y)+[EX]^2Var(Y)+[EY]^2Var(X)
$$

$$
=(EX^2-[EX]^2)(EY^2-[EY]^2)+[EX]^2(EY^2-[EY]^2)+[EY]^2(EX^2-[EX]^2)=EX^2EY^2-[EX]^2[EY]^2=EX^2Y^2-[EXY]^2=Var(XY)
$$

#### A.6 Fisher weighting

$$
无偏性:E\hat{\theta}=E\sum_{j=1}^{p}w_j\hat{\theta_j}=\sum_{j=1}^pw_jE\hat{\theta_j}=\theta\ (when\ \sum_jw_j=1)
$$

$$
最小方差:min\ Var(\hat{\theta})=Var(\sum_jw_j\hat{\theta_j})=\sum_jw_j^2Var(\hat{\theta_j})=\sum_jw_j^2v_j
$$

$$
考虑利用:\frac{\partial\sum_jw_j^2v_j}{w}=0\Rightarrow\ w_jv_j=0,for\ all\ j.\ 且\sum_jw_j=1
$$

$$
\Rightarrow w_j=\frac{\frac{1}{v_j}}{\sum_{j'}\frac{1}{v_{j'}}}\ and\ min\ Var(\hat{\theta})=\frac{1}{\sum_jv_j}
$$

#### C.1 Sampling without replacement and the Hypergeometric distribution

根据题意，总样本为n的二元变量实现中，有T个取值为1，n-T个取值为0，现从n的样本中重新抽取n1个则

$$
P(t=k)=\frac{C_T^kC_{n-T}^{n_1-k}}{C_n^{n_1}}
$$

因此统计量服从超几何分布$E(t)=n_1\frac{T}{n},D(t)=n_1\frac{T(n-T)(n-n_1)}{n^2(n-1)}$