---
layout: post
title: Particle Filter
lang: ja
categories:
    - StatML
tags:
    - Bayes

---

<div class='pixels-photo'>
<a href='https://500px.com/photo/80813059/Flow-of-Time-by-Jimin--Jacob' alt='Flow of Time... by Jimin  Jacob on 500px.com'>
  <img src='https://drscdn.500px.org/photo/80813059/m%3D900/v2?sig=9c99293070333accc348ef432437c6c0eeb6b87f5d14199485cae5d51d92e3db' alt='Flow of Time... by Jimin  Jacob on 500px.com' />
</a>
</div>
<script type='text/javascript' src='https://500px.com/embed.js'></script>

**State Space Model (SSM)**

State Space Model(SSM) is widely used in the field requiring the sequential estimation or online learning.
This model is effective if you consider a system having two different variables; one completely represents the actual state but cannot be observed and the other partially represents the actual state but can be observed. Here, I call the former $$x$$ (state variable) and the latter $$y$$ (observation variable).

In SSM, we intruduce the following equations $$F, H$$ (or $$f, h$$) and identify them by observed data sample $$[y_1, \dots, y_t]$$.

- Equation of each state $$x_t$$ : 

  $$
  \begin{aligned}
  x_{t+1} &= F(x_t) ~~ (\text{Deterministic process}) \\
  x_{t+1} &\sim f(\cdot\vert x_t) ~~ (\text{Stochastic process}) 
  \end{aligned}
  $$

- Equation of each observation $$y_t$$ :

  $$
  \begin{aligned}
  y_t &= H(x_t) ~~ (\text{Deterministic process}) \\
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
   



