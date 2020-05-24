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



最適化に疎いのでまとめる.



- カルマンフィルタ
- Adamに置けるモメンタム
- 強化学習の報酬
- ゲーム理論におけるFP
- 株価におけるテクニカル分析
- (金融工学)バリュエーションにおけるDCF法





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














