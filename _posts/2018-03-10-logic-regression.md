---
layout: post
title:  logic regression
date:   2017-07-05 20:36:28 +0800
categories: tech test
---




线性回归中$$ h_\theta(x) $$ 可能>1或者<0
而对于分类问题(classification)，其输出值y是0或1。
这就要使得  $$h_\theta(x) \in \{0,1\}$$

线性回归 Hypothesis:

$$ h_\theta(x) = \theta^T x $$

逻辑回归 Hypothesis:
$$h_\theta(x) = g(\theta^T x) $$
逻辑方程 Logistic function:

$$ g(z) = \frac{1}{1+e^{-z}} $$
如果z是个矩阵，则对每个元素求解对应值。


$$h_\theta(x) = g(\theta^T x) $$  输出一个{0,1}的值，比如0.7，这意味着该预测函数接受输入x，算出x有70%的概率使得y=1。即，
$$h_\theta(x) = P(y=1|x;\theta)$$

但是分类问题最重要有一个答案，y是0还是1。因此就要对$h_\theta(x)$进行判断。比如$h_\theta(x)>=0.5，则y=1$，反之y=0。

g(z)的函数图:
![]({{"assets/img/img1.jpg" | absolute_url}})
因此
z>=0 → g(z) >= 0.5
z<0 → g(z) < 0.5

$h_\theta(x) = g(\theta^T x) $
$\theta^T x >= 0 $ → $h_\theta(x) >=0.5 → y=1$
$\theta^T x < 0 $ → $h_\theta(x) <0.5 → y=0$

$\theta^T x = 0$ 得到的方程被称为decision boundary。对于二维而言，如果是一条直线(线性linear)，那么这条线把平面分割成2部分，y=1和y=0。
decision boundary跟数据集无关，数据集是用来寻找最合适的$\theta$，而$\theta$决定了decision boundary。
decision boundary也可能是非线性的。
比如 $h_\theta(x) = g(\theta_0 + \theta_1x_1 + \theta_2x_2 + \theta_3x_1^2 + \theta_4x_2^2)$
decision boundary: 是个圆的方程


## 寻找最优θ
cost方程定义:
$J(\theta) = (1/m)\Sigma_{i=1}^mCost(h_\theta(x^{(i)}),y^i))$

线性回归的平方差cost:
$Cost(h_\theta(x^{(i)}),y^i)) = \frac{1}{2}(h_\theta(x^{i}) - y^{i})^2$

定义convex函数: 如果J(θ)是弧形的，即只有一个全局的极值，那么使用梯度下降一定会找到最优点，那么称J(θ)是convex函数。如果有很多极值，梯度下降不一定会找到全局最优点，则成为non-convex函数。

如果采用线性回归的平方差cost函数，应用于逻辑回归，由于逻辑回归中$h_\theta(x) = g(\theta^T x) $，这么算出来的J(θ)是non-convex的。

逻辑回归cost函数:
$Cost(h_\theta(x),y)) = $
$-log(h_\theta(x))$, if y = 1 
$-log(1-h_\theta(x))$,  if y = 0 
写在一起:
$Cost(h_\theta(x),y)) = -y*log(h_\theta(x)) - (1-y)*log(1-h_\theta(x))$

y=1时，$h_\theta(x) = 1 → Cost(h_\theta(x),y)) = 0$，$h_\theta(x) = 0 → Cost(h_\theta(x),y)) = +\infty$
y=0时，$h_\theta(x) = 0 → Cost(h_\theta(x),y)) = 0$，$h_\theta(x) = 1 → Cost(h_\theta(x),y)) = +\infty$



### 梯度下降

$J(θ) = - \frac{1}{m}[\Sigma^m_{i=1}y^{(i)}*log(h_\theta(x^{(i)})) + (1-y^{(i)})*log(1-h_\theta(x^{(i)}))]$

要求使得J(θ)最小的θ值

$θ_j:=θ_j - α\frac{d}{d\theta_j}J(θ)$
导数展开:

$\frac{d}{d\theta_j}J(θ) = \frac{1}{m}\Sigma^m_{i=1}(h_\theta(x^{(i)})-y^{(i)})x_j^{(i)}$


### Octave 库算法 fminunc

设要求$θ = [θ_0,θ_1,...,θ_n]^T$

设定cost函数:

```
function [jval,gradient] = costFunction(theta)
	jval = //J(θ)的算法
	gradient = zeros(n,1);
	gradient(1) = //J(θ)对θ0的偏导数
	....
end
```
实际例子:

{% highlight matlab%}

