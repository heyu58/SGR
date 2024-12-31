#### 26.2 Principal score method under monotonicity

from the Assumption 26.1(CRE) and 26.2(monotonicity)

$$
E\{w_{1,(1,1)}(X)Y|Z=1,M=1\}\overset{Tower Property}{=}E\left\{E\left[w_{1,(1,1)}(X)Y|Z=1,M=1,X\right]|Z=1,M=1\right\}
$$

$$
=\int E[Y|Z=1,M=1,X]w_{1,(1,1)}(X)P[X|Z=1,M=1]dX
$$

而

$$
E\{Y(1)|M(1)=1,M(0)=0\}=E\left\{E[Y(1)|M(1)=1,M(0)=0,X]|M(1)=1,M(0)=0\right\}
$$

$$
=\int E[Y|Z=1,M=1,X]P[X|M(1)=1,M(0)=0]dX
$$

所以只要证明$P[X|M(1)=1,M(0)=0]=w_{1,(1,1)}(X)P[X|Z=1,M=1]$

后续利用贝叶斯定理即可

-----------------------上面是蒋教授给出的解答———————————————

-----------------------下面是我自己的推导（过程存在问题）---------------------------------

考虑$E[w_{1,(1,1)}(X)Y|Z=1,M=1,X]$

$$
=E\left\{\frac{\pi_{(1,1)}(X)}{\pi_{(1,0)}(X)+\pi_{(1,1)}(X)}\frac{\pi_{(1,0)}+\pi_{(1,1)}}{\pi_{(1,1)}}Y|Z=1,M=1,X\right\}
=E\{Y|Z=1,M=1,X\}$$

$$
=E\left\{\frac{\pi_{(1,1)}(X)}{\pi_{(1,1)}(X)+\pi_{(1,0)}(X)}Y(1)|M(1)=1,M(0)=1,X\right\}+E\left\{\frac{\pi_{(1,0)}(X)}{\pi_{(1,1)}(X)+\pi_{(1,0)}(X)}Y(1)|M(1)=1,M(0)=0,X\right\}
$$

$$
\overset{Assumption 26.5}{=}E\{Y(1)|M(1)=1,M(0)=1,X\}
$$

所以$\tau(1,1)=E\{E[Y(1)|M(1)=1,M(0)=1,X]|Z=1,M=1\}-E(Y|Z=0,M=1)=E[Y(1)-Y(0)|M(1)=1,M(0)=1]$

对于$\tau(0,0)$中的$E\{w_{0,(0,0)}Y|Z=0,M=0\}$可以观察到

$$
E\{Y|Z=0,M=0\}=E\{Y(0)|M(0)=0\}=\frac{\pi_{(0,0)}}{\pi_{(0,0)}+\pi_{(1,0)}}E\{Y(0)|M(1)=0,M(0)=0\}+\frac{\pi_{(1,0)}}{\pi_{(0,0)}+\pi_{(1,0)}}E\{Y(0)|M(1)=1,M(0)=0\}
$$

应用类似上面的推导，可以得到

$\tau(0,0)=E[Y|Z=1,M=0]-E[Y(0)|M(1)=0]=E[Y(1)-Y(0)|M(1)=0,M(0)=0]$

将上面分解思想应用到$E\{Y|Z=1,M=1\}$和$E\{Y|Z=0,M=0\}$可以得到

$$
\tau(1,0)=E\{w_{1,(1,0)}Y|Z=1,M=1\}-E\{w_{0,(1,0)}Y|Z=0,M=0\}=E\{Y(1)-Y(0)|M(1)=1,M(0)=0\}
$$

#### 26.3 Principal score method in observational studies

Under Assumption 26.6,26.3 and 26.4

1、the conditional and marginal probabilities

$$
\pi(X)=pr(M=1|Z=1,X)\overset{Assumption26.6}{=}pr(M=1,Z=1|X)=pr(M(1)=1|Z)
$$

$$
\pi=E\{pr(M=1|Z=1,X)\}=E\{E(M|Z=1,X)\}=E\{E[M(1)|X]\}=E[M(1)]=pr(M(1)=1)
$$

2、the principal stratification average causal effects

$$
(1):E\left\{\frac{MZ}{\pi e(X)}Y\right\}=E\left\{\frac{1}{e(X)}E\left[\frac{MZ}{\pi }Y|X\right]\right\}\overset{Assumption26.4}{=}E\left\{\frac{1}{e(X)\pi}E[Z|X]\times E[MY|X]\right\}
$$

