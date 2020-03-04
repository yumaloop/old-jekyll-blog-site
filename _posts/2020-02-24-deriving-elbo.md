---
layout: post
title: Deriving ELBO
lang: en
categories:
    - StatML
tags:
    - hoge
    - foo
---



## ELBO

aafe





$$
\begin{align}
(\text{Evidence})
&= \log p(x) \\
&= \log \int_{Z} p(x,z) \\
&= \log \int_{Z} p(x,z) \frac{q(z)}{q(z)} \\
&= \log \int_{Z} q(z) \frac{p(x,z)}{q(z)} \\
&= \log \mathbb{E}_{z \sim q} \left[ \frac{p(x,z)}{q(z)} \right] \\
&\geq \mathbb{E}_{z \sim q} \left[ \log \frac{p(x,z)}{q(z)} \right] \\
&= \mathbb{E}_{z \sim q} \left[ \log p(x,z) \right] + H_q(Z) \\
&= (\text{Evidence Lower Bound, ELBO})
\end{align}
$$



## KL-divergence

aaa
$$
\begin{align}
D_{KL}( q(z) \vert\vert p(z|x) ) 
&= \int_{Z} q(z) \frac{q(z)}{p(z|x)} \\
&= - H_q(Z) - \mathbb{E}_{z \sim q} \left[ \log p(z|x) \right]
\end{align}
$$


## KL-divergence and ELBO

aaa
$$
\begin{align}
ELBO 
&= \mathbb{E}_{z \sim q} \left[ \log p(x,z) \right] + H_q(Z) \\
&= \mathbb{E}_{z \sim q} \left[ \log p(x) + \log p(z|x) \right] + H_q(Z) \\
&= \mathbb{E}_{z \sim q} \left[ \log p(x) \right] + \mathbb{E}_{z \sim q} \left[ \log p(z|x) \right] + H_q(Z) \\
&= \log p(x) + H_q(Z) + \mathbb{E}_{z \sim q} \left[ \log p(z|x) \right] \\
&= \log p(x) - D_{KL}( q(z) \vert\vert p(z|x) ) 
\end{align}
$$


