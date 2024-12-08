#### 10.2 Nonparametric identification of other causal effects

Under ignorability :$Y(z)\perp\!\!\!\perp Z|X$

$$
DCE_y=pr\{Y(1)>y\}-pr\{Y(0)>y\}
$$

$$
pr\{Y(1)>y\}=E[P\{Y(1)>y|X\}]=E[P\{Y(1)>y|Z=1,X\}]=E[P\{Y>y|Z=1,X\}]
$$

可以表示为观测数据的函数，所以是非参数可识别的

$$
QCE_y=quantile_q\{Y(1)\}-quantile_q\{Y(0)\}
$$

$$
quantile_q\{Y(1)\}=F^{-1}_{Y(1)}(q)\ where\ F_{Y(1)}(y)=1-P\{Y(1)>y\}
$$

由于F是非参数可识别的，所以其逆函数也是非参数可识别的，但是下面统计量是不可识别的

$$
quantile_q\{Y(1)-Y(0)\}
$$

#### 10.4 Data analysis: stratification and regression

##### (1)Stratified analysis

```
library(senstrat)
dt<-homocyst[,-c(1,4)]#去掉第1,4个变量SEQN,stf
t<-as.data.frame(table(dt$st, dt$z))
paste("have only treated or control units:",nrow(t[t$Freq==0, ]))
paste("the proportion:",(nrow(t[t$Freq==0, ]))/length(unique(t$Var1)))
paste("Drop these strata")
idx<-c(4,6,29,46,47,17,48,51,53,54, 57, 69, 76, 78, 94, 100, 105, 106)
dt_1<-dt[!dt$st %in% idx, ]#取反保证取出st不在a中的数据
```

```
var0 <- function(x) {
  if (length(x) == 1) {return(0)} 
  else {return(var(x))}
}#处理只有一个数据的c(3)，var0函数返回0
Neyman_SRE = function(z,y,x){
  xlevels = unique (x)#x作为block,y作为outcome,z作为treatment
  K = length (xlevels)
  PiK = rep(0,K)
  TauK = rep(0,K)
  varK = rep(0,K)
  for(k in 1:K){
    xk = xlevels[k]
    zk = z[x==xk]
    yk = y[x==xk]
    PiK [ k ] = length(zk)/length(z)
    TauK [ k ] = mean(yk[zk==1])-mean(yk[zk==0])
    varK [ k ] = var0(yk[zk==1])/sum(zk) + var0(yk[zk==0])/sum(1-zk)
  }
  return ( c(
    est = sum(PiK*TauK),#点估计
    se = sqrt(sum(PiK^2*varK)),#方差估计量
    CI_down = sum(PiK*TauK)-sqrt(sum(PiK^2*varK))*1.96,
    CI_up = sum(PiK*TauK)+sqrt(sum(PiK^2*varK))*1.96,
    pv = 1-pnorm(sum(PiK*TauK)/sqrt(sum(PiK^2*varK)))#标准正态检验p值
    ) ) 
}
Neyman_SRE(dt_1$z,dt_1$homocysteine,dt_1$st)
```

##### (2)Run the OLS

```
library(car)
#利用EHW方法得到robust standard error
fit1<-lm(homocysteine~.,data = dt[,-3])
fit2<-lm(homocysteine~.,data = dt_1[,-3])#drop the strata
out<-c(coef(fit1)[2],hccm(fit1)[2,2]^0.5,
       coef(fit2)[2],hccm(fit2)[2,2]^0.5)
out = t(matrix (out,nrow = 2 , ncol = 2))
colnames ( out) = c ("est","robust se")
rownames ( out ) = c ("OLS no drop","OLS drop")
out
```

##### (3)Lin's estimater

