#### 4.1 Proof of Lemma 4.1

$$
Given\ S^2(\tau)=\frac{1}{n-1}\sum_{i=1}^n(\tau_i-\tau)^2=\frac{1}{n-1}\sum_{i=1}^n\left\{[Y_i(1)-\overline{Y(1)}]-[Y_i(0)-\overline{Y(0)}]\right\}^2
$$

$$
S^2(1)+S^2(0)-S^2(\tau)=\frac{1}{n-1}\sum_{i=1}^n\left\{[Y_i(1)-\overline{Y(1)}]\right\}^2+\frac{1}{n-1}\sum_{i=1}^n\left\{[Y_i(0)-\overline{Y(0)}]\right\}^2-\frac{1}{n-1}\sum_{i=1}^n\left\{[Y_i(1)-\overline{Y(1)}]-[Y_i(0)-\overline{Y(0)}]\right\}^2
$$

$$
=2\times\frac{1}{n-1}\sum_{i=1}^n\left\{[Y_i(1)-\overline{Y(1)}][Y_i(0)-\overline{Y(0)}]\right\}=2S(0,1)
$$

#### 4.2 Alternative proof of Theorem 4.1

Given the proof of Lemma C.2

$$
Var\{\hat{\overline{Y}}(1)\}=Var(\frac{1}{n_1}\sum_{i=1}^nZ_iY_i)=\frac{n_0}{nn_1}\frac{1}{n_1-1}\sum_{i=1}^nZ_i(Y_i-\hat{\overline{Y}}(1))^2=\frac{n_0}{nn_1}S^2(1)
$$

$$
Var\{\hat{\overline{Y}}(0)\}=Var\{\frac{1}{n_0}\sum_{i=1}^n(1-Z_i)Y_i\}=\frac{n_1}{nn_0}\frac{1}{n_0-1}\sum_{i=1}^n(1-Z_i)(Y_i-\hat{\overline{Y}}(0))^2=\frac{n_1}{nn_0}S^2(0)
$$

$$
Cov\{\hat{\overline{Y}}(1),\hat{\overline{Y}}(0)\}=Cov\left\{\frac{1}{n_1}\sum_{i=1}^nZ_iY_i,\frac{1}{n_0}\sum_{i=1}^n(1-Z_i)Y_i\right\}=\frac{1}{n_1n_0}Cov\left\{\sum_{i=1}^nZ_i(Y_i-\hat{\overline{Y}}(1)),\sum_{i=1}^n(1-Z_i)(Y_i-\hat{\overline{Y}}(0))\right\}
$$

$$
=-\frac{1}{n_1n_0}Cov\left\{\sum_{i=1}^nZ_i(Y_i-\hat{\overline{Y}}(1)),\sum_{i=1}^nZ_i(Y_i-\hat{\overline{Y}}(0))\right\}=-\frac{1}{n}S(1,0)
$$

Therefore

$$
Var(\hat{\tau})=Var(\hat{\overline{Y}}(1)-\hat{\overline{Y}}(0))=Var\{\hat{\overline{Y}}(1)\}+Var\{\hat{\overline{Y}}(0)\}-2Cov\{\hat{\overline{Y}}(1),\hat{\overline{Y}}(0)\}
$$

$$
=\frac{n-n_1}{nn_1}S^2(1)+\frac{n-n_0}{nn_0}S^2(0)+\frac{2}{n}S(1,0)=\frac{1}{n_1}S^2(1)+\frac{1}{n_2}S^2(0)-\frac{1}{n}\left(S^2(1)+S^2(0)-2S(1,0)\right)
$$

$$
=\frac{S^2(1)}{n_1}+\frac{S^2(0)}{n_2}-\frac{S^2(\tau)}{n}
$$

#### 4.3 Neymanian Inference and OLS

Given the method of OLS: $(\hat{\alpha},\hat{\beta})=arg\ \underset{(a,b)}{min}\sum_{i=1}^n(Y_i-a-bZ_i)^2$

$$
\hat{\beta}=\frac{n\sum Z_iY_i-\sum Z_i\sum Y_i}{n\sum Z_i^2-(\sum Z_i)^2}=\frac{n\sum Z_iY_i-n_1\sum(1-Z_i)Y_i-n_1\sum Z_iY_i}{nn_1-n_1^2}=\frac{\sum Z_iY_i}{n_1}-\frac{\sum(1-Z_i)Y_i}{n_0}=\hat{\tau}\ \ \ \ (4.3)
$$

