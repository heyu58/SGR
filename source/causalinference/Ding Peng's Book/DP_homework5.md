#### 21.1 Variance of Wald estimator

由于$\hat{\tau_D}=\overline{D_1}-\overline{D_0}=\frac{1}{n_1}\sum Z_iD_i-\frac{1}{n_0}\sum(1-Z_i)D_i$

我们总可以找到样本使得$P(\hat{\tau_D}=0)>0$

$$
Var(\hat{\tau_c})=Var(\frac{\hat{\tau_Y}}{\hat{\tau_D}})=\infty
$$

#### 21.3 Prove Theorem

under the assumptions 21.2-21.4, we have

$$
if\ U=c,\tau_c=E\{Y(d=1)-Y(d=0)|U=c\}=E\{Y(1)-Y(0)|U=c\}=\frac{\tau_Y}{\tau_D}
$$

$$
\Rightarrow \tau_Y=\tau_c\times\tau_D在假设exclusion\ restriction下默认群体c为主要效果
$$

$$
即E\{Y(D(1))-Y(D(0))\}=E\{Y(d=1)-Y(d=0)|U=c\}\times E\{D(1)-D(0)\}两边同取对U=c的条件期望
$$

$$
\{Y(D(1))-Y(D(0))\}=\{D(1)-D(0)\}\times\{Y(d=1)-Y(d=0)\}
$$

#### 21.6 Binary IV and ordinal treatment received

pay attention to the difference $D\in\{0,1,...,J\}$ is ordinal treatment received

$$
Define\ \ D(z)=\sum_{j=0}^Jj\mathbf{1}\{D(z)=j\}\ and\ Y(D(z))=\sum_{j=0}^JY(j)\mathbf{1}\{D(z)=j\}
$$

$$
so\ \ D(1)-D(0)=\sum_{j=0}^Jj\left[\mathbf{1}\{D(1)=j\}-\mathbf{1}\{D(0)=j\}\right]=\sum_{j=0}^Jj[\mathbf{1}\{D(1)\geq j\}-\mathbf{1}\{D(1)\geq j+1\}-\mathbf{1}\{D(0)\geq j\}+\mathbf{1}\{D(0)\geq j+1\}]
$$

$$
=\sum_{j=0}^Jj[\mathbf{1}\{D(0)<j\leq D(1)\}-\mathbf{1}\{D(0)<j+1\leq D(1)\}]=\sum_{j=0}^J\mathbf{1}\{D(0)<j\leq D(1)\}
$$

$$
Y(D(1))-Y(D(0))=\sum_{j=0}^JY(j)\left[\mathbf{1}\{D(1)=j\}-\mathbf{1}\{D(0)=j\}\right]\overset{同上}{=}\sum_{j=0}^JY(j)\mathbf{1}\{D(0)<j+1\leq D(1)\}=\sum_{j=0}^J[Y(j)-Y(j-1)]\mathbf{1}\{D(0)<j\leq D(1)\}
$$

$$
\frac{E[Y|Z=1]-E[Y|Z=0]}{E[D|Z=1]-E[D|Z=0]}=\frac{E[Y(1)-Y(0)]}{E[D(1)-D(0)]}=\dfrac{\sum_{j=1}^JE[Y(j)-Y(j-1)|D(0)<j\leq D(1)]\times P[D(0)<j\leq D(1)]}{\sum_{j=1}^JP[D(0)<j\leq D(1)]}
$$

$$
=\sum_{j=1}^Jw_jE[Y(j)-Y(j-1)|D(1)\geq j>D(0)],\ \ \ where\ w_j=\frac{pr(D(1)\geq j>D(0))}{\sum_{j^/=1}^Jpr(D(1)\geq j^/>D(0))}
$$

#### 22.3 Disentangle the mixtures: distributional results

we know the define $n:never-takers,a:always-takers,c:compliers$

$$
\pi_a+\pi_c+\pi_n=1(\pi_d=0)
$$

$$
f_n(y)=f_{1n}(y)=f_{0n}(y)=P(Y=y|U=n)=P(Y=y|Z=1,D=0)=g_{10}(y)
$$

$$
f_a(y)=f_{1a}(y)=f_{0a}(y)=P(Y=y|U=a)=P(Y=y|Z=0,D=1)=g_{01}(y)
$$

$$
同Theorem\ 22.1的推导：P(Y=y|Z=1,D=1)=\frac{\pi_c}{\pi_c+\pi_a}P(Y(1)=y|U=c)+\frac{\pi_a}{\pi_c+\pi_a}P(Y(1)=y|U=a)
$$

$$
\Rightarrow f_{1c}(y)=\pi_c^{-1}[(\pi_c+\pi_a)P(Y=y|Z=1,D=1)-\pi_aP(Y=y|Z=0,D=1)]=\pi_c^{-1}[(1-\pi_n)g_{11}(y)-\pi_ag_{01}(y)]
$$