```
#Lin's estimater(注意需要对协变量做标准化,无论连续还是离散)
vars_to_scale <- c("female","age3", "ed3", "bmi3", "pov2")
for (var in vars_to_scale) {
  dt[[var]] <- scale(dt[[var]])
  dt_1[[var]] <- scale(dt_1[[var]])
}
fit1<-lm(homocysteine~.+z*female+z*age3+z*ed3+z*bmi3+z*pov2,data = dt[,-3])
fit2<-lm(homocysteine~.+z*female+z*age3+z*ed3+z*bmi3+z*pov2,data = dt_1[,-3])#drop
out<-c(coef(fit1)[2],hccm(fit1)[2,2]^0.5,
       coef(fit2)[2],hccm(fit2)[2,2]^0.5)
out = t(matrix (out,nrow = 2 , ncol = 2))
colnames ( out) = c ("est","robust se")
rownames ( out ) = c ("Lin no drop","Lin drop")
out
```

##### (4)Compare

Lin‘s estimater(OLS with interaction)会更可信：与Neyman_SRE的估计相比，其估计标准差se更小；与OLS方法相比，带covariate interaction的结果基于Lin的理论是更合理的方式。

#### 11.1 Prove

记$T=\{Y(0),Y(1),X\}$那么$e(Y(0),Y(1),X)=P(Z=1|Y(0),Y(1),X)=E[Z|Y(0),Y(1),X]=E[Z|T]=g(T)$

$$
一方面：P(Z=1|Y(0),Y(1),X,e(Y(0),Y(1),X))=E[Z|T,g(T)]=E[Z|T]=g(T)
$$

$$
另一方面：P(Z=1|e(Y(0),Y(1),X))=E[Z|g(T)]=E[\ E[Z|T,g(T)]\ |g(T)]=E[g(T)|g(T)]=g(T)
$$

$$
\Rightarrow P(Z=1|Y(0),Y(1),X,e(Y(0),Y(1),X))=P(Z=1|e(Y(0),Y(1),X))
$$

$$
\Leftrightarrow Z \perp\!\!\!\perp\{Y(1),Y(0),X\}|e(Y(0),Y(1),X)
$$

#### 11.3 Prove

$$
Proposition\ 1:\frac{1}{n}\sum_{i=1}^n\frac{Z_i(Y_i+c)}{\hat{e}(X_i)}-\frac{1}{n}\sum_{i=1}^n\frac{(1-Z_i)(Y_i+c)}{1-\hat{e}(X_i)}=\hat{\tau}^{ht}+c\left[\frac{1}{n}\sum_{i=1}^n\frac{Z_i}{\hat{e}(X_i)}-\frac{1}{n}\sum_{i=1}^n\frac{(1-Z_i)}{\hat{e}(X_i)}\right]
$$

$$
E\left\{\frac{1}{n}\sum_{i=1}^n\frac{Z_i}{e(X_i)}\right\}=\frac{1}{n}\sum_{i=1}^nE\left[\frac{Z_i}{e(X_i)}\right]=\frac{1}{n}\sum_{i=1}^nE\left[E\left[\frac{Z_i}{e(X_i)}|X_i\right]\right]=\frac{1}{n}\sum_{i=1}^nE\left[\frac{1}{e(X_i)}E[Z_i|X_i]\right]=\frac{1}{n}\sum_{i=1}^nE\left[\frac{e(X_i)}{e(X_i)}\right]=1
$$

$$
注意到在强可忽略性假设下，E[Z_i|X_i]=P(Z_i=1|X_i)=e(X_i)同理可证得E\left\{\frac{1}{n}\sum_{i=1}^n\frac{(1-Z_i)}{1-e(X_i)}\right\}=1
$$

$$
Hajek\ estimator\ remains\ the\ same:\frac{\sum_{i=1}^n\frac{Z_i(Y_i+c)}{\hat{e}(X_i)}}{\sum_{i=1}^n\frac{Z_i}{\hat{e}(X_i)}}-\frac{\sum_{i=1}^n\frac{(1-Z_i)(Y_i+c)}{1-\hat{e}(X_i)}}{\sum_{i=1}^n\frac{1-Z_i}{1-\hat{e}(X_i)}}=\hat{\tau}^{hajek}+c\left(\frac{\sum_{i=1}^n\frac{Z_i}{\hat{e}(X_i)}}{\sum_{i=1}^n\frac{Z_i}{\hat{e}(X_i)}}-\frac{\sum_{i=1}^n\frac{(1-Z_i)}{1-\hat{e}(X_i)}}{\sum_{i=1}^n\frac{1-Z_i}{1-\hat{e}(X_i)}}\right)=\hat{\tau}^{hajek}
$$