%cost函数
function [J, grad] = costFunction(theta, X, y)
	J = (-1/m)*(log(sigmoid(theta'*X'))*y + log(1-sigmoid(theta'*X'))*(1-y));
	grad =(1/m)*((sigmoid(theta'*X')-y')*X)';
end

{% endhighlight %}



```matlab
%sigmoid函数
function g = sigmoid(z)
	g = (1+ e .^ (-z)).^(-1);
end
```

octave库函数调用:

```matlab
options = optimset('GradObj','on','MaxIter','100');
initialTheta = zeros(n,1);
[optTheta, functionVal, exitFlag] = fminunc(@costFunction, initialTheta, options);
```
调用fminunc函数，自动返回最优的theta值，保存在optTheta；最优的theta值产生的J(θ)，保存在functionVal。已经函数退出状态exitFlag。






## Multiclass Classification
如果分类的类别不止2种，有k类，就采用k个逻辑回归算法。
比如有3种类别，比如邮件归类: 一般邮件(y=1)，垃圾邮件(y=2)，重要邮件(y=3)。

那么对于每个类别构造逻辑方程:
$h_\theta(x) = g(\theta^T x) $
对于y=1，进行y=1和y≠1的分类，$h_\theta(x)$返回y=1的概率
对于y=2，进行y=2和y≠2的分类，$h_\theta(x)$返回y=2的概率
对于y=3，进行y=3和y≠3的分类，$h_\theta(x)$返回y=3的概率
比较3个概率，取最大的概率。


## Overfitting问题

![Screen Shot 2017-06-01 at 13.58.16](http://o8e9d1ezz.bkt.clouddn.com/Screen Shot 2017-06-01 at 13.58.16.png)


overfitting是指，预测函数$h_\theta(x)$ 用了很多高纬度的变量，使得结果很好的拟合了训练数据集，但是对于要预测的数据而言不一定准确。

underfitting是指，比如，想当然地以为拟合函数是一条直线，然而并不是。


### 解决方案: Regularization
设线性回归问题的预测函数:
$h_\theta(x) = \theta_0 + \theta_1x + \theta_2x^2 + \theta_3x^3 $
如果训练数据集，实际上是个线性函数，那么该预测函数就overfitting了。
也就是说$\theta_2x^2 + \theta_3x^3$实际上是没什么用的，要消除overfitting问题，就得削弱$x^2，x^3$的作用。

重写cost函数:
J(θ) = $\frac{1}{m} [ \Sigma_i^m(\frac{1}{2}(h_\theta(x^{i}) - y^{i})^2 + 1000\theta_2^2 + 1000\theta_3^2)]$
这样要求使得J(θ)最小化的θ，$θ_2,θ_3$必须趋近于0，否则J(θ)就会很大。$θ_2,θ_3$趋近于0，也就意味着预测函数$h_\theta(x) = \theta_0 + \theta_1x$是线性的。


一般化:
Regularized cost function:
J(θ) = $\frac{1}{m} [\Sigma_i^m(\frac{1}{2}(h_\theta(x^{i}) - y^{i})^2 + \lambda\Sigma_{j=1}^n\theta_j^2)]$
($θ_0$不削弱)
对于一组训练数据，我们一开始并不知道到底哪些θ是需要削弱的，因此对所有θ都进行处理。λ用来调节，如果λ太大，那么所有θ都会趋近于0，那么最终预测函数$h_\theta(x) = \theta_0$，会造成underfitting。

#### Regularized 

Regularized 逻辑回归Cost函数:  
J(θ) = $$ \frac{-1}{m} [\Sigma_i^m(y^{(i)}*log(h_\theta(x^{(i)})) + (1-y^{(i)})*log(1-h_\theta(x^{(i)})))] + \frac{\lambda}{2m}\Sigma_{j=1}^n\theta_j^2 $$

Regularized 梯度下降参数:
$\theta_0 := \theta_0 - \alpha{\frac{d}{d\theta_0}J(\theta_0}) = \theta_0 - \alpha(1/m)\Sigma^m_{i=1}(h_{\theta}(x^{i}) - y^i)x^i_0$

$\theta_j := \theta_j - \alpha{\frac{d}{d\theta_j}J(\theta_j}) = \theta_j - \alpha [\frac{1}{m}\Sigma^m_{i=1}(h_{\theta}(x^{i}) - y^i)x^i_j + \frac{λ}{m}\theta_j] = \theta_j(1-α\frac{λ}{m}) - \alpha \frac{1}{m}\Sigma^m_{i=1}(h_{\theta}(x^{i}) - y^i)x^i_j$
j = 1,2,3,...,n

其中$(1-α\frac{λ}{m})<1$，也就是比起原来的梯度下降方程，此时每次循环使得$\theta_j$缩小了一点。






octave函数:

```
function [J, grad] = costFunctionReg(theta, X, y, lambda)
	n = length(theta);
	
	J = (-1/m)*(log(sigmoid(theta'*X'))*y + log(1-sigmoid(theta'*X'))*(1-y)) + (lambda/2/m)*sum(theta(2:n,:) .^2);
	
	theta2 = theta .* (lambda/m);
	theta2(1) = 0;
	
	grad =(1/m)*((sigmoid(theta'*X')-y')*X)' + theta2;
end

```


#### Regularized normal equation

$$\theta = (X^TX + λ*K )^{-1}X^TY$$
其中 K 是 (n+1) * (n+1) 矩阵，对角线上除了第一行，其他都为1。  

$$  
	K =
	\begin{bmatrix}
	0 & 0 & 0 & ... & 0 \\
	0 & 1 & 0 & ... & 0 \\
	0 & 0 & 1 & ... & 0 \\
	... & ... & ... & ... & ... \\
	0 & 0 & 0 & 0 & 1 \\
	\end{bmatrix}
$$

不可逆问题，如果m<n，矩阵<span>$X^TX$</span>可能不可逆。
但是用Regularizion就不需要担心。
只要λ>0，矩阵$X^TX + λ*K $一定是可逆的。





