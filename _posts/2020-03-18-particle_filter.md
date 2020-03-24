---
layout: post
title: Particle Filter
lang: ja
categories:
    - StatML
tags:
    - Bayes

---



**State Space Model (SSM)**

SSM is the model 

A system in which the variables of actual state and the variables that can be actually observed are different.



It is widely used in the field requiring the sequential estimation or online learning.



- Equation of each state $$x_t$$ : 
  $$
  \begin{aligned}
  x_{t+1} &= F(x_t) ~~ (\text{Decision process}) \\
  x_{t+1} &\sim f(\cdot\vert x_t) ~~ (\text{Stochastic process}) 
  \end{aligned}
  $$

- Equation of each observation $$y_t$$ :
  $$
  \begin{aligned}
  y_t &= H(x_t) ~~ (\text{Decision process}) \\
  y_t &\sim h(\cdot \vert x_t) ~~ (\text{Stochastic process}) 
  \end{aligned}
  $$



**Perticle filter**



1. For each $$i$$ in $$[1 … M]$$

   1. (Prediction)

      Derive prediction distribution $$f(x_t \vert \cdot)$$ depends on particles $$\hat{x}_{t-1}$$.

      Sample $$x^{i}_{t \vert t-1} ~~~ (i = 1, \dots, M)$$ following $$f(x_t \vert \cdot)$$.
      $$
      x^{i}_{t \vert t-1} \sim f(x_t \vert \hat{x}_{t-1})
      $$
      

   2. (Likelihood)

      Derive the likelihood of $$x^i_{t \vert t-1}$$ from given sample data $$y_t$$ based on $$h(\cdot)$$
      $$
      w^i_t \sim h(y_t \vert x^i_{t \vert t-1})
      $$
      

2. (Resampling)

   Resampe $$\hat{x}^i_{t \vert t-1}$$ based on the likelihood $$w^i_t ~~~ (i=1,\dots,M)$$ .

   Derive the filter distribution $$p(x_t \vert y_{1:t})$$ for any $$x_t$$:
   $$
   \begin{aligned}
   p(x_t \vert y_{1:t}) 
   &\approx \frac{1}{M} \sum_{i=1}^{M} \delta(x_t - \hat{x}^i_{t \vert t-1}) \\
   &\approx \sum_{i=1}^{M} \frac{}{\sum_{i=1}^{M} } \delta(x_t - \hat{x}^i_{t \vert t-1}) \\
   \end{aligned}
   $$
   