#### 11.5 Prove Theorem 11.5

$$
要证明Z\perp\!\!\!\perp X|b(X)\Leftrightarrow e(X)=f(b(X))
$$

$$
证明\Rightarrow:已知E[Z|X,b(X)]=P(Z=1|X,b(X))=P(Z=1|b(X))=E[Z|b(X)]\ \ \ \ \ \ (*)
$$

$$
(*)式左边：E[Z|X,b(X)]=E[Z|X]=e(X)
$$

$$
(*)式右边：E[Z|b(X)]=E[\ E[Z|e(X),b(X)]\ |b(X)]\overset{Theorem}{=}E[\ E[Z|X,e(X),b(X)]\ |b(X)]=E[\ E[Z|X]\ |b(X)]=E[e(X)|b(X)]
$$

$$
所以可以推出:e(X)=E[e(X)|b(X)]\Leftrightarrow e(X)=f(b(X))\ for \ some\ f(.)
$$

$$
证明\Leftarrow:注意到上面的推导都是双向的，基于e(X)=f(b(X))=E[e(X)|b(X)]可反方向证明
$$

#### 11.6 Some basics of subgroup effects

我们已经证明在Strong Ingorability和Overlap condition下有$$E\{Y(1)\}=E\{\frac{ZY}{e(X)}\},E\{Y(0)\}=\frac{(1-Z)Y}{1-e(X)}$$

那么对于$X_1=x_1$的子总体利用上面的证明

$$
E\{Y(1)|X_1=x_1\}\times pr(X_1=x_1)=\sum_y\frac{pr(Y(1)=y,X_1=x_1)}{pr(X_1=x_1)}pr(X_1=x_1)=E\{\mathbf{1}(X_1=x_1)Y(1)\}=E\{\frac{\mathbf{1}(X_1=x_1)ZY}{e(X)}\}
$$

$$
同理:E\{Y(0)|X_1=x_1\}\times pr(X_1=x_1)=E\{\frac{\mathbf{1}(X_1=x_1)(1-Z)Y}{1-e(X)}\}
$$

$$
综上:\tau(x_1)=E\{Y(1)-Y(0)|X_1=x_1\}=E\left\{\frac{\mathbf{1}(X_1=x_1)ZY}{e(X)}-\frac{\mathbf{1}(X_1=x_1)(1-Z)Y}{1-e(X)}\right\}/pr(X_1=x_1)
$$

$$
\hat{\tau}^{ht}=\left[\frac{1}{n}\sum_{i=1}^n\frac{Z_iY_i}{\hat{e}(X_i)}-\frac{1}{n}\sum_{i=1}^{n}\frac{(1-Z_i)X_i}{1-\hat{e}(X_i)}\right]/pr(X_1=x_1)
$$

$$
\hat{\tau}^{hajck}=\frac{\sum_i\dfrac{\mathbf{1}(X_1=x_1)Z_iY_i}{\hat{e}(X_i)}}{\sum_i\dfrac{\mathbf{1}(X_1=x_1)Z_i}{\hat{e}(X_i)}}-\frac{\sum_i\dfrac{\mathbf{1}(X_1=x_1)(1-Z_i)Y_i}{1-\hat{e}(X_i)}}{\sum_i\dfrac{\mathbf{1}(X_1=x_1)(1-Z_i)}{1-\hat{e}(X_i)}}
$$

#### 12.2 An alternative form of the doubly robust estimator for tau

$$
\widetilde{\mu}_1^{dr2}-\mu_1=\frac{E\left[\frac{Z\{Y-\mu_1(X,\beta_1)\}}{e(X,\alpha)}\right]}{E\left[\frac{Z}{e(X,\alpha)}\right]}-E[Y(1)-\mu_1(X,\beta_1)]=\frac{E\left\{E\left[\frac{Z\{Y-\mu_1(X,\beta_1)\}}{e(X,\alpha)}|X\right]\right\}}{E\left\{E\left[\frac{Z}{e(X,\alpha)}|X\right]\right\}}-E\{E[Y(1)-\mu_1(X,\beta_1)|X]\}
$$