Given that$\overline{Y}=\hat{\alpha}+\hat{\beta}\overline{Z}=\hat{\alpha}+\hat{\tau}\frac{n_1}{n}\Rightarrow \hat{\alpha}=\overline{Y}-\hat{\tau}\frac{n_1}{n}$

$$
\hat{Cov}\left(\begin{array}{cccc} \hat{\alpha} \\\hat{\beta} \\ \end{array}\right)=\hat{\sigma}^2(xx^T)^{-1}=\hat{\sigma}^2\left(
\begin{bmatrix}1&...&1\\Z_1&...&Z_n\end{bmatrix}\begin{bmatrix}1&...&1\\Z_1&...&Z_n\\\end{bmatrix}^T\right)^{-1}=\hat{\sigma}^2\begin{bmatrix}\dfrac{1}{n_0}&-\dfrac{1}{n_0}\\-\dfrac{1}{n_0}&\dfrac{n}{n_0n_1}\\\end{bmatrix}
$$

$$
\hat{V}_{ols}=\hat{\sigma}^2\dfrac{n}{n_0n_1}=\frac{n\sum_{i=1}^n\hat{\epsilon_i}^2}{n_0n_1(n-2)}=\frac{n\sum_{i=1}^n(Y_i-\overline{Y}+\hat{\tau}\frac{n_1}{n}-\hat{\beta}Z_i)^2}{n_1n_0(n-2)}=\frac{n\sum(Y_i-\frac{n_1}{n}\hat{Y}(1)-\frac{n_0}{n}\hat{Y}(0)+\frac{n_1}{n}\hat{Y}(1)-\frac{n_1}{n}\hat{Y}(0)-\hat{\tau}Z_i)^2}{n_0n_1(n-2)}
$$

$$
=\frac{n_0n_1}{n(n-2)}\sum_{i=1}^n\left[Y_i-Z_i\hat{Y}(1)-(1-Z_i)\hat{Y}(0)\right]^2=\frac{n_0n_1}{n(n-2)}\sum_{i=1}^n\left[Z_i(Y_i-\hat{Y}(1))+(1-Z_i)(Y_i-\hat{Y}(0))\right]^2
$$

$$
=\frac{n}{n_0n_1(n-2)}\sum_{i=1}^nZ_i(Y_i-\hat{Y}(1))^2+\frac{n}{n_0n_1(n-2)}\sum_{i=1}^n(1-Z_i)(Y_i-\hat{Y}(0))^2=\frac{n(n_1-1)}{n_0n_1(n-2)}\hat{S}^2(1)+\frac{n(n_0-1)}{n_0n_1(n-2)}\hat{S}^2(0)\ \ \ (4.4)
$$

$$
\hat{Cov}\left(\begin{array}{cccc} \hat{\alpha} \\\hat{\beta} \\ \end{array}\right)=(XX^T)^{-1}X\begin{bmatrix}\hat{\sigma_1}^2&0&0\\0&..&0\\0&0&\hat{\sigma_n}^2 \end{bmatrix}X^T(XX^T)^{-1}=\begin{bmatrix}\frac{1}{n_0}&-\frac{1}{n_0}\\-\frac{1}{n_0}&\frac{n}{n_0n_1}\\\end{bmatrix}\begin{bmatrix}1&..&1\\Z_1&..&Z_n\end{bmatrix}\begin{bmatrix}\hat{\epsilon_1}^2&0&0\\0&..&0\\0&0&\hat{\epsilon_n}^2 \end{bmatrix}\begin{bmatrix}1&..&1\\Z_1&..&Z_n\end{bmatrix}^T\begin{bmatrix}\frac{1}{n_0}&-\frac{1}{n_0}\\-\frac{1}{n_0}&\frac{n}{n_0n_1}\\\end{bmatrix}
$$

$$
\hat{V}_{EHW}=\frac{\hat{S^2}(1)}{n_1}\frac{n_1-1}{n_1}+\frac{\hat{S^2}(0)}{n_0}\frac{n_0-1}{n_0}
\ \ \ (4.5)$$

#### 4.6 Vector version of Neyman

如果实验个体的潜在结果是向量$\mathbf{V}_i=[V_i^{(1)},V_i^{(2)},...V_i^{(k)}]^T\in\mathbb{R}^K,\forall i$

