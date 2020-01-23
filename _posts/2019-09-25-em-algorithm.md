---
layout: post
title: EM Algorithm
categories:
    - StatML
tags:
    - hoge
    - foo
---

![EM algorithm header img](https://www.mathworks.com/matlabcentral/mlc-downloads/downloads/submissions/65772/versions/3/screenshot.png"EM algorithm")

​	EM algorithm is **an algorithm for deriving the maximum likelihood estimator (MLE)**, which is generally applied to statistical methods for incomplete data. Originally, the concept of **“incomplete data and complete data”** was established to handle missing data, but by extending the definition, it can be applied to cut data, censored data, mixed distribution models, Robust distribution models, and latent data. It can also be applied to variable models, and Bayesian modeling.

​	Also, a number of statistical approach for clustering and unsupervised learning (eg, k-means, Gaussian mixture models) can be **generalized** as EM algorithms when focusing on the computational process. In addition, researches on analyzing the EM algorithm from the viewpoint of **information geometry** has been active, and applying EM algorithm to the stochastic model including an exponential family can be summarized in the form of **e-projection / m-projection**.



<br>



## 1. Statistical inference

> Purpose: To find out the probability distribution $$q(x)$$ that a certain variable $$x \in X$$ follows.

Namely, when considering a stochastic model $$p(x \vert \theta)$$ determined by the parameter $$\theta \in \Theta$$ and detecting the optimal parameter $$\theta^{*} \in \Theta$$ from dataset $$ \mathcal{D} := {\{x_i\}}_{i=1}^{n}$$, the follwing Approximation holds.
$$
~ \\
\begin{align}
x \sim q(x) \approx p(x|\theta) \\
\end{align}
~ \\
$$
This is called a statistical inference (or statistical estimation).



<br>




## 2. Maximum likelihood estimation

The most basic algorithm for statistical inference is maximum likelihood estimation (MLE). A log likelihood function of the stochastic model $$p(x \vert \theta)$$ is defined as
$$
~ \\
\begin{align}
\ell(\theta | x) := \log p(x | \theta)
\end{align}
~ \\
$$
and an empirical objective function of $$\theta$$:
$$
~ \\
\begin{align}
J(\theta) := \frac{1}{n} \sum_{i=1}^{n} \ell(\theta | x_i)
\end{align}
~ \\
$$
that depends on dataset $$ \mathcal{D} := {\{x_i\}}_{i=1}^{n}$$ can be obtained, MLE of parameter $$\theta$$ is derived as follows.
$$
~ \\
\begin{align}
\hat{\theta}_{MLE} = \underset{\theta \in \Theta}{\rm argmax} ~ J(\theta)
\end{align}
~ \\
$$


<br>



## 3. EM algorithm

Let's define the following data categories.

- Complete data $$x\in X$$ : <br>
    - **not observable** but completely follows the true distribution $$p(x)$$ 
- Imcomplete data $$y \in Y$$ : <br>
    - observable but **not completely follows** the true distribution $$p(x)$$ 

In general, the relationship complete data $$x$$ and incomplete data $$y$$ is a one-to-many relationship. but here, as a convenient assumption, I introduce a latent variable $$z \in Z$$ to express this constraint, that is assume $$x = [y,z]$$ holds. Considering the stochastic model for the complete data $$x$$,
$$
~ \\
\begin{align}
p(x | \theta) = p(y,z | \theta)
\end{align}
~ \\
$$

data sample $$x_i$$ cannot be observed and its likelihood $$p(x_i \vert \theta)$$ cannot be calculated. However, for data sample $$y_i$$ can be observed and its likelihood $$p(y_i \vert \theta)$$ can be calculated.
$$
~ \\
\begin{align}
p(y_i | \theta) 
&= \int_{Z} p(x_i | \theta) ~ dz \\
&= \int_{Z} p(y_i, z | \theta) ~ dz
\end{align}
~ \\
$$
By using this formula, the estimated value of $$\hat{\theta}_{MLE}$$ can be obtained by approximating $$p(x \vert \theta)$$, the likelihood function of complete data $$x$$. The procedure to derive the estimated value of $$\hat{\theta}_{MLE}$$ is called EM algorithm because it is an iterative method that repeats E-step and M-step alternately.



<br>


> **EM algorithm**
> 
> 1. Initialize $$\theta$$ with $$\theta^{0}$$.
> 
> 2. For each step $$t$$: 
> 	- E step 
> 	
> 	  Update the expectation value $$Q$$.
> 	
> 	  $$ Q(\theta | \theta^{(t)}) = \mathbb{E}_{x \sim p(x|\theta)} \left[ \log p(x|\theta) | y, \theta^{(t)} \right] $$
> 	
> 	- M step 
> 	
> 	  Derive the optimal parameter \theta^{(t+1)} that maximize $$Q$$ value.
> 	
> 	  $$ \theta^{(t+1)} = \underset{\theta \in \Theta}{\rm argmax} ~ Q(\theta | \theta^{(t)}) $$
> 	
> 3. Consider the convergence value $$\theta^{(\infty)}$$ as the algorithm output. 



<br>

As a result, the estimated value of $$\hat{\theta}_{MLE}$$ is derived as $$\hat{\theta}_{EM}$$ and the following holds.
$$
~ \\
\begin{align}
\hat{\theta}_{MLE} \approx \hat{\theta}_{EM}, ~~~ 
p(x|\hat{\theta}_{MLE} ) \approx p(x|\hat{\theta}_{EM} )
\end{align}
~ \\
$$


Also, the summarized formula of calculations in E step and M step is as follows.

> **EM step**
> $$
> \begin{align}
> \theta^{(t+1)} 
> &= \underset{\theta \in \Theta}{\rm argmax} ~ \mathbb{E}_{x \sim p(x|\theta)} \left[ \log p(x | \theta) | y, \theta^{(t)} \right] \\
> &= \underset{\theta \in \Theta}{\rm argmax} ~ \mathbb{E}_{(y,z) \sim p(y,z|\theta)} \left[ \log p(y, z | \theta) | y, \theta^{(t)} \right] \\
> &= \underset{\theta \in \Theta}{\rm argmax} ~ \mathbb{E}_{z | {\theta}^{(t)} \sim \int_{Y} p(y,z | {\theta}^{(t)}) dy} \left[ \log p(y, z | \theta) \right]
> \end{align}
> $$

<br><br>

## References

- <a href="http://ebsa.ism.ac.jp/ebooks/sites/default/files/ebook/1881/pdf/vol3_ch9.pdf">9章 EMアルゴリズム - 「21 世紀の統計科学」第 III 巻 日本統計学会, 2008</a>
- <a href="https://staff.aist.go.jp/s.akaho/papers/josho-main.pdf">解説 EMアルゴリズムの幾何学 - 赤穂昭太郎, 電子技術総合研究所</a>
- <a href="https://www.ism.ac.jp/~shiro/papers/books/embook2000.pdf">EMアルゴリズムと神経回路網, 2000, 統計数理研究所</a>