$$
=\frac{E[\frac{e(X)}{e(X,\alpha)}\times\frac{\mu_1(X)-\mu_1(X,\beta_1)}{e(X,\alpha)}]}{E[\frac{e(X)}{e(X,\alpha)}]}-E[\mu_1(X)-\mu_1(X,\beta_1)]=0,if\ either\ e(X,\alpha)=e(X)\ or\ \mu_1(X,\beta_1)=\mu_1(X)
$$

$$
\widetilde{\mu}_0^{dr2}=\frac{E\left[\frac{(1-Z)\{Y-\mu_1(X,\beta_0)\}}{1-e(X,\alpha)}\right]}{E\left[\frac{1-Z}{1-e(X,\alpha)}\right]}+E[\mu_1(X,\beta_0)]
$$

$$
\hat{\mu_1}^{dr}=\frac{\frac{1}{n}\sum_{i=1}^n\left[\frac{Z_i\{Y_i-\mu_1(X_i,\hat{\beta}_1)\}}{e(X_i,\hat{\alpha})}\right]}{\frac{1}{n}\sum_{i=1}^n\left[\frac{Z_i}{e(X_i,\hat{\alpha})}\right]}+\frac{1}{n}\sum_i\mu_1(X_i,\hat{\beta}_1)
$$

$$
\hat{\mu_0}^{dr}=\frac{\frac{1}{n}\sum_{i=1}^n\left[\frac{(1-Z_i)\{Y_i-\mu_0(X_i,\hat{\beta}_0)\}}{1-e(X_i,\hat{\alpha})}\right]}{\frac{1}{n}\sum_{i=1}^n\left[\frac{1-Z_i}{1-e(X_i,\hat{\alpha})}\right]}+\frac{1}{n}\sum_i\mu_0(X_i,\hat{\beta}_0)
$$

#### Average treatment effect on the treated

##### (a) Show ATT(Based on $Z\perp\!\!\!\perp Y(0)|X$

$$
ATT=E[Y(1)|Z=1]-E[Y(0)|Z=1]=E[Y|Z=1]-E\{\ E[Y(0)|Z=1,X]\ |Z=1\}
$$

$$
=E[Y|Z=1]-E\{\ E[Y(0)|Z=0,X]\ |Z=1\}=E[Y|Z=1]-E\{\ E[Y|Z=0,X]\ |Z=1\}
$$

##### (b)estimator of ATT based on OLS

$$
基于(a)和线性模型假设E[Y|Z=0,X]=\beta_0+\beta^{T}_xX
$$

$$
\tau_{T}=E[Y|Z=1]-E\{\beta_0+\beta_x^TX|Z=1\}=E[Y|Z=1]-\beta_0-\beta_x^TE[X|Z=1]
$$

$$
\Rightarrow \hat{\tau}_T=\hat{\overline{Y}}(1)-\hat{\beta}_0-\hat{\beta}_x^T\hat{\overline{X}}(1)
$$

##### (c)Show that IPW of ATT

$$
\tau_T=E[Y|Z=1]-E\left[\frac{e(X)}{pr(Z=1)}\frac{1-Z}{1-e(X)}Y\right]\ and\ define\ \hat{o}(X_i)=\frac{\hat{e(X_i)}}{1-\hat{e}(X_i)}
$$

$$
\hat{\tau}_T^{ht}=\hat{\overline{Y}}(1)-\frac{1}{\dfrac{n_1}{n}}\frac{1}{n}\sum_{i=1}^n\frac{\hat{e}(X_i)(1-Z_i)}{1-\hat{e}(X_i)}Y_i=\hat{\overline{Y}}(1)-\frac{1}{n_1}\sum_{i=1}^n\hat{o}(X_i)(1-Z_i)Y_i
$$