$$
E(\hat{\tau}_{\mathbf{V}})=E(\frac{1}{n_1}\sum_{i=1}^nZ_i\mathbf{V}_i-\frac{1}{n_0}\sum_{i=1}^n(1-Z_i)\mathbf{V}_i)=\frac{1}{n_1}\sum_{i=1}^nEZ_i\mathbf{V}_i(1)-\frac{1}{n_0}\sum_{i=1}^n(1-EZ_i)\mathbf{V}_i(0)=\frac{1}{n}\sum_{i=1}^n\{\mathbf{V}_i(1)-\mathbf{V}_i(0)\}=\tau_{\mathbf{V}}
$$

$$
C=E[(\hat{\tau}_\mathbf{V}-\tau_{\mathbf{V}})(\hat{\tau}_\mathbf{V}-\tau_{\mathbf{V}})^T]=\frac{1}{n^2}E[(n_0\overline{\mathbf{V}}_i(1)-n_1\overline{\mathbf{V}}_i(0))(n_0\overline{\mathbf{V}}_i(1)-n_1\overline{\mathbf{V}}_i(0))^T]
$$

$$
c_{kj}=\frac{1}{n^2}E[(n_0\overline{V}^{(k)}_i(1)-n_1\overline{V}^{(k)}_i(0))(n_0\overline{V}^{(j)}_i(1)-n_1\overline{V}^{(j)}_i(0))]
$$

$$
从协方差矩阵的角度考虑:Cov(\hat{\tau}_{\mathbf{V}})=Cov(\frac{1}{n_1}\sum_{i=1}^n\mathbf{V}_i(1)-\frac{1}{n_0}\sum_{i=1}^n\mathbf{V}_i(0))
$$

#### 5.4 Compare CRE and SRE

$$
\sum_{k=1}^K\left[\frac{\pi_{[k]}}{n_1}(\overline{Y}_{[k]}(1)-\overline{Y}(1))^2+\frac{\pi_{[k]}}{n_0}(\overline{Y}_{[k]}(0)-\overline{Y}(0))^2-\frac{\pi_{[k]}}{n}(\tau_{[k]}-\tau)^2\right]=\sum_{k=1}^K\left[\frac{\pi_{[k]}}{n_1}A^2+\frac{\pi_{[k]}}{n_0}B^2-\frac{\pi_{[k]}}{n}(A-B)^2\right]
$$

$$
=\sum_{k=1}^K\frac{\pi_{[k]}}{n}\left\{(\frac{n}{n_1}-1)A^2+(\frac{n}{n_0}-1)B^2+2AB\right\}=\sum_{k=1}^K\frac{\pi_{[k]}}{n}\left\{\sqrt{\frac{n_0}{n_1}}A+\sqrt{\frac{n_1}{n_0}}B\right\}^2
$$

$$
=\sum_{k=1}^K\frac{\pi_{[k]}}{n}\left\{\sqrt{\frac{n_0}{n_1}}\left[\overline{Y}_{[k]}(1)-\overline{Y}(1)\right]+\sqrt{\frac{n_1}{n_0}}\left[\overline{Y}_{[k]}(0)-\overline{Y}(0)\right]\right\}^2\geq0\ \ \ \ (5.4)
$$

#### 5.9 From CRE to SRE

```{r}
library(Matching)
data(lalonde)
z=lalonde$treat#treatment
y=lalonde$re78#outcome
```

Fisherian inference基于强零假设$H_{0F}$构造CRE下的检验，得到检验p值

Neymanian inference在有限总体的CRE假设下，构造平均因果作用的无偏点估计，并获得置信区间

```{r}
#Fisehrian inference
fisher.pv = c(
  tauhat.p.value =t.test(y[z == 1],y[z == 0],var.equal=TRUE)$p.value,
  student.p.value = t.test(y[z == 1],y[z == 0],var.equal=FALSE)$p.value,
  W.p.value = wilcox.test(y[z == 1],y[z == 0])$p.value,
  D.p.value = ks.test(y[z == 1],y[z == 0])$p.value
)
#查看单边检验p值
round(0.5*fisher.pv,3)

#Neymanian inferences
olsfit = lm(y~z)
neyman.pv = c(neyman.p.value = summary(olsfit)$coef[2,4])
neyman.pv
```