$$
同Theorem\ 22.1的推导：P(Y=y|Z=0,D=0)=\frac{\pi_c}{\pi_c+\pi_n}P(Y(0)=y|U=c)+\frac{\pi_n}{\pi_c+\pi_n}P(Y(0)=y|U=n)
$$

$$
\Rightarrow f_{0c}(y)=\pi_c^{-1}[(\pi_c+\pi_n)P(Y=y|Z=0,D=0)-\pi_nP(Y=y|Z=1,D=0)]=\pi_c^{-1}[(1-\pi_a)g_{00}-\pi_ng_{10}(y)]
$$

#### 22.7 Violations of the key assumptions

（1）we know randomization, monotonicity

$$
\pi_c=1-\pi_n-\pi_a(monotonicity)=1-P(D=0|Z=1)-P(D=1|Z=0)=E(D|Z=1)-E(D|Z=0)
$$

$$
E(Y|Z=1)-E(Y|Z=0)\overset{randomization}{=}E(Y(1)-Y(0))=\pi_a\tau_a+\pi_n\tau_n+\pi_c\tau_c
$$

$$
\Rightarrow\frac{E(Y|Z=1)-E(Y|Z=0)}{E(D|Z=1)-E(D|Z=0)}-\tau_c=
\frac{\pi_a\tau_a+\pi_n\tau_n}{\tau_c}$$

（2）we know randomization, exclusion restriction

$$
\pi_c-\pi_d=P(D=1|Z=1)+P(D=0|Z=0)-P(D=0|Z=1)-P(D=1|Z=0)=E(D|Z=1)-E(D|Z=0)
$$

$$
E(Y|Z=1)-E(Y|Z=0)\overset{randomization}{=}E(Y(1)-Y(0))=\pi_a\tau_a+\pi_n\tau_n+\pi_c\tau_c+\pi_d\tau_d\overset{\tau_c=\tau_d=0}{=}\pi_d\tau_d+\pi_c\tau_c
$$

$$
\Rightarrow\frac{E(Y|Z=1)-E(Y|Z=0)}{E(D|Z=1)-E(D|Z=0)}-\tau_c=\frac{\pi_d(\tau_c+\tau_d)}{\pi_c-\pi_d}
$$

#### 22.10 One-sided noncompliance and statistical inference

One-sided noncompliance happens when $Z_i=0\Longrightarrow D_i=0$

Above statemnet imply that$pr(D=0|Z=0)=1$

##### 1、monotonocity Assumption 21.2 still hold because$D_i(0)=0\leq D_i(1)\ for\ i$

$$ U_i=\begin{cases}complier & D_i(1)=1\ and\ D_i(0)=0 \\
never-taker & D_i(1)=0\ and\ D_i(0)=0\\
\end{cases}$$

| Observed Group | latent Group |
|----------------|--------------|
| Z=1,D=0        | n            |
| Z=1,D=1        | c            |
| Z=0,D=0        | c,n          |

$$
pr(D=0|Z=1)=pr(U=n|Z=1)=pr(U=n)=\pi_n
$$

$$
pr(D=1|Z=1)=pr(U=c|Z=1)=pr(U=c)=\pi_c
$$

##### 2、exclusion restriction: $Y(1)=Y(0),for\ U=a,n$

$$
E\{Y|Z=1,D=0\}=E\{Y(1)|Z=1,U=n\}=E\{Y(1)|U=n\}=E\{Y(0)|U=n\}$$

$$
E\{Y|Z=1,D=1\}=E\{Y(1)|Z=1,U=c\}=E\{Y(1)|U=c\}
$$

$$
由于E\{Y|Z=0,D=0\}=E\{Y(0)|U=c\}\pi_c+E\{Y(1)|U=n\}\pi_n$$

$$
\Rightarrow E\{Y(1)|U=c\}=\pi_c^{-1}[E\{Y|Z=0,D=0\}-E\{Y|Z=1,D=0\}\pi_n]
$$

we know Assumption21.1-21.3 all hold here, and $pr(D=1|Z=0)=0$

so CACE can obtain by the way below

$$
CACE=E(Y(1)-Y(0)|U=c)=\frac{E(Y|Z=1)-E(Y|Z=0)}{E(D|Z=1)-E(D|Z=0)}=\frac{E(Y|Z=1)-E(Y|Z=0)}{E(D|Z=1)}
$$

##### 3、how to improve the estimation efficiency of CACE with covarites

Based on the question2, we obtain

$$
CACE=\frac{E(Y|Z=1)-E(Y|Z=0)}{E(D|Z=1)}
$$

if we have pretreatment covariate X

$$
\widehat{CACE}=\frac{E[Y|X,Z=1]-E[Y|X,Z=0]}{E[D|X,Z=1]}
$$

##### 4、IV inequalities$\clubsuit\ and\ \spadesuit$

$$
由E[Y(1)|U=c]=\frac{E\{Y|Z=0,D=0\}-E\{Y|Z=1,D=0\}\pi_n}{\pi_c}\geq0
$$