$$
\hat{\tau_T}^{hajek}=\hat{\overline{Y}}(1)-\frac{\frac{1}{\frac{n_1}{n}}\frac{1}{n}\sum_{i=1}^n\frac{\hat{e}(X_i)(1-Z_i)}{1-\hat{e}(X_i)}Y_i}{\frac{1}{\dfrac{n_1}{n}}\frac{1}{n}\sum_{i=1}^n\frac{\hat{e}(X_i)(1-Z_i)}{1-\hat{e}(X_i)}}=\hat{\overline{Y}}(1)-\frac{\sum_i\hat{o}(X_i)(1-Z_i)Y_i}{\sum_i\hat{o}(X_i)(1-Z_i)}
$$

##### (d)double robust estimator for ATT

$$
对于E[Y(0)|Z=1]=E[Y(0)-\mu_0(X,\beta_0)|Z=1]+E[\mu_0(X,\beta_0)|Z=1]那么可以构造估计量
$$

$$
\widetilde{\mu}_{T}^{dr}=E\left[\frac{e(X,\alpha)(1-Z)}{1-e(X,\alpha)}\{Y-\mu_0(X,\beta_0)\}|Z=1\right]+E[\mu_0(X,\beta_0)|Z=1]=E\left[\frac{e(X,\alpha)(1-Z)}{1-e(X,\alpha)}\{Y-\mu_0(X,\beta_0)\}+Z\mu_0(X,\beta_0)\right]/pr(Z=1)
$$

$$
\hat{\tau}_T^{dr}=\hat{\overline{Y}}(1)-\hat{\mu}_T^{dr}=\hat{\overline{Y}}(1)-\frac{1}{n_1}\sum_{i=1}^n\left[o(X_i,\hat{\alpha})(1-Z_i)\{Y_i-\mu_0(X_i,\hat{\beta_0})\}+Z_i\mu_0(X_i,\hat{\beta}_0)\right]
$$

prove the double robustness

$$
pr(Z=1)\times\left[\widetilde{\mu}_T^{dr}-E[Y(0)|Z=1]\right]=E\left[o(X,\alpha)(1-Z)\{Y-\mu_0(X,\beta_0)\}+Z\mu_0(X,\beta_0)-ZY(0)\right]
$$

$$
=E[\{o(X,\alpha)(1-Z)-Z\}\{Y(0)-\mu_0(X,\beta_0)\}]=E\left[\frac{e(X,\alpha)}{1-e(X,\alpha)}\{Y(0)-\mu_0(X,\beta_0)\}\right]
$$

$$
=E\left[E\left\{\frac{e(X,\alpha)-Z}{1-e(X,\alpha)}|X\right\}\times E\{Y(0)-\mu_0(X,\beta_0)|X\}\right]=E\left[\frac{e(X,\alpha)-e(X)}{1-e(X,\alpha)}\times\{\mu_0(X)-\mu_0(X,\beta_0)\}\right]
$$

上式子为0当$e(X,\alpha)=e(X)$或者$\mu_0(X,\beta_0)=\mu_0(X)$其中一个成立即可

#### Stimulation for ATT estimators in Observational studies

##### (a)goal

我们已经得到ATT的double robust estimator，我们希望通过模拟数据来验证$\hat{\tau}_T^{dr}$估计量的双稳健性

-   case1：两个模型假设都是正确的

-   case2：倾向性得分模型假设是错误的

-   case3：结果变量模型假设是错误的

-   case4：两个模型假设都是错误的

    ```
    ATT.est = function (z,y,x,out.family = gaussian, Utruncps = 1){
      ## sample size
      nn = length(z);nn1 = sum (z)
      ## fitted propensity score
      pscore = glm(z~x,family = binomial)$ fitted.values
      pscore = pmin ( Utruncps , pscore )
      odds = pscore/(1-pscore)
      ## fitted potential outcomes
      outcome0 = glm(y~x,weights=(1-z),family=out.family)$fitted.values
      ## outcome regression estimator
      ace.reg0 = lm (y~z+x)$coef[2]
      ace.reg = mean(y[z==1])-mean(outcome0[z==1])
      ## propensity score weighting estimator
      ace.ipw0 = mean(y[z==1])-mean(odds*(1-z)*y)*nn/nn1
      ace.ipw = mean(y[z==1])-mean (odds*(1-z)*y)/mean(odds*(1-z))
      ## doubly robust estimator
      res0 = y - outcome0
      ace.dr = ace.reg-mean(odds*(1-z)*res0)*nn/nn1
      return ( c ( ace.reg , ace.ipw0 , ace.ipw , ace.dr ))
    }
    OS_ATT = function(z,y,x ,n.boot = 10^2,out.family = gaussian,Utruncps = 1){
      point.est = ATT.est (z,y,x,out.family,Utruncps)
      ## nonparametric bootstrap
      n = length(z)
      x = as.matrix(x)
      boot.est = replicate(n.boot,{
        id.boot = sample(1:n,n,replace = TRUE)
        ATT.est(z[id.boot],y[id.boot],x[id.boot, ],out.family,Utruncps )
        })
      boot.se = apply ( boot.est , 1 , sd )
      res = rbind ( point.est , boot.se )
      rownames ( res ) = c ( " est " , " se " )
      colnames ( res ) = c ( " reg " , " HT " , " Hajek " , " DR " )
      return ( res )
    }
    ```

