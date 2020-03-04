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



Evidence Lower Bound (ELBO) is widely used in variational inference. Recently, according to the massive success of DeepLearning and related models, variational inference (and its technic) gains exposure in the filed of representation learning. For instance, stochastic generative models such as VAE and GAN are famous for their variational aspects.



## ELBO

Evidence Lower Bound (ELBO) is a lower bound of Log likelihood of $$X$$ (Evidence) in the model. The below inequality holds based on Cauchy-Schwartz inequality because of the convexity of log function.


$$
\begin{aligned}
(\text{Evidence})
&= \log p(x) \\
&= \log \int_{Z} p(x,z) \\
&= \log \int_{Z} p(x,z) \frac{q(z)}{q(z)} \\
&= \log \int_{Z} q(z) \frac{p(x,z)}{q(z)} \\
&= \log \mathbb{E}_{z \sim q} \left[ \frac{p(x,z)}{q(z)} \right] \\
&\geq \mathbb{E}_{z \sim q} \left[ \log \frac{p(x,z)}{q(z)} \right] \\
&= \mathbb{E}_{z \sim q} \left[ \log p(x,z) \right] + H_q(Z) \\
&= ELBO(q) ~~~ (\text{Evidence Lower Bound, ELBO})
\end{aligned}
$$


So that, we can obtain the optimization formula below.


$$
\begin{aligned}
\underset{\theta}{\rm max} ~ \log p_{\theta}(x) 
&= \underset{q}{\rm max} ~ ELBO(q)
\end{aligned}
$$



## KL-divergence and ELBO


$$
\begin{aligned}
D_{KL}( q(z) \vert\vert p(z|x) ) 
&= \int_{Z} q(z) \frac{q(z)}{p(z|x)} \\
&= - H_q(Z) - \mathbb{E}_{z \sim q} \left[ \log p(z|x) \right]
\end{aligned}
$$

ELBO is considered as the difference between Log likelihood  $$\log p(x) $$ and KL-divergence  $$D_{KL}( q(z) \vert\vert p(z|x) ) $$.

$$
\begin{aligned}
ELBO 
&= \mathbb{E}_{z \sim q} \left[ \log p(x,z) \right] + H_q(Z) \\
&= \mathbb{E}_{z \sim q} \left[ \log p(x) + \log p(z|x) \right] + H_q(Z) \\
&= \mathbb{E}_{z \sim q} \left[ \log p(x) \right] + \mathbb{E}_{z \sim q} \left[ \log p(z|x) \right] + H_q(Z) \\
&= \log p(x) + H_q(Z) + \mathbb{E}_{z \sim q} \left[ \log p(z|x) \right] \\
&= \log p(x) - D_{KL}( q(z) \vert\vert p(z|x) ) 
\end{aligned}
$$



So that, we can obtain the below relation.
$$
\begin{align}
\underset{\theta}{\rm max} ~ \log p_{\theta}(x) 
&= \underset{q}{\rm max} ~ ELBO(q) \\
&= \underset{q}{\rm min} ~ D_{KL}( q(z) \vert\vert p(z|x) ) 
\end{align}
$$
