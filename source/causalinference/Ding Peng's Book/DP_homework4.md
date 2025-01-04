#### 13.6 Estimating individual effect and conditional average casual effect

Assume $\{Z_i,X_i,Y_i(1),Y_i(0)\}_i^n\overset{IID}{\sim}\{Z,X,Y(1),Y(0)\}$

##### (1)Under $Z_i\perp\!\!\!\perp\{Y_i(1),Y_i(0)\}$ and $e=pr(Z_i=1)$

$$
E(\delta_i-\tau_i)=E\left(\left[\frac{Z_iY_i}{e}-Y_i(1)\right]-\left[\frac{(1-Z_i)Y_i}{1-e}-Y_i(0)\right]\right)=E[Y_i(1)]-E[Y_i(1)]-E[Y_i(0)]+E[Y_i(0)]
$$

$$
上面推导基于：E\left(\left[\frac{Z_i(Z_iY_i(1)+(1-Z_i)Y_i(0))}{e}-Y_i(1)\right]\right)=\frac{1}{e}E[Z_i^2Y_i(1)+Z_i(1-Z_i)Y_i(0)]-E[Y_i(1)]=E[Y_i(1)]-E[Y_i(1)]
$$

$$
那么：E\delta_i=E\tau_i=E[Y_i(1)-Y_i(0)]=\tau\ for\ all\ i=1...n
$$

##### (2)Under $Z_i\perp\!\!\!\perp\{Y_i(1),Y_i(0)\}|X_i$ and $e(X_i)=pr(Z_i=1|X_i)$

$$
基于E\left(\frac{Z_iY_i}{e(X_i)}-Y_i(1)\right)=E\left[E\left(\frac{Z_iY_i}{e(X_i)}-Y_i(1)|X_i\right)\right]=E\left[\frac{1}{e(X_i)}E(Z_iY_i|X_i)-E(Y_i(1)|X_i)\right]=E\left[\frac{1}{e(X_i)}e(X_i)E(Y_i(1)|X_i)-E(Y_i(1)|X_i)\right]=0
$$

$$
因此：E(\delta_i-\tau_i)=E\left(\left[\frac{Z_iY_i}{e(X_i)}-Y_i(1)\right]-\left[\frac{(1-Z_i)Y_i}{1-e(X_i)}-Y_i(0)\right]\right)=0-0=0
$$

$$
另外：E\{\delta_i-\tau(X_i)\}=E\delta_i-E(E\{Y_i(1)-Y_i(0)|X_i\})=E\delta_i-E(Y_i(1)-Y_i(0))=E(\delta_i-\tau_i)=0
$$

$$
同样E\delta_i=E\tau_i=\tau\ for\ all\ i=1...n
$$

#### 13.10 Doubly robust estimation for general estimate

Under ignorability and overlap

$$
\widetilde{\mu}_1^{h,dr}-E\{h(X)Y(1)\}=E\left[\frac{Zh(X)\{Y-\mu_1(X,\beta_1)\}}{e(X,\alpha)}-h(X)\{Y(1)-\mu_1(X,\beta_1)\}\right]=E\left[h(X)\{Y(1)-\mu_1(X,\beta_1)\}\frac{Z-e(X,\alpha)}{e(X,\alpha)}\right]
$$

$$
=E\left(E\left[\frac{Z-e(X,\alpha)}{e(X,\alpha)}h(X)\{Y(1)-\mu_1(X,\beta_1)\}|X\right]\right)=E\left(E\left[\frac{Z-e(X,\alpha)}{e(X,\alpha)}|X\right]\times E[h(X)|X]\times E[Y(1)-\mu_1(X,\beta_1)|X]\right)
$$

$$
=E\left(\frac{e(X)-e(X,\alpha)}{e(X,\alpha)}\times h(X)\{\mu_1(X)-\mu_1(X,\beta_1)\}\right)对\widetilde{\mu}_0^{h,dr}-E\{h(X)Y(0)\}也是同理
$$

另外$$\frac{\widetilde{\mu}_1^{h,dr}-\widetilde{\mu}_0^{h,dr}}{E\{h(X)\}}\overset{doubly \ robust\ condition}{=}\frac{E\{h(X)Y(1)\}-E\{h(X)Y(0)\}}{E\{h(X)\}}=\frac{1}{E\{h(X)\}}E\{h(X)[Y(1)-Y(0)]|X\}=\frac{E\{h(X)\tau(X)\}}{E\{h(X)\}}=\tau^h$$

#### 14.3 Weighted logistic regression with a binary outcome

考虑加权最小二乘（对Z=1的情况）

$$
min\sum_{Z_i=1}w_i(Y_i-\beta X_i)^2
$$

现在来考虑对逻辑斯特回归的加权想法，利用极大似然估计方法

$$
P(Y_i=1|Z_i)=\mu(X_i,\beta)=\frac{e^{\beta_0+\beta_1X_i}}{1+e^{\beta_0+\beta_1X_i}}
$$

$$
L=\prod_{Z_i=1}\mu(X_i,\beta)^{Y_i}\{1-\mu(X_i,\beta)\}^{1-Y_i}=\prod_{Z_i=1}\frac{e^{Y_i(\beta_0+\beta_1X_i)}}{1+e^{\beta_0+\beta_1X_i}}
$$

那么类似的，取对数函数后我们也可以引入权重，目标函数变成了

