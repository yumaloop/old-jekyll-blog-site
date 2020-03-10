---
layout: post
title: 平均場近似と自由エネルギー
lang: ja
categories:
    - StatML
tags:
    - Bayes

---



KL-divergence:

$$
\begin{aligned}
D_{KL}(p \vert\vert q) := \int_{Z} q(z) \log \frac{q(z)}{p(z)} 
\end{aligned}
$$

に対して，エネルギー関数$$\phi(z)$$が与えられたときの$$Z$$のGibbs分布（Boltzmann分布）:

$$
\begin{aligned}
p(z) 
&= \frac{\exp (- \beta \phi(z))}{\int_Z \exp (- \beta \phi(z))} 
= \frac{\exp (- \beta \phi(z))}{Z^{\phi}(\beta)}
\end{aligned}
$$

を考える．このとき，

$$
\begin{aligned}
D_{KL}(p \vert\vert q) 
&= \int_Z q(z)\phi(z) - \left\{ - \int_Z q(z)\log q(z) \right\} + \log \int_Z \exp(-\beta \phi(z)) \\
&= \mathbb{E}_{z \sim q}[\phi(z)] - H_q(Z) + \log Z^{\phi}(\beta) \\
&= (\text{Internal energy}) - (\text{Entropy}) + (\text{Const.}) \\
\end{aligned}
$$

だから，自由エネルギー:

$$
F^{\phi}(q) := \mathbb{E}_{z \sim q}[\phi(z)] - H_q(Z) ~~~ (\text{Free energy})
$$

を定義すれば，任意の$$p(z) = \frac{\exp (- \beta \phi(z))}{\int_Z \exp (- \beta \phi(z))}$$に対して，

$$
\begin{aligned}
\underset{q}{\rm min} ~ D_{KL}(p \vert\vert q) = 
\underset{q}{\rm min} ~ F^{\phi}(q)
\end{aligned}
$$

が成り立つ．また．自由エネルギー$$F^{\phi}(q)$$の最小値は，

$$
{F^{\phi}}^{*}(q) = - \log \int_Z \exp (-\beta \phi(z)) = - \log Z^{\phi}(\beta)
$$

となる．