```{r}
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
    varK [ k ] = var(yk[zk==1])/sum(zk) + var(yk[zk==0])/sum(1-zk)
    }
  return ( c(
    hat_est = sum(PiK*TauK),#点估计
    hat_se = sqrt(sum(PiK^2*varK)),#方差估计量
    pv = 1-pnorm(sum(PiK*TauK)/sqrt(sum(PiK^2*varK)))#标准正态检验p值
    ) )
}
```

```{r}
z=lalonde$treat#treatment
y=lalonde$re78#outcome

#stratified on the race(black, Hispanic, or other).
lalonde$race=ifelse(lalonde$black==1,1,0)+ifelse(lalonde$hisp==1,2,0)
print("stratified on the race(black, Hispanic, or other)")
Neyman_SRE(z,scale(y),lalonde$race)

#stratified on marital status
print("stratified on marital status")
Neyman_SRE(z,scale(y),lalonde$married)

#stratified on the indicator of a high school diploma
print("stratified on the indicator of a high school diploma")
Neyman_SRE(z,scale(y),lalonde$nodegr)
```

#### 6.7 Proposition 6.2

注意在回归之前需要对outcome结果变量进行标准化

$$
\hat{\tau}(\hat{\beta_0},\hat{\beta_1})=\hat{\gamma_1}-\hat{\gamma_0}=\left[\hat{\overline{Y}}(1)-\hat{\beta_1}\hat{\overline{X}}(1)\right]-\left[\hat{\overline{Y}}(0)-\hat{\beta_0}\hat{\overline{X}}(0)\right]=\frac{1}{n_1}\sum_{i=1}^nZ_i(Y_i-\hat{\beta_1}^TX_i)-\frac{1}{n_0}\sum_{i=1}^n(1-Z_i)(Y_i-\hat{\beta_0}^TX_i)
$$

考虑$Y_i$对$(1,X_i,Z_i,Z_i\times X_i)$的OLS fit

$$
Y_i=\hat{\alpha_0}+\hat{\alpha_1}X_i+\hat{\alpha_2}Z_i+\hat{\alpha_3}(Z_i\times X_i)
$$

对所有$Z_i=1$的情况$min\sum_{i=1}^n[Y_i(1)-\alpha_0-\alpha_2-(\alpha_1+\alpha_3)X_i(1)]^2$

对所有$Z_i=0$的情况$min\sum_{i=1}^n[Y_i(0)-\alpha_0-\alpha_1X_i]^2$

所以$\hat{\beta_1}=\hat{\alpha_1}+\hat{\alpha_3},\hat{\beta_0}=\hat{\alpha_1}$，又因为基于OLS的模型，我们有

$$
\begin{cases}
\hat{\overline{Y}}(1)=\hat{\alpha_0}+(\hat{\alpha_1}+\hat{\alpha_3})\hat{\overline{X}}(1)+\hat{\alpha_2} 
\\\hat{\overline{Y}}(0)=\hat{\alpha_0}+\hat{\alpha_1}\hat{\overline{X}}(0), 
\end{cases}
$$

$$
\Rightarrow \hat{\tau}_L=\hat{\alpha_2}=\hat{\tau}(\hat{\beta_0},\hat{\beta_1})
$$

#### 6.8 Prove

$$
\hat{\tau}_{pred}=\frac{1}{n}\left\{\sum_{Z_i=1}Y_i+\sum_{Z_i=0}\hat{\mu}_1(X_i)-\sum_{Z_i=1}\hat{\mu}_0(X_i)-\sum_{Z_i=0}Y_i\right\}==\frac{1}{n}\left\{\sum Z_i(Y_i-\hat{\beta_1}^TX_i)-\sum(1-Z_i)(Y_i-\hat{\beta_1}^TX_i)+n_0\hat{\gamma_1}-n_1\hat{\gamma_0}\right\}
$$

$$
=\frac{1}{n}\left\{\sum Z_i\hat{\gamma_1}-\sum(1-Z_i)\hat{\gamma_0}+n_0\hat{\gamma_1}-n_1\hat{\gamma_0}\right\}=\frac{1}{n}[n_1\hat{\gamma_1}-n_0\hat{\gamma_0}+n_0\hat{\gamma_1}-n_1\hat{\gamma_0}]=\hat{\gamma_1}-\hat{\gamma_0}=\hat{\tau}(\hat{\beta_0},\hat{\beta_1})=\hat{\tau}_L\ \ \ (6.8)
$$

