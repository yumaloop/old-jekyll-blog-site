---
layout: post
title: Kullback-Leibler Divergence
categories:
    - StatML
tags:
    - foo
---



KL-divergence frequently appears in many fields such as statistics and information theory. It is defined as the **expected value** of **logarithmic** transformation of **likelihood ratio**. Note that:

1. expected value: weighted integration with probability density.
2. logarithmic transformation: conversion multiplication to linear combination that is suitable for convex optimization and function analysis.
3. likelihood ratio: a measure of likelihood comparison



<br>



## 1. What is KL-divergence?

#### 1.1 Definition

　For any probability distributions $$P$$ and $$Q$$, **KL-divergence** (Kullback-Leibler divergence)[^1] is defined as follows, using their probability density function $$p(x)$$ and $$q(x)$$. 
$$
D_{KL}( Q \mid\mid P ) := \int q(x) \log \frac{q(x)}{p(x)} ~dx  \tag{1}
$$



<br>



#### 1.2 Basic properties

　KL-divergence has the following properties.

* （**non-negative**）It has a non-negative range.

$$
0 \leq D_{KL}( Q \mid\mid P ) \leq \infty  \tag{2.1}
$$

* （***completeness***）When it equals to $$0$$, $$P$$ and $$Q$$ are equivalent.

$$
D_{KL}( Q \mid\mid P ) = 0 ~~ \Leftrightarrow ~~ P = Q   \tag{2.2}
$$

* （**assymmetry**）It is not symmetric about $$P$$ and $$Q$$.

$$
D_{KL}( Q \mid\mid P ) \neq D_{KL}( P \mid\mid Q )   \tag{2.3}
$$

* （**absolute continuity**）Unless it diverges, $$Q$$ is absolutely continuous with respect to $$P$$.

$$
D_{KL}( Q \mid\mid P ) \lt \infty ~~ \Rightarrow ~~ P \gg Q   \tag{2.4}
$$



<br>

​	For example, calculating KL-divergence[^2] between two Gaussian distributions gives the following results: It can be seen that the more the shapes between two distributions do not match, the more  KL-divergence increases.

{{% img src="images/dkl_norm.png" width="600" %}}

<br>

#### 1.3 Is KL-divergence a metrics?

​	KL-divergence is so important measurement when considering probability and information that it is called by various names depending on the field and context.

> "KL-divergence"
> "KL-metrics"
> "KL-information"
> "Information divergence"
> "Information gain"
> "Relative entropy"

Since KL-divergence is always non-negative, it might be interpreted as the metrics in the space where the probability distributions $$P$$ and $$Q$$ exist. However, KL-divergence is **not** strictly a metric because it only satisfies "non-negativity" and "completeness" among the following **axioms of metrics**. 



> Axioms of metrics $$d(~)$$:
> 
> 1. non-negativity                     $$d(x, ~ y) \geq 0$$
> 
> 1. completeness                      $$d(x, ~ y) = 0 ~~ \Leftrightarrow ~~ x = y$$
> 
> 1. symmetry                             $$d(x, ~ y) = d(y, ~ x)$$
> 
> 1. The triangle inequality       $$d(x, ~ y) + d(y, ~ z) \geq d(x, ~ z)$$

Note that $$d()$$ is called the *distance function* or simply *distance*



For example, Euclidean distance, squared distance, Mahalanobis distance, and Hamming distance satisfy these conditions, and can be clearly considered as metrics. On the other hand, KL-divergence is a divergence, not metrics. In mathematics, **"divergence"** is an extended concept of "metrics" that satisfies only **non-negativity** and **completeness** among axioms of metrics. By introducing "divergence", you can reduce the constraints of axioms of metrics and  have a high level of abstraction.

The word "divergence" is generally interpreted as the process or state of diverging; for example, in physics it appears as a vector operator **div**. There is no Japanese words that corresponds to the meaning of divergence, but it seems that the degree of difference, "相違度", "分離度", "逸脱度", "乖離度" etc. might be used.

As an example, let's measure the KL-divergence between two Gaussian distributions $$ N (0, 1) $$ (blue) and $$ N (1, 2) $$ (red). In the figure, the left shows **KL-divergence from red one as seen from blue one**, and the right shows **KL-divergence from blue one as seen from red one**. Their value are surely different. 

Note that given two Gaussian distribution $$p_1,p_2$$ as 