##### (b)data generating mechanism

所有的数据生成机制都应该使得真实ATT值为0，即$\tau_T=E[Y|Z=1]-E[Y(0)|Z=1]=0$

结果变量模型要求$E[Y|Z=0,X]=\beta_0+\beta_1X$

倾向性得分模型要求$logit[e(X)]=\beta_0+\beta_1X$

##### (c)display

```
n = 500
#case1
print("case1:两个模型假设都是正确的")
x = matrix(rnorm(n*2), n, 2)
x1 = cbind(1, x)
beta.z = c(0, 1, 1)
pscore = 1/(1 + exp(- as.vector(x1%*%beta.z)))
z = rbinom(n, 1, pscore)
beta.y1 = c(1, 2, 1)
beta.y0 = c(1, 2, 1)
y1 = rnorm(n, x1%*%beta.y1);y0 = rnorm(n, x1%*%beta.y0)
y = z*y1 + (1 - z)*y0
OS_ATT(z,y,x)
#case2
print("case2:倾向性得分模型假设是错误的")
x = matrix(rnorm(n*2), n, 2)
x1 = cbind(1, x, exp(x))
beta.z = c(-1, 0, 0, 1, -1)
pscore = 1/(1 + exp(- as.vector(x1%*%beta.z)))
z = rbinom(n, 1, pscore)
beta.y1 = c(1, 2, 1, 0, 0)
beta.y0 = c(1, 1, 1, 0, 0)
y1 = rnorm(n, x1%*%beta.y1);y0 = rnorm(n, x1%*%beta.y0)
y = z*y1 + (1 - z)*y0
OS_ATT(z,y,x)
#case3
print("case3:结果变量模型假设是错误的")
x = matrix(rnorm(n*2), n, 2)
x1 = cbind(1, x, exp(x))
beta.z = c(0, 1, 1, 0, 0)
pscore = 1/(1 + exp(- as.vector(x1%*%beta.z)))
z = rbinom(n, 1, pscore)
beta.y1 = c(1, 0, 0, 2, -1)
beta.y0 = c(1, 0, 0, -2, 1)
y1 = rnorm(n, x1%*%beta.y1);y0 = rnorm(n, x1%*%beta.y0)
y = z*y1 + (1 - z)*y0
OS_ATT(z,y,x)
#case4
print("case4:两个模型假设都是错误的")
x = matrix(rnorm(n*2), n, 2)
x1 = cbind(1, x, exp(x))
beta.z = c(-1, 0, 0, 1, -1)
pscore = 1/(1 + exp(- as.vector(x1%*%beta.z)))
z = rbinom(n, 1, pscore)
beta.y1 = c(1, 0, 0, 2, -1)
beta.y0 = c(1, 0, 0, -2, 1)
y1 = rnorm(n, x1%*%beta.y1);y0 = rnorm(n, x1%*%beta.y0)
y = z*y1 + (1 - z)*y0
OS_ATT(z,y,x)
```

##### (d)explain

从上面的结果我们可以看出，结果变量模型或倾向性得分模型中只要有一个假设正确，DR估计量就接近于正确模型的拟合结果。因此我们可以说明该估计量具有双稳健性。