$$
由于\sum_{Z_i=1}\hat{\mu_1}(X_i)=\sum_{Z_i=1}(\hat{\gamma_1}+\hat{\beta_1}^TX_i)=n_1\hat{\gamma_1}+\hat{\beta_1}^T\sum_{Z_i=1}X_i=\sum_{Z_i=1}Y_i\ ,同理\sum_{Z_i=0}\hat{\mu_0}(X_i)=\sum_{Z_i=0}Y_i
$$

$$
\hat{\tau}_{proj}=\frac{1}{n}\sum_{i=1}^n[\hat{\mu_1}(X_i)-\hat{\mu_0}(X_i)]=\frac{1}{n}[\sum_{Z_i=1}\hat{\mu_1}(X_i)+\sum_{Z_i=0}\hat{\mu_1}(X_i)-\sum_{Z_i=1}\hat{\mu_0}(X_i)-\sum_{Z_i=0}\hat{\mu_0}(X_i)]
$$

$$
=\frac{1}{n}\left\{\sum_{Z_i=1}Y_i+\sum_{Z_i=0}\hat{\mu}_1(X_i)-\sum_{Z_i=1}\hat{\mu}_0(X_i)-\sum_{Z_i=0}Y_i\right\}=\hat{\tau}_{pred}=\hat{\tau}_L\ \ \ (6.10)
$$

#### Estimating ACE in randomized experiments with GLM

```{r}
set.seed(100)
n=10000;p=8#八个变量
X = matrix(rnorm(n*p), n, p)
Z = rbinom(n,1,0.5)#treatment完全随机化
gamma1= rep(c(0.5,-0.5),4)
gamma0= rep(c(0.5,-0.5),4)
#这说明outcome与covariate之间的生成关系是Logistic模型
p.Y1 = 1/(1 + exp(-1- as.vector(X%*%gamma1) ))
p.Y0 = 1/(1 + exp(-as.vector(X%*%gamma0)))
Y1 = rbinom(n, 1, p.Y1)
Y0 = rbinom(n, 1, p.Y0)

Y=Z*Y1+(1-Z)*Y0#实际中无法看到潜在结果
dt = as.data.frame(cbind(X,Z,Y))
rm( list=c('gamma1','gamma0','Y1','Y0','p.Y1','p.Y0'))
```

##### (a)Use linear regression to do the 9 regressions and report the coefficient of Z. Plot a figure with x-axis to be the number of covariates included and y-axis to be the estimates.

```{r}
models <- list()
models[[1]] <- lm(Y~Z,data=dt)
for (i in 1:8) {
  formula_str <- paste("Y ~", paste(c("Z", paste0("V", 1:i)), collapse = " + "))
  formula <- as.formula(formula_str)
  model <- lm(formula, data = dt)
  models[[i+1]] <- model
}
y<-c()
for (i in 1:9) {
  y=c(y, summary(models[[i]])$coef[2,1])
}
plot(1:9,y,type="o-",pch = 19,col="red",main="linear model coef with coveriates number",ylim = c(0,1),
     xlab="the number of covariates included",ylab ="the coefficient of Z")
text(1:9, y, labels=round(y,3), pos=3, cex=0.8)
```

##### (b)Use logistic regression to do the same analysis. If you are interested, you could also use other link functions such as probit, tobit, complementary log-log.

```{r}
models <- list()
models[[1]] <- glm(Y~Z,data=dt,family = binomial())
for (i in 1:8) {
  formula_str <- paste("Y ~", paste(c("Z", paste0("V", 1:i)), collapse = " + "))
  formula <- as.formula(formula_str)
  model <- glm(formula, data = dt,family = binomial())
  models[[i+1]] <- model
}
y<-c()
for (i in 1:9) {
  y=c(y, summary(models[[i]])$coef[2,1])
}
plot(1:9,y,type="o-",pch = 19,col="red",main="logistic model coef with coveriates number",ylim = c(0,1),
     xlab="the number of covariates included",ylab ="the coefficient of Z")
text(1:9, y, labels=round(y,3), pos=3, cex=0.8)
```

##### (c)Discuss the findings from your analysis

在(a)中，我们用最小二乘法对9个linear model得到了Z的回归系数，可以发现系数均处于0.16-0.165的范围之间，这与平均因果作用的估计不应产生较大变化相符（尽管有更多的协变量介入，但仍然是相合估计）。然而在(b)中，改用9个logistic model得到的Z的系数，可以发现系数与方程变量个数有明显的正相关。