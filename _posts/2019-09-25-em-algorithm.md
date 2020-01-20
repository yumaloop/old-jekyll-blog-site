---
layout: post
title: EM Algorithm
categories:
    - Stat/ML
tags:
    - hoge
    - foo
---

![EM algorithm header img](https://www.mathworks.com/matlabcentral/mlc-downloads/downloads/submissions/65772/versions/3/screenshot.png"EM algorithm")

　EM アルゴリズムは，不完全データに基づく統計モデル一般に適用される，**最尤推定量を導出するためのアルゴリズム**です．
もともと，「不完全データ・完全データ」という概念は欠損データの問題に対処するために立てられましたが，定義を拡張することで，切断データ・打ち切りデータ・混合分布モデル・ロバスト分布モデル・潜在変数モデル・ベイズモデルなどにも適用できます．

　クラスタリングや教師なし学習に用いられる多くの手法（例：k-means，混合ガウス分布モデル）は，計算過程に着目するとEMアルゴリズムとして一般化できます．さらに，情報幾何の観点からEMアルゴリズムを解析した研究も盛んで，指数分布族をもつ確率モデルに対するEMアルゴリズムの適用は，e-射影/m-射影という形式でまとめられます．



<br>



## 1. 統計的推測

> 目的：ある変数$$x \in X$$ が従う確率分布$$q(x)$$を求めたい．

このとき，パラメータ$$\theta \in \Theta$$で決定される確率モデル$$p(x \vert \theta)$$を考え，$n$コのデータサンプル$$ \mathcal{D} := {\\{x\_i\\}}\_{i=1}^{n}$$から最適なパラメータ$$\theta^{*} \in \Theta$$を選ぶことで

$$
x \sim q(x) \approx p(x|\theta)
$$

という近似式を成り立たせたい．これを統計的推測（推定）という．



<br>




## 2. 最尤推定

統計的推測の最も基本的なアルゴリズムは最尤推定である．対数尤度関数:

$$
\ell(\theta | x) := \log p(x | \theta)
$$

を定義して，$$n$$コのデータサンプル$$\mathcal{D} := {\{x_i\}}_{i=1}^{n}$$から，

$$
J(\theta) := \frac{1}{n} \sum_{i=1}^{n} \ell(\theta | x_i), ~~~
\hat{\theta}_{MLE} = \underset{\theta \in \Theta}{\rm argmax} ~ J(\theta)
$$

を解けば，パラメータ$$\theta$$の最尤推定量$$\hat{\theta}_{MLE}$$を求めることができる．



<br>



## 3. EMアルゴリズム

- 完全データ$$x \in X$$ : <br>
    - 観測不可能だが，求めたい確率分布$$p(x)$$に完全に従うデータ．
- 不完全データ$$y \in Y$$ : <br>
    - 観測可能だが，求めたい確率分布$$p(x)$$に完全に従わないデータ．

を考える．完全データ$x \in X$ と不完全データ$$y \in Y$$の関係は一般に「$$x : y$$ = 1 : 多」となるが，ここでは応用上都合の良い仮定として，潜在変数$$x \in Z$$を用いてこれを表す．すなわち，$$ x = [y, z] $$と仮定する．

このとき，完全データ$$x$$ の確率モデル：

$$
p(x | \theta) = p(y, z | \theta)
$$

を考える，ここで，変数$$x$$のデータサンプル$$y_i$$は観測できないため尤度関数$$p(x_i \vert \theta)$$は計算できないが，変数$$y$$のデータサンプル$$y_i$$は観測できることを利用して，

$$
\begin{eqnarray}
p(y | \theta) 
&=& \int_{Z} p(x | \theta) ~ dz \\
&=& \int_{Z} p(y, z | \theta) ~ dz
\end{eqnarray}
$$

という量を考える．このとき，変数$$x \in X$$ が従う確率分布$$q(x)$$ を近似する確率モデル$$p(x \vert \theta)$$ を定めるための計算法としてEMアルゴリズムが登場する．具体的には以下の通り．

> 1. 初期値$$\theta^{0}$$を与える．
> 2. For each step $$t$$: <br>
>     - E step <br>
>     期待値（Expectation）$$Q$$ を計算する．
>     $$ Q(\theta | \theta^{(t)}) = \mathbb{E}_{x \sim p(x|\theta)} \left[ \log p(x|\theta) | y, \theta^{(t)} \right] $$
> 
>     - M step <br>
>     期待値 $$Q$$ を最大化（Maximzation）するパラメータ$$\theta$$を求める．
>     $$ \theta^{(t+1)} = \underset{\theta \in \Theta}{\rm argmax} ~ Q(\theta | \theta^{(t)}) $$
> 3. 収束した値$$\theta^{(\infty)}$$ を推定値$$ \hat{\theta}_{EM}$$ とし，確率モデル$$p(x \vert \hat{\theta}_{EM})$$ を得る．



E step と M step で行う計算をまとめると，

> - EM step <br>
> $$
> \begin{eqnarray}
> \theta^{(t+1)} 
> &=& \underset{\theta \in \Theta}{\rm argmax} ~ \mathbb{E}_{x \sim p(x|\theta)} \left[ \log p(x | \theta) | y, \theta^{(t)} \right] \\\
> &=& \underset{\theta \in \Theta}{\rm argmax} ~ \mathbb{E}_{(y,z) \sim p(y,z|\theta)} \left[ \log p(y, z | \theta) | y, \theta^{(t)} \right] \\\
> &=& \underset{\theta \in \Theta}{\rm argmax} ~ \mathbb{E}_{z | {\theta}^{(t)} \sim \int_{Y} p(y,z | {\theta}^{(t)}) dy} \left[ \log p(y, z | \theta) \right]
> \end{eqnarray}
> $$

となる．

<br>

## 参考文献

- <a href="http://ebsa.ism.ac.jp/ebooks/sites/default/files/ebook/1881/pdf/vol3_ch9.pdf">9章 EMアルゴリズム - 「21 世紀の統計科学」第 III 巻 日本統計学会, 2008</a>
- <a href="https://staff.aist.go.jp/s.akaho/papers/josho-main.pdf">解説 EMアルゴリズムの幾何学 - 赤穂昭太郎, 電子技術総合研究所</a>
- <a href="https://www.ism.ac.jp/~shiro/papers/books/embook2000.pdf">EMアルゴリズムと神経回路網, 2000, 統計数理研究所</a>