$$
min\sum_{Z_i=1}w_i\left\{log[e^{Y_i(\beta_0+\beta_1X_i)}]-log[1+e^{\beta_0+\beta_1X_i}]\right\}
$$

对$\alpha+\beta X_i$的部分求导，令导数为零求极值点

$$
\sum_{Z_i=1}w_i[Y_i-\mu(X_i,\beta)]=0,where\ w_i=\frac{1}{\hat{e}(X_i)}
$$

#### 17.2 Technical assumption for Theorem 17.1

Assume $Z\perp\!\!\!\perp Y\ |\ (X,U)$

$$
RR_{ZY|x}^{obs}=\frac{pr(Y=1|Z=1,X=x)}{pr(Y=1|Z=0,X=x)}\overset{Theorem}{=}\frac{f_{1,x}RR_{UY|x}+1-f_{1,x}}{f_{0,x}RR_{UY|x}+1-f_{0,x}}=\frac{(RR_{UY|x}-1)f_{1,x}+1}{\dfrac{RR_{UY|x}-1}{RR_{ZU|x}}f_{1,x}+1}
$$

$$
其中\frac{f_{1,x}}{f_{0,x}}=\frac{pr(U=1|Z=1,X=x)}{pr(U=1|Z=0,X=x)}=RR_{ZU|x}
$$

$$
由于:f(x)=\frac{ax+1}{bx+1}\ is\ increasing\ in\ x\ if\ a>b
$$

$$
if\ RR_{ZU|x}>1\ (RR_{ZU|x}=1+a)\ and\ RR_{UY|x}<1\Rightarrow(RR_{UY|x}-1)<\frac{RR_{UY|x}-1}{RR_{ZU|x}}
$$

$$
\Rightarrow RR_{ZY|x}^{obs}\leq\frac{RR_{ZU|x}RR_{UY|x}}{RR_{ZU|x}+RR_{UY|x}-1}=\frac{(1+a)RR_{UY|x}}{RR_{UY|x}+a}<1
$$

#### 17.6 Cornfield-type inequalities for the risk difference

Consider binary$Z,Y,U$ and condition on $X$ implicitly. Under ignorability$Z\perp\!\!\!\perp Y|(X,U)$

$$
RD_{ZU}\times RD_{UY}=[P(U=1|Z=1,X=x)-P(U=1|Z=0,X=x)]\times[P(Y=1|U=1,X=x)-P(Y=1|U=0,X=x)]=([1]-[2])\times([3]-[4])
$$

$$
[1]\times[3]=\frac{P(U=1|Z=1,X=x)P(Y=1|U=1,X=x)P(U=1,X=x)}{P(U=1,X=x)}=P(Y=1,U=1|Z=1,X=x)
$$

$$
[2]\times[3]=\frac{P(U=1|Z=0,X=x)P(Y=1|U=1,X=x)P(U=1,X=x)}{P(U=1,X=x)}=P(Y=1,U=1|Z=0,X=x)
$$

$$
[2]\times[4]=P(Y=1|U=0,X=x)-P(Y=1,U=0|Z=0,X=x)
$$

$$
[1]\times[4]=P(Y=1|U=0,X=x)-P(Y=1,U=0|Z=1,X=x)
$$

$$
\Rightarrow RD_{ZU}\times RD_{UY}=[1]\times[3]+[2]\times[4]-[2]\times[3]-[2]\times[4]=P(Y=1|Z=1,X=x)-P(Y=1|Z=0,X=x)=RD_{ZY|x}^{obs}
$$

#### 20.1 Prove Theorem 20.3

Given that $Z\perp\!\!\!\perp Y(z)|X\ and\ E\{Y(z)|X\}=f_z(r(X))$

$$
E[Y|Z=1,r(X)]=E[ZY(1)|Z=1,r(X)]=E[Y(1)|r(X)]=E\{E[Y(1)|X,r(X)]\ |\ r(X)\}=E\{E[Y(1)|X]\ |\ r(X)\}
$$

$$
=E\{f_1[r(X)]|r(X)\}=f_1[r(X)]=E[Y(1)|X]
$$

$$
\Rightarrow\tau=E\{E(Y|Z=1,r(X))-E(Y|Z=0,r(X))\}=E\{E(Y(1)|X)-E(Y(0)|X)\}=E[Y(1)-Y(0)] 
$$

$$
E\left\{\frac{ZY}{e[r(X)]}\right\}=E\left\{E\left[\frac{ZY}{e[r(X)]}|r(X)\right]\right\}=E\left\{\frac{1}{e[r(X)]}E[ZY|r(X)]\right\}=E\left\{\frac{1}{e[r(X)]}E\{E[ZY|X,r(X)]\ |\ r(X)\}\right\}
$$

$$
=E\left\{\frac{1}{e[r(X)]}E\{E[Z|X,r(X)\times E[Y|X,r(X)]\ |\ r(X)\}\right\}=E\left\{\frac{E\{f_1[r(X)]\}}{e[r(X)]}E\{E[Z|X,r(X)]\ |\ r(X)\}\right\}
$$

$$
=E\left\{\frac{E\{f_1[r(X)]\}}{e[r(X)]}E[Z|r(X)]\right\}=E\{E\{f_1[r(X)]\}\}=E\{Y(1)\}
$$

$$
\Rightarrow\tau=E\left\{\frac{ZY}{e[r(X)]}\right\}-E\left\{\frac{(1-Z)Y}{1-e[r(X)]}\right\}=E\{Y(1)\}-E\{Y(0)\}
$$