$$
\small
\begin{align}
p_1(x) &= \mathcal{N}(\mu_1, \sigma_1^2) = \frac{1}{\sqrt{2 \pi \sigma_1^2}} \exp \left\{ - \frac{ {(x - \mu_1)}^2}{2 \sigma_1^2} \right\} \\
p_2(x) &= \mathcal{N}(\mu_2, \sigma_2^2) = \frac{1}{\sqrt{2 \pi \sigma_2^2}} \exp \left\{ - \frac{ {(x - \mu_2)}^2}{2 \sigma_2^2} \right\}
\end{align}
$$

the following holds.

$$
\small
\begin{align}
{D}_{KL}(p_1 \mid\mid p_2) &= \int_{-\infty}^{\infty} p_1(x) \log \frac{p_1(x)}{p_2(x)} dx \\
&= \log \frac{\sigma_2}{\sigma_1} + \frac{\sigma_1^2 + {( \mu_1 - \mu_2 )}^2}{2 \sigma_2^2} - \frac{1}{2}
\end{align}
$$

.

img src="images/comparison_of_dkl_norm.png" width="600"



Incidentally, in addition to the KL-divergence, the following is known as a measure of the proximity (or closeness) between two probability distributions.



> **The metrics to measure closeness between $$q(x)$$ and $$p(x)$$**
>
> * $$ {\chi}^2(q ; p) := \sum_{i=1}^{k} \frac{ { \{ p_i - q_i \} }^{2} }{p_i}$$    *$$ {\chi}^2$$ statistics*
> * $$ L_1(q ; p) := \int \vert q(x) - p(x) \vert ~ dx$$    *$$L_1$$-norm*
> * $$ L_2(q ; p) := \int { \{ q(x) - p(x) \} }^{2} ~ dx$$    *$$L_2$$-norm*
> * $$ I_K(q ; p) := \int { \\{ \sqrt{ q(x) } - \sqrt{ p(x) } \\} }^{2} ~ dx $$    *Herringer distance*
> * $$ \mathbb{D}(q ; p) := \int f \left( {\large \frac{q(x)}{p(x)} } \right) q(x) ~ dx$$    *$$f$$-divergence*
> * $$ I_{\lambda}(q ; p) := \int \left\{ { \left( {\large \frac{q(x)}{p(x)} } \right) }^{\lambda} - 1 \right\} q(x) ~ dx$$    *Generalized information (Kawada, 1987)*
> * $$ {D}_{KL}(q ; p) := \int \log \left( {\large \frac{q(x)}{p(x)} } \right) q(x) ~ dx$$    *KL-divergence*
> * $$ JSD(q \mid\mid p) := \frac{1}{2} {D}_{KL}(q \mid\mid \frac{q+p}{2}) + \frac{1}{2} {D}_{KL}(p \mid\mid \frac{q+p}{2})$$    *JS-divergence*



<br>



## 2. Relatinoship to other measurements

#### 2.1 KL-divergence vs Mutual information

　In information theory, entropy $$H(X)$$, join entropy $$H(X,Y)$$, conditional entropy $$H(X \vert Y)$$, mutual information $$MI(X,Y)$$ are defined as follows by using probability density $$Pr()$$[^3].

$$
\begin{align}
  H(X)    &:= - \int Pr(x) \log Pr(x) ~dx \tag{3.1} \\
  H(X,Y)  &:= - \int Pr(x,y) \log Pr(x,y) ~dy~dx \tag{3.2} \\
  H(X|Y)  &:= - \int Pr(x,y) \log Pr(x|y) ~dx~dy \tag{3.3} \\
  MI(X,Y) &:= \int \int Pr(x,y) \log \frac{Pr(x,y)}{Pr(x)Pr(y)} ~dxdy \tag{3.4}
\end{align}
$$



For any two random variable $$X$$ and $$Y$$, mutual information $$MI(X, Y)$$ specifies the mutual (symmetric) dependence between them.



  {{% img src="images/dkl_and_mutual_information.png" width="400" %}}



$$
\begin{align}
  MI(X,Y) &= H(X) - H(X|Y) \tag{3.5.1} \\
                &= H(Y) - H(Y|X) \tag{3.5.2} \\
                &= H(X) + H(Y) - H(X,Y) \tag{3.5.3} 
\end{align}
$$

　Here, the following relationship holds between KL-divergence and mutual information.

$$
\begin{align}
  MI(X, Y) &= D_{KL} \bigl( Pr(x, y) \mid\mid Pr(x)Pr(y) \bigr)  \tag{3.6.1} \\
                 &= \mathbb{E}_{Y} \bigl[ D_{KL} \bigl( Pr(x|y) \mid\mid Pr(x) \bigr) \bigr]  \tag{3.6.2} \\
                 &= \mathbb{E}_{X} \bigl[ D_{KL} \bigl( Pr(y|x) \mid\mid Pr(y) \bigr) \bigr]  \tag{3.6.3} \\