$$
可以得到E\{Y|Z=0,D=0\}\geq E\{Y,D=0|Z\}\clubsuit
$$

$$
另一方面由E[Y(0)|U=c]\pi_c+E[Y(0)|U=n]\pi_n=E[Y(0)]
$$

$$
可以得到不等式E[Y(0)|U=c]=\frac{E[Y|Z=0]-E[Y|Z=1,D=0]\pi_n}{\pi_c}\geq 0
$$

$$
因此E[Y|Z=0]\geq E[Y,D=0|Z=1]\spadesuit
$$

##### 5、Based on the data

$$
\widehat{CACE}=\frac{pr(Y=1|Z=1)-pr(Y=1|Z=0)}{pr(D=1|Z=1)}=\frac{\frac{12048}{12094}-\frac{11514}{11588}}{\frac{9675}{12094}}\approx0.003
$$

#### 23.1 More algebra for Two-stage least Squares

1、对于通过最小二乘方法得到的等式$\hat{D_i}=\hat{\Gamma}^TZ_i$

$$
\Rightarrow\hat{\Gamma}^T=E(Z_iZ_i^T)^{-1}E(Z_iD_i)Z_i=[n^{-1}\sum_{i=1}^nZ_iZ_i^T]^{-1}[n^{-1}\sum_{i=1}^nZ_iD_i^T]=\left(\sum_{i=1}^nZ_iZ_i^T\right)^{-1}\left(\sum_{i=1}^nZ_iD_i^T\right)
$$

2、基于（23.9）$\hat{\beta}_{TSLS}=\beta+\left\{\hat{\Gamma}^T\left(\dfrac{1}{n}\sum_iZ_iZ_i^T\right)\hat{\Gamma}\right\}^{-1}\hat{\Gamma}^T\left(\dfrac{1}{n}\sum_iZ_i\epsilon_i\right)$

$$
已知\frac{1}{n}\sum_iZ_iZ_i^T,\frac{1}{n}\sum_iZ_iD_i^T满秩可逆
$$

$$
\hat{\beta}_{TSLS}=\beta+\hat{\Gamma}^{-1}\left(\frac{1}{n}\sum_iZ_iZ_i^T\right)^{-1}\left(\frac{1}{n}\sum_iZ_i\epsilon_i\right)\overset{代入1的结果}{=}\beta+\left(\sum_iZ_iD_i^T\right)^{-1}\sum_iZ_i\epsilon_i
$$

$$
=\left(\sum_iZ_iD_i^T\right)^{-1}\left(\sum_iZ_iD_i^T\right)\beta+\left(\sum_iZ_iD_i^T\right)^{-1}\sum_iZ_i\epsilon_i=\left(\sum_iZ_iD_i^T\right)^{-1}\sum_iZ_i(D_i^T\beta+\epsilon_i)=\left(\sum_iZ_iD_i^T\right)^{-1}\sum_iZ_iY_i=\hat{\beta}_{IV}
$$

#### 23.3 Control function in the linear instrumental variable model

in control function estimator $\hat{\beta}_{CF}$ step 1, we run OLS of D on Z

$$
\check{D_i}=D_i-\hat{\beta}_{D\sim Z}Z_i=D_i-E[ZZ^T]^{-1}E[ZD]Z_i=D_i-\left(\sum_iZ_iZ_i^T\right)^{-1}\left(\sum_iZ_iD_i^T\right)Z_i=D_i-\hat{\Gamma}Z_i
$$

in step 2, we run OLS of Y on D and $\check{D}$, now the coveriate matrix $X=[D,\check{D}]^T$

$$
\hat{\beta}=(XX^T)^{-1}XY=\left([D,\check{D}]^T[D,\check{D}]\right)^{-1}[\sum_iD_iY_i,\sum_i\check{D_i}Y_i]^T=
\begin{bmatrix}
D^TD&D^T\check{D}\\
D^T\check{D}&\check{D}^T\check{D}
\end{bmatrix}^{-1}
[\sum_iD_iY_i,\sum_i\check{D_i}Y_i]^T
$$

$$
利用克莱姆法则\hat{\beta}_{CF}=\frac{\begin{vmatrix}D^TY&D^T\check{D}\\\check{D}^TY&\check{D}^T\check{D}\end{vmatrix}}{\begin{vmatrix}D^TD&D^T\check{D}\\D^T\check{D}&\check{D}^T\check{D}\end{vmatrix}}\overset{省略大量步骤}{=}\frac{(D-\check{D})^TY}{D^T(D-\check{D})}=\frac{\hat{D}^TY}{D^T\hat{D}}=\frac{\hat{D}^TY}{\hat{D}^T\hat{D}}=\left(\sum_i\hat{D_i}\hat{D_i}^T\right)^{-1}\sum_i\hat{D_i}Y_i=\hat{\beta}_{TSLS}
$$