$$
\overset{Assumption26.3}{=}E\left\{\frac{1}{\pi}E[ZM(1)Y|X]\right\}=E\left\{\frac{1}{\pi}E[M(1)Y(1)|X]\right\}=E\{Y(1)|M(1)=1\}
$$

$$
(2):E\left\{\frac{\pi(X)[1-Z]}{\pi[1-e(X)]}Y\right\}=E\left\{\frac{E[M(1)|X]}{[1-e(X)]\pi}E[1-Z|X]\times E[Y|X]\right\}=E\{Y(0)|M(1)=1\}
$$

$$
\Rightarrow\tau(1,0)=(1)-(2)=E\{Y(1)-Y(0)|M(1)=1,M(0)=0\}
$$

同理可以得到

$$
\tau(0,0)=E\left\{\frac{1-M}{1-\pi}\frac{Z}{e(X)}Y\right\}-E\left\{\frac{1-\pi(X)}{1-\pi}\frac{1-Z}{1-e(X)}Y\right\}=E\{Y(1)-Y(0)|M(1)=0,M(0)=0\}
$$

#### 27.2 Sequential randomization and joint randomization

we know that

$$
(27.1):(Z,M)\perp\!\!\!\perp Y(z,m)|X
$$

so prove the (27.2) and (27.3)

$$
E\{(Z,M)|X\}\times E\{Y(z,m)|X\}=E\{E[(Z,M)|X,Z]|X\}\times E\{E[Y(z,m)|X,Z]|X\}
$$

$$
=E\{E[M|X,Z]|X\}\times E\{E[Y(z,m)|X,Z]|X\}=E\{(Z,M)Y(z,m)|X\}=E\{E[(Z,M)Y(z,m)|X,Z]|X\}
$$

$$
这导出\Rightarrow M\perp\!\!\!\perp Y(z,m)|(X,Z)\ (27.3)
$$

$$
E\{(Z,M)|X\}\times E\{Y(z,m)|X\}=E\{E[(Z,M)|X,M]|X\}\times E\{E[Y(z,m)|X,M]|X\}
$$

$$
=E\{E[Z|X,M]|X\}\times E\{E[Y(z,m)|X,M]\}=E\{(Z,M)Y(z,m)|X\}=E\{E[ZY(z,m)|X,M]|X\}
$$

$$
这导出\Rightarrow Z\perp\!\!\!\perp Y(z,m)|X\ (27.2)
$$

#### 27.8 Mediation analysis with continuous mediator and binary outcome

the following Normal linear model

$$
M|Z,X\sim N(\beta_0+\beta_1Z+\beta_2^TX,\sigma_M^2)
$$

$$
logit\{pr(Y=1|Z,M,X)\}=\theta_0+\theta_1Z+\theta_2M+\theta_4^TX
$$

so we can estimate$\theta_{direct}\ and\ \theta_{indirect}$

$$
NDE(x)=\sum_m\left[E(Y|Z=1,M=m,X=x)-E(Y|Z=0,M=m,X=x)\right]pr(M=m|Z=0,X=x)
$$

$$
=\sum_m[expit(\theta_0+\theta_1+\theta_2M+\theta_4^TX)-expit(\theta_0+\theta_2M+\theta_4^TX)]pr(M=m|Z=0,X=x)
$$

$$
=E[expit(\theta_0+\theta_1+\theta_2M+\theta_4^TX)-expit(\theta_0+\theta_2M+\theta_4^TM)|Z=0]
$$

$$
NIE(x)=\sum_mE[Y|Z=1,M=m,X=x]\{pr(M=m|Z=1,X=x)-pr(M=m|Z=0,X=x)\}
$$

$$
=\sum_mexpit(\theta_0+\theta_1+\theta_2M+\theta_4^TX)\{pr(M=m|Z=1,X=x)-pr(M=m|Z=0,X=x)\}
$$

$$
=E[expit(\theta_0+\theta_1+\theta_2M+\theta_4^TX)|Z=1]-E[expit(\theta_0+\theta_1+\theta_2M+\theta_4^TX)|Z=0]
$$

if we know the distribution of X

$$
NDE=\sum_xNDE(x)pr(X=x)\ and\ NIE=\sum_xNIE(x)pr(X=x)
$$