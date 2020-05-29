---
layout: post
title: Moving Average and Iterative Optimization Algorithm
lang: ja
categories:
    - Econ
tags:
    - Tips
---





# 移動平均と逐次最適化アルゴリズム

マルコフ性を仮定したシステムに対して，逐次更新（逐次最適化）を行う場合，指数移動平均(Exponential Moving Average, EMA)がよく使われる．具体的な例として，たとえば以下が挙げられる．

- 分布更新 (カルマンフィルタ)
- モメンタム更新 (SGD, Adam系)
- Policy Gradient (強化学習)
- Fictitious Play (ゲーム理論)
- DCF法 (バリュエーション)
- Moving Average (株式)

この記事では，これら多くの逐次更新アルゴリズムで指数移動平均が用いられることの理由と妥当性について考えてみる．



## 指数移動平均

ある変数$Y$に対して，$t$期の観測値を$Y_t$とすると，$t$期の指数移動平均を$$S_t$は以下のように定義される．

$$
\begin{align}
S_T 
&= \sum_{t=1}^{T} \alpha {(1 - \alpha)}^{T - t} Y_t \\
&= \alpha \left\{ Y_T + (1 - \alpha) Y_{T-1} + {(1 - \alpha)}^2 Y_{T-2} + ... + {(1 - \alpha)}^{T-1} Y_{1} \right\}
\end{align}
$$

あるいは

$$
S_t = \alpha Y_t + (1 - \alpha) S_{t-1} 
$$

となる．





1. 「マルコフ性」のもつ良い性質

何かの時間変化を考えるときに，はとても受け入れられている．

- 線形性: 現在と過去の関係性は1次関数で近似可能 (1次の変化を扱う)
- 正規性: 不確実性は正規分布に従う (2次のばらつきまでを仮定)
- マルコフ性: 現在の状態は，直前の状態にのみ依存する





1. 実データのもつ系列性・反復性

実データは，反復測定が多い．ある指標の定点観測（日別の）





移動平均とは何か？




$$
\begin{align}
m_t &= \gamma \cdot m_t − 1+η \cdot \frac{\partial L(w_t)}{\partial w} \\
w_{t+1} &= w_t - m_t
\end{align}
$$



$$
m_t = g_t + \gamma \cdot g_{t-1} + \gamma^2 \cdot g_{t-2} \cdots + \gamma^t \cdot g_{0}
$$














