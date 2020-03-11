---
layout: post
title: Free energy and Bayes inference
lang: ja
categories:
    - StatML
tags:
    - Bayes

---



**平均場近似と自由エネルギー**

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
D_{KL}(q \vert\vert p) := \int_{X} q(x) \log \frac{q(x)}{p(x)} 
\end{aligned}
$$

は以下のように分解できる．

$$
\begin{aligned}
D_{KL}(q \vert\vert p) 
&= \beta \int_X q(x)\phi(x) - \left\{ - \int_X q(x)\log q(x) \right\} + \log \int_X \exp(-\beta \phi(x)) \\
&= \beta~ \mathbb{E}_{x \sim q}[\phi(x)] - H_q(X) + \log Z^{\phi}(\beta) \\
&= \beta~ (\text{Internal energy}) - (\text{Entropy}) + (\text{Const.}) \\
\end{aligned}
$$

いま，近似分布$$q(x)$$に対する汎関数として，自由エネルギー:

$$
F^{\phi}(q) := \mathbb{E}_{x \sim q}[\phi(x)] - \frac{1}{\beta}H_q(X) ~~~ (\text{Free energy})
$$

を定義すれば，

$$
D_{KL}(q \vert\vert p)  = \beta~ F^{\phi}(q) + \log Z^{\phi}(\beta)
$$

となるから，$$q(x)$$による$$p(x)$$の近似問題は次式で表現できる．

$$
\begin{aligned}
\underset{q}{\rm min} ~ D_{KL}(p \vert\vert q) &= 
\underset{q}{\rm min} ~ F^{\phi}(q)
\end{aligned}
$$

また．自由エネルギー$$F^{\phi}(q)$$の最小値は，

$$
{F^{\phi}}^{*}(q) = - \frac{1}{\beta} \log \int_X \exp (-\beta \phi(x)) = - \frac{1}{\beta} \log Z^{\phi}(\beta)
$$

となる．すなわち，

$$
\begin{aligned}
{F^{\phi}}(q) 
= - \frac{1}{\beta} \log Z^{\phi}(\beta) 
~~ \Leftrightarrow ~~ 
D_{KL}(p \vert\vert q) = 0
~~ \Leftrightarrow ~~ 
p(\cdot) \equiv	 q(\cdot)
\end{aligned}
$$

となる．



<br>

**熱力学(統計力学)との関係**

温度$$T$$，内部エネルギー$$U$$，エントロピー$$S$$に対して，Helmholtzの自由エネルギー$$F$$は以下のように定義される．

$$
F = U - TS
$$

$$F^{\phi}(q)$$の定義式で，$$F = F^{\phi}(q)$$，$$U = \mathbb{E}_{x \sim q}[\phi(x)]$$，$$S = H_q(X)$$とおけば，

$$
\begin{aligned}
\beta F &= \beta U - S \\
F &= U - \frac{1}{\beta} S
\end{aligned}
$$

となるから，汎関数$$F^{\phi}(q)$$は，熱力学におけるHelmholtzの自由エネルギー$$F$$と類似した形式を持っていることがわかる．なお，Bayes理論において定数$$\beta$$は「逆温度」と呼ばれるが，これは温度$$T$$に由来する．





<br>

**Bayes脳やFEPとの関係**

神経科学の分野でK.Fristonによって提唱された自由エネルギー原理(Free energy principle, FEP)は，上にある汎関数$$F^{\phi}(q)$$を変分推論を組み合わせたものである（と解釈できる）．ここでは，ELBOとの関係にのみ触れておく．

<br>

ELBOの定義式:

$$
\begin{aligned}
(\text{Evidence}) 
&= \log p(y) \\
&\geq \mathbb{E}_{\theta \sim q}[\log p(y,\theta)] - \mathbb{E}_{\theta \sim q}[\log q(\theta)] \\
&= \mathcal{L}_{ELBO}(q) \\
&= (\text{Evidence Lower Bound})
\end{aligned}
$$

と<a href="[https://www.fil.ion.ucl.ac.uk/~karl/The%20free-energy%20principle%20-%20a%20rough%20guide%20to%20the%20brain.pdf](https://www.fil.ion.ucl.ac.uk/~karl/The free-energy principle - a rough guide to the brain.pdf)">FristonのCell論文(2009)</a>にある自由エネルギーの定義式

$$
F(y) = - \mathbb{E}_{\theta \sim q}[\log p(y,\theta)] + \mathbb{E}_{\theta \sim q}[\log q(\theta)] 
$$

を比べると，以下の関係が得られる．

$$
\begin{aligned}
F(y) 
&= - \mathbb{E}_{\theta \sim q}[\log p(y,\theta)] + \mathbb{E}_{\theta \sim q}[\log q(\theta)] \\
&= -\mathcal{L}_{ELBO}(q) \\
&\geq - \log p(y) \\
&= -(\text{Evidence}) \\
&= (\text{Surprise})
\end{aligned}
$$

つまり，Fristonの自由エネルギー$$F(y)$$は「脳の外部環境$$Y$$に対する観測データ$${\{y_t\}}_{t=1}^{n}$$の対数尤度下限(ELBO)に$$-1$$をかけたもの」である．なお，Bayes推論では,対数尤度$$\log p(y)$$をエビデンス(Evidence)といい，情報理論では負の対数尤度$$-\log p(y)$$をサプライズ(Surprise)という．



<a href="[https://www.fil.ion.ucl.ac.uk/~karl/The%20free-energy%20principle%20-%20a%20rough%20guide%20to%20the%20brain.pdf](https://www.fil.ion.ucl.ac.uk/~karl/The free-energy principle - a rough guide to the brain.pdf)">FristonのCell論文(2009)</a>にあるエージェントの行動$$\alpha$$や脳の内部状態$$\mu$$の更新式:
$$
\begin{aligned}
\alpha^{*} &= \underset{\alpha}{\rm argmin} ~ F(y) \\
\mu^{*} &= \underset{\mu}{\rm argmin} ~ F(y)
\end{aligned}
$$

における$$F(y)$$の最小化は，「脳の外部環境$$Y$$に対する観測データ$${\{y_t\}}_{t=1}^{n}$$の対数尤度(Evidence)」を最大化する過程を表している．

執筆時の個人的な理解としては，FEPにおける各変数$$\theta, \mu, y, \alpha$$の更新規則は，「観測データを用いた最尤推定」そのものだと思っている．論文で提唱されている自由エネルギー$$F(y)$$最小化は，Variational BayesにおけるELBO最大化と同じであるから，むしろ4つの変数間のループ構造（グラフ表現）の方が重要なのだろう． 

<a href="https://www.mendeley.com/viewer/?fileId=05efc940-2dc2-9039-46ca-bb6e1ae1bdba&documentId=ed320c28-3a4c-373a-9089-2a456e2b56f3">FristonのNature論文(2010)</a>では，自由エネルギー$$F(y)$$の定義がより複雑化しており，よく理解していない．