\end{align}
$$

So that, mutual information $$MI (X, Y)$$ is interpreted as the degree of difference (average degree of deviation) between the joint distribution $$Pr (x, y)$$ when the $$X$$ and $$Y$$ are **not independent** and the joint distribution $$Pr (x) Pr (y)$$ when $$ X$$ and $$Y$$ are **independent**. 

（cf.）Formula transformation of 3.6.2

$$
\begin{eqnarray}
MI(X,Y) & = & \int \int Pr(x,y) \log \frac{Pr(x,y)}{Pr(x)Pr(y)} ~dxdy \\
& = & \int \int Pr(x|y)Pr(y) \log \frac{Pr(x|y)Pr(y)}{Pr(x)Pr(y)} ~dxdy \\
& = & \int Pr(y) \int Pr(x|y) \log \frac{Pr(x|y)}{Pr(x)} ~dx~dy \\
& = & \int Pr(y) \cdot D_{KL} \bigl( Pr(x|y) \mid\mid Pr(x) \bigr) ~dy \\
& = & \mathbb{E}_{Y} \bigl[ D_{KL} \bigl( Pr(x|y) \mid\mid Pr(x) \bigr) \bigr] 
\end{eqnarray}
$$

<br>

### 2.2 KL-divergence vs Log likelihood ratio

​	In the field of Bayes inference and statistical modeling, you often face the problem of estimating the **true distribution** $$q(x)$$ by $$p_{\hat{\theta}}(x)$$ (that is the combination of stochastic model $$p_{\theta}(x)$$ and estimated parameter $$\hat{\theta}$$ ) . Therefore, KL-divergence is used when you want to measure the difference between two distributions, or when you want to incorporate the estimation error into the loss function or risk function in order to solve  the optimization problem for the parameter $$\theta$$ . 

