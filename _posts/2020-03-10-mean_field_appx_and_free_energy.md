---
layout: post
title: 平均場近似と自由エネルギー
lang: ja
categories:
    - StatML
tags:
    - Bayes

---







ある変数$$X$$のとりうるすべての状態(実現値)$$x$$に対して，何らかのエネルギー関数$$\phi(x)$$が与えられたとする．このとき，変数$$X$$のGibbs分布（Boltzmann分布）:
$$
\begin{aligned}
p(x) 
&= \frac{\exp (- \beta \phi(x))}{\int_X \exp (- \beta \phi(x))} 
= \frac{\exp (- \beta \phi(x))}{Z^{\phi}(\beta)}
\end{aligned}
$$

を考える．このとき，Gibbs分布$$p(x)$$と任意の近似分布$$q(x)$$とのKL-divergence:
$$
\begin{aligned}
D_{KL}(p \vert\vert q) := \int_{X} q(x) \log \frac{q(x)}{p(x)} 
\end{aligned}
$$
は以下のように分解できる．
$$
\begin{aligned}
D_{KL}(p \vert\vert q) 
&= \int_X q(x)\phi(x) - \left\{ - \int_X q(x)\log q(x) \right\} + \log \int_X \exp(-\beta \phi(x)) \\
&= \mathbb{E}_{x \sim q}[\phi(x)] - H_q(X) + \log Z^{\phi}(\beta) \\
&= (\text{Internal energy}) - (\text{Entropy}) + (\text{Const.}) \\
\end{aligned}
$$

いま，近似分布$$q(x)$$に対する汎関数として，自由エネルギー:

$$
F^{\phi}(q) := \mathbb{E}_{x \sim q}[\phi(x)] - H_q(X) ~~~ (\text{Free energy})
$$

を定義すれば，$$q(x)$$による$$p(x)$$の近似問題は，次式で表現できる．

$$
\begin{aligned}
\underset{q}{\rm min} ~ D_{KL}(p \vert\vert q) &= 
\underset{q}{\rm min} ~ F^{\phi}(q)
\end{aligned}
$$

また．自由エネルギー$$F^{\phi}(q)$$の最小値は，

$$
{F^{\phi}}^{*}(q) = - \log \int_X \exp (-\beta \phi(x)) = - \log Z^{\phi}(\beta)
$$

となる．すなわち，

$$
\begin{aligned}
{F^{\phi}}(q) 
= - \log Z^{\phi}(\beta) 
~~ \Leftrightarrow ~~ 
D_{KL}(p \vert\vert q) = 0
~~ \Leftrightarrow ~~ 
p(\cdot) \equiv	 q(\cdot)
\end{aligned}
$$

となる．