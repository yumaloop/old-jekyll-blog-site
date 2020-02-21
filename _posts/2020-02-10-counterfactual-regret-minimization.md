---
layout: post
title: Counterfactual Regret Minimization
lang: en
categories:
    - StatML
tags:
    - hoge
    - foo
---



## Counterfactual Regret Minimization (CFR Algorithm)


### Notation

- Set, variables
  - $$N: $$ set of players
    - $$i \in N$$: player
  - $$A :$$ set of actions
    - $$a \in A: $$ action
  - $$H: $$set of sequences
    - $$h \in H: $$ sequences (= possible history of actions, $$h = (a_1, \dots, a_t$$)
    - $$Z \subseteq H: $$ set of terminal histories
- Function, relations
  - $$u_i: Z \to \mathbb{R}: $$ utility function of player $$i$$
  - $$\sigma_i: A \to [0,1]$$ a strategy of player $$i$$, probability distribution on action set $$A$$.
  - $$\sigma~: A^N \to [0,1]$$ a strategy profile, $$\sigma := (\sigma_1, \dots, \sigma_N)$$
  - $$\pi^{\sigma}_i: H \to [0,1]: $$ probability of history $$h$$ under a strategy of player $$i$$ $$\sigma_i$$

  - $$\pi^{\sigma}: H^N \to [0,1]: $$ probability of history $$h$$ under a strategy profile  $$\sigma$$
  



Then, you can also interplate $$u_i$$ as the functino mapping a storategy profile $$\sigma$$ to its utility. 


$$
\begin{align}
u_i(\sigma) 
&= \sum_{h \in Z} u_i(h) \pi^{\sigma}(h) \\
&= \sum_{h \in Z} u_i(h) \prod_{i \in N} \pi^{\sigma}_i(h)
\end{align}
$$




### Nash equilibrium



**Definition:** $$(\text{Nash equilibrium})$$

In $$N$$-player extensive game, a strategy profile $$\acute{\sigma} := (\acute{\sigma_1}, \dots, \acute{\sigma_N})$$ is the Nash equilibrium if and only if the followings holds.
$$
\begin{align}
u_1(\acute{\sigma_1}, \dots, \acute{\sigma_N}) 
&\geq \underset{\sigma_1}{\rm max} ~ u_1(\sigma_1, \acute{\sigma_{-1}}) \\
u_2(\acute{\sigma_1}, \dots, \acute{\sigma_N}) 
&\geq \underset{\sigma_2}{\rm max} ~ u_2(\sigma_2, \acute{\sigma_{-2}}) \\
&~ \vdots \\
u_N(\acute{\sigma_1}, \dots, \acute{\sigma_N}) 
&\geq \underset{\sigma_N}{\rm max} ~ u_N(\sigma_N, \acute{\sigma_{-N}})
\end{align}
$$


**Definition: $$\text{(}\varepsilon\text{-Nash equilibrium)}$$**

In $$N$$-player extensive game, a strategy profile $$\acute{\sigma} := (\acute{\sigma_1}, \dots, \acute{\sigma_N})$$ is the $$\varepsilon$$-Nash equilibrium if and only if the followings holds when $$\forall \varepsilon \geq 0$$ is given.
$$
\begin{align}
u_1(\acute{\sigma_1}, \dots, \acute{\sigma_N}) + \varepsilon
&\geq \underset{\sigma_1}{\rm max} ~ u_1(\sigma_1, \acute{\sigma_{-1}}) \\
u_2(\acute{\sigma_1}, \dots, \acute{\sigma_N}) + \varepsilon
&\geq \underset{\sigma_2}{\rm max} ~ u_2(\sigma_2, \acute{\sigma_{-2}}) \\
&~ \vdots \\
u_N(\acute{\sigma_1}, \dots, \acute{\sigma_N}) + \varepsilon
&\geq \underset{\sigma_N}{\rm max} ~ u_N(\sigma_N, \acute{\sigma_{-N}})
\end{align}
$$


- Average overall regret








### Counterfactual regret



**Regret**

- Average overall regret of player $$i$$ at time $$T$$：

$$
R_i^T 
:= \frac{1}{T} \underset{\sigma_i^*}{\rm max} \sum_{t=1}^{T} \left( u_i(\sigma_i^*, \sigma_{-i}^{t}) - u_i(\sigma_i^t, \sigma_{-i}^{t}) \right)
$$



- Average strategy for player $$i$$ from time $$1$$ to $$T$$：

$$
\begin{align}
\overline{\sigma}_i^t(I)(a) 
&:= \frac{\sum_{t=1}^{T} \pi_i^{\sigma^t}(I) \cdot \sigma^t(I)(a)}{\sum_{t=1}^{T} \pi_i^{\sigma^t}(I)} \\
&= \frac{\sum_{t=1}^{T} \sum_{h \in I} \pi_i^{\sigma^t}(h) \cdot \sigma^t(h)(a)}{\sum_{t=1}^{T} \sum_{h \in I} \pi_i^{\sigma^t}(h)} \\
\end{align}
$$



- Counterfactual utility：

$$
\begin{align}
u_i(\sigma, I) = \frac{\sum_{h \in H, h' \in Z} \pi_{-i}^{\sigma}(h)\pi^{\sigma}(h,h')u_i(h) }{\pi_{-i}^{\sigma}(I)}
\end{align}
$$



- Immediate counterfactual regret：

$$
\begin{align}
R_{i,imm}^{T}(I)
:= \frac{1}{T} \underset{a \in A(I)}{\rm max} \sum_{t=1}^{T} 
\pi_{-i}^{\sigma^t}(I)
\left( 
u_i(\sigma^t_{I \to a}, I) - u_i(\sigma^t, I) 
\right)
\end{align}
$$