​	Also, KL-divergence is related to the log likelihood ratio so much that it has a deep connection to the model selection method [^4] such as likelihood ratio test, Bayes factor, and AIC (Akaike's information criterion).

* 　KL-divergence of estimated distribution $$p_{\theta}(x)$$ for the true distribution $$q(x)$$ : $$D_{KL}(q \mid\mid p_{\theta})$$ is considerd as the expected value of the log likelihood ratio $$q(x)/p_{\theta}(x)$$ for tue true distribution $$q(x)$$.

$$
\left( \text{Log likelihood ratio} \right) = \log \frac{q(x)}{p_{\theta}(x)}   \tag{4}
$$

$$
\begin{eqnarray}
D_{KL}( q \mid\mid p_{\theta} ) & := & \int q(x) \log \frac{q(x)}{p_{\theta}(x)} ~dx \\\
                                         & = & \mathbb{E}_{X} \left[ \log \frac{q(x)}{p_{\theta}(x)} \right] \left(\text{Expected log likelihood ratio} \right)   \tag{5.1} \\\
\end{eqnarray}
$$

​	When using KL-divergence as the evaluation/loss value in model selection/comparison, it is equivalent that minimizing KL-divergence: $$D_{KL}( q \mid\mid p )$$ and maximizing the log likelihood: $$\log p(x)$$ as follows.

$$
\begin{eqnarray}
D_{KL}( q \mid\mid p_{\theta} ) & = & \mathbb{E}_{X} \bigl[ \log q(x) \bigr] - \mathbb{E}_{X} \bigl[ \log p_{\theta}(x) \bigr] \tag{5.2} \\
                               & \propto & - \mathbb{E}_{X} \bigl[ \log p_{\theta}(x) \bigr] \left(-1 \cdot \text{ Expected log likelihood} \right)    \tag{5.3}
\end{eqnarray}
$$

<br>

* For any parametric stochastic model $$ f(x \vert \theta) $$ (such as a linear regression model) which represents the estimated distribution as
  $$
  \begin{align}
  	p_{\theta}(x) = f(x|\theta)
  \end{align}
  $$
  , if a certain loss function $$L(\theta)$$ is given, the optimal parameter $$\theta^*$$ exists as it satisfy the following.
  $$
  \begin{align}
    q(x) &= f(x|\theta^*) \\
  \end{align}
  $$
  Then, for any estimated parameter $$\hat{\theta}$$ ,the estimated loss of the model $$f(x \vert \hat{\theta})$$ is represented by KL-divergence. (Note that $$ \ell( \cdot \vert x) $$ means the log likelihood function.)

$$
\left( \text{Log likelihood ratio} \right) = \log \frac{f(x|\theta^{*})}{f(x|\hat{\theta})}   \tag{6}
$$

$$
\begin{eqnarray}
 \hat{\theta} 
 & :=& \underset{\theta \in \Theta}{\rm argmin} ~ L(\theta) \tag{7} 
 \\
 D_{KL}( q \mid\mid p_{\hat{\theta}} ) 
 & = & D_{KL}( p_{\theta^{*}} \mid\mid p_{\hat{\theta}} ) \\
 & = & D_{KL}( f_{\theta^{*}} \mid\mid f_{\hat{\theta}} ) \\
 & = & \int f(x|\theta^{*}) \log \frac{f(x|\theta^{*})}{ f(x|\hat{\theta})} dx \\
 & = & \mathbb{E}_{X} \left[ \log \frac{ f(x|\theta^{*}) }{ f(x|\hat{\theta}) } \right] \tag{8.1} \\
 & = & \mathbb{E}_{X} \bigl[ \ell( \theta_{0}|x ) \bigr] - \mathbb{E}_{X} \bigl[ \ell( \hat{\theta} | x ) \bigr] \tag{8.2}
\end{eqnarray}
$$

<br>

#### 2.3 KL-divergence vs Fisher information

Given a certain stochastic model $$f(\cdot \vert \theta)$$, **Fisher information** $$I(\theta)$$ for the parameter $$\theta$$ is defined as follows.  (Note that $$ \ell( \cdot \vert x) $$ means the log likelihood function.)

$$
\begin{align}
  I(\theta) &:= \mathbb{E}_{X} \left[ { \left\{ \frac{d}{dx} \ell(\theta \vert x) \right\} }^{2} \right]  \tag{9.1} \\
            &= \mathbb{E}_{X} \left[ { \left\{ \frac{d}{dx} \log f(x|\theta) \right\} }^{2} \right]  \tag{9.2} 
\end{align}
$$

Also, between KL-divergence and Fisher information, the following holds.
　
$$
\lim_{h \to 0} \frac{1}{h^{2}} D_{KL} \bigl( f(x|\theta) \mid\mid f(x|\theta+h) \bigr) = \frac{1}{2} I(\theta)   \tag{10}
$$

(cf.) The following equation holds by using Taylor expansion of $$ \ell( \cdot \vert x) $$.

$$
\ell(\theta + h) - \ell(\theta) = {\ell}^{'}(\theta)h + \frac{1}{2} {\ell}^{''}(\theta) h^{2} + O(h^{3})
$$

This formula indicates that in parameter space $$ \Theta $$, for all point $$ \theta \in \Theta $$ ant its neighborring point $$ \theta + h $$, their KL-divergence：$$ D_{KL} ( f(x \vert \theta) \mid\mid f(x \vert \theta+h) ) $$ is **directly proportional to** Fisher information $$ I(\theta)$$. After all, Fisher information $$ I(\theta)$$ measures **the local information** that the stochastic model $$ f(\cdot \vert \theta) $$ has at the point $$ \theta$$.


{{% img src="images/dkl_and_fischer_information.png" width="600" %}}



<br>



## 3. References

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=tf_til&t=yumaloop0f-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4254127820&linkId=1456f8ade37cd01c91d31448ce7b50f2&bc1=ffffff&lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr">
</iframe>

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=tf_til&t=yumaloop0f-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=4785314117&linkId=a437161b2bfff7107300d73243499d9d&bc1=FFFFFF&lt1=_top&fc1=333333&lc1=0066C0&bg1=FFFFFF&f=ifr">
</iframe>

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="https://rcm-fe.amazon-adsystem.com/e/cm?ref=tf_til&t=yumaloop0f-22&m=amazon&o=9&p=8&l=as1&IS1=1&detail=1&asins=0471241954&linkId=477c693b4215ab3b8aa2cdee1450fef7&bc1=ffffff&lt1=_top&fc1=333333&lc1=0066c0&bg1=ffffff&f=ifr">
</iframe>


<br><br><br>

[^1]: Also, f-divergence is defined as its generalized class.

[^2]: I used scipy.stats.entropy().

[^3]: Although thermodynamic entropy is originated in Boltzmann, the historical background of Shannon information is mentioned below link. There seems to be a reference flow: Hartley → Nyquist → Shannon. http://www.ieice.org/jpn/books/kaishikiji/200112/200112-9.html

[^4]: Article on gneralized information criterion(GIC): https://www.ism.ac.jp/editsec/toukei/pdf/47-2-375.pdf
