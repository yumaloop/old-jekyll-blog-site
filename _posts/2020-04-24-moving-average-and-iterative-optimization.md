---
layout: post
title: Moving Average and Iterative Optimization Algorithm
lang: ja
categories:
    - Econ
tags:
    - Tips
---





# 移動平均と逐次最適化

多くの動的モデル（Dynamic Model-System）は，定常状態（time-steady state）に至ることを目的として設計される．ここで，定常状態とは，「ある変数$X$に作用する，何らかの時変量（parameter $\theta_t$）や関数（$L_t(x; \theta)$）が一定値に収束すること」と定義しておく．

そして，着目している動的モデルが定常状態に至るプロセスは，システム同定（System identification）と呼ばれ，定常状態を示した概念として，均衡（equilibrium）や平衡（balance）と呼ばれる用語が使われる．





1. [化学反応](https://ja.wikipedia.org/wiki/化学反応)においては、[可逆反応](https://ja.wikipedia.org/wiki/可逆反応)の生成物の変化量と出発物質の変化量が合致した状態を指す。[化学平衡](https://ja.wikipedia.org/wiki/化学平衡)を参照。
2. [力学](https://ja.wikipedia.org/wiki/力学)においては、物体に加わっている全ての[力](https://ja.wikipedia.org/wiki/力_(物理学))の合力と[力のモーメント](https://ja.wikipedia.org/wiki/力のモーメント)の和がともに 0 である状態を平衡と呼ぶ。[力学的平衡](https://ja.wikipedia.org/w/index.php?title=力学的平衡&action=edit&redlink=1)（[英語](https://ja.wikipedia.org/wiki/英語): [Mechanical equilibrium](https://en.wikipedia.org/wiki/Mechanical_equilibrium)）を参照。
3. [熱力学](https://ja.wikipedia.org/wiki/熱力学)においては通常、[熱平衡](https://ja.wikipedia.org/wiki/熱平衡)、[力学的平衡](https://ja.wikipedia.org/wiki/熱力学的平衡)、[化学平衡](https://ja.wikipedia.org/wiki/化学平衡)の三つを合わせて、[熱力学的平衡](https://ja.wikipedia.org/wiki/熱力学的平衡)とよぶ。
4. [統計力学](https://ja.wikipedia.org/wiki/統計力学)においては、系のエネルギー分布が、ボルツマン分布に従うことである。[熱力学的平衡](https://ja.wikipedia.org/wiki/熱力学的平衡)を参照。
5. [物理化学](https://ja.wikipedia.org/wiki/物理化学)においては、複数の物質相から構成される系において、相間の物質の出入りが合い等しい状態を指す。[相平衡](https://ja.wikipedia.org/wiki/相平衡)を参照。
6. [電気工学](https://ja.wikipedia.org/wiki/電気工学)においては、信号源と負荷の間のインピーダンスが合致していることを指す。[インピーダンス平衡](https://ja.wikipedia.org/wiki/インピーダンス平衡)を参照。
7. [電気回路](https://ja.wikipedia.org/wiki/電気回路)においては、信号回路の双方が接地点に接続されていないことを指す。[平衡接続](https://ja.wikipedia.org/wiki/平衡接続)を参照。
8. [情報工学](https://ja.wikipedia.org/wiki/情報工学)においては、データ[木構造](https://ja.wikipedia.org/wiki/木構造_(データ構造))の任意の節においてその配下の節点の数が等しい状態を指す。
9. [生態学](https://ja.wikipedia.org/wiki/生態学)においては、生物群集間の分布と個体数の変化が無い状態を指す。
10. [生理学](https://ja.wikipedia.org/wiki/生理学)においては、水平であることを認知することを指す。[平衡感覚](https://ja.wikipedia.org/wiki/平衡感覚)を参照。
11. [経済学](https://ja.wikipedia.org/wiki/経済学)においては、[需要と供給](https://ja.wikipedia.org/wiki/需要と供給)が釣り合って[価格](https://ja.wikipedia.org/wiki/価格)が不動になることなどを指す。[均衡](https://ja.wikipedia.org/wiki/均衡)を参照。



連立(微分)方程式で記述できるため．



### なぜ線形モデルが有用なのか？

答えはシンプルで，Taylor展開


$$
\begin{align}
\frac{d x_1}{d t} &= f_1(x_1, \dots, x_n; \theta_1) \\
\frac{d x_2}{d t} &= f_2(x_1, \dots, x_n; \theta_1) \\
&\vdots \\
\frac{d x_n}{d t} &= f_n(x_1, \dots, x_n; \theta_1) 
\end{align}
$$


このモデルが，定常状態にいる場合，


$$
\begin{align}
	f_1 = f_2 = \cdots = f_n = 0
\end{align}
$$
が成り立つから，$n$個の変数${\bf x} = (x_1, x_2, \dots, x_n)$に対して，$n$個の方程式が得られる．この解がシステムの定常化となる．これを${\bf x}^* = (x_1^*, x_2^*, \dots, x_n^*)$とおくと，$f_1, f_2, \dots, f_n$に対して，点${\bf x}^*$の近傍でTaylo​r展開が可能になる．


$$
\begin{align}
\frac{d x_1}{d t} 
= f_1(x_1, \dots, x_n; \theta_1) 
=& a_{11}(x_1 - x_1^*) + a_{12} {(x_2 - x_2^*)} + \cdots a_{1n} {(x_n - x_n^*)} + \\
& a_{111}{(x_1 - x_1^*)}^2 + a_{112}(x_1 - x_1^*)(x_2 - x_2^*) + \cdots + a_{11n}{(x_1 - x_1^*)}^2 + \\
& ~~ \vdots \\
& a_{11\cdots1}{(x_1 - x_1^*)}^n + \cdots \\
\frac{d x_2}{d t} 
= f_2(x_1, \dots, x_n; \theta_1) 
=& a_{21}(x_1 - x_1^*) + a_{22} {(x_2 - x_2^*)} + \cdots a_{2n} {(x_n - x_n^*)} + \\
& a_{211}{(x_1 - x_1^*)}^2 + a_{212}(x_1 - x_1^*)(x_2 - x_2^*) + \cdots + a_{21n}{(x_1 - x_1^*)}^2 + \\
& ~~ \vdots \\
& a_{21\cdots1}{(x_1 - x_1^*)}^n + \cdots \\ \\
& ~~ \vdots \\ \\
\frac{d x_n}{d t} 
= f_n(x_1, \dots, x_n; \theta_1) 
=& a_{n1}(x_1 - x_1^*) + a_{n2} {(x_2 - x_2^*)} + \cdots a_{nn} {(x_n - x_n^*)} + \\
& a_{n11}{(x_1 - x_1^*)}^2 + a_{n12}(x_1 - x_1^*)(x_2 - x_2^*) + \cdots + a_{n1n}{(x_1 - x_1^*)}^2 + \\
& ~~ \vdots \\
& a_{n1\cdots1}{(x_1 - x_1^*)}^n + \cdots \\
\end{align}
$$


つまり，任意の微分可能関数$f_1, f_2, \dots, f_n$によって表現されたダイナミクスをもつ動的モデルは，（定常解の近傍では）任意のn次多項式によって近似できる．これにより，線形システムの妥当性が保証される．一般解は，


$$
\begin{align}
x_1 
=& ~ x_1^*  + C_{11}e^{\lambda_1 t} + C_{12}e^{\lambda_2 t} + \cdots C_{1n}e^{\lambda_n t} + \\
 & ~~~~~~~~~~ C_{111}e^{2\lambda_1 t} + C_{112}e^{2\lambda_2 t} + \cdots + C_{11n}e^{2\lambda_n t} + \\
 & ~~~~~~~~~~ ~~ \vdots \\
  & ~~~~~~~~~~ C_{11\cdots1}e^{n\lambda_1 t} + C_{11\cdots2}e^{n\lambda_2 t} + \cdots + C_{11\cdots n}e^{n\lambda_n t} \\
x_2
=& ~ x_2^*  + C_{21}e^{\lambda_1 t} + C_{22}e^{\lambda_2 t} + \cdots C_{2n}e^{\lambda_n t} + \\
 & ~~~~~~~~~~ C_{211}e^{2\lambda_1 t} + C_{212}e^{2\lambda_2 t} + \cdots + C_{11n}e^{2\lambda_n t} + \\
 & ~~~~~~~~~~ ~~ \vdots \\
  & ~~~~~~~~~~ C_{21\cdots1}e^{n\lambda_1 t} + C_{21\cdots2}e^{n\lambda_2 t} + \cdots + C_{21\cdots n}e^{n\lambda_n t} \\ \\
& ~~~~~~~~~~ ~~ \vdots
\\ \\
x_n
=& ~ x_n^*  + C_{n1}e^{\lambda_1 t} + C_{n2}e^{\lambda_2 t} + \cdots C_{nn}e^{\lambda_n t} + \\
 & ~~~~~~~~~~ C_{n11}e^{2\lambda_1 t} + C_{n12}e^{2\lambda_2 t} + \cdots + C_{n1n}e^{2\lambda_n t} + \\
 & ~~~~~~~~~~ ~~ \vdots \\
  & ~~~~~~~~~~ C_{n1\cdots1}e^{n\lambda_1 t} + C_{n1\cdots2}e^{n\lambda_2 t} + \cdots + C_{n1\cdots n}e^{n\lambda_n t} \\


\end{align}
$$
となる．







### モデルとは何か？

つまり，多くの分野において数理モデルとか計量モデルとか呼ばれるものは，以下の手続きを必要とする．



1. 定常状態に至ることを目的とする**動的モデル**を定義する
2. モデルの状態変化を**最適化問題(過程**)として定式化する．
3. 定常状態への収束が保証された**最適化アルゴリズム**を考える



### 移動平均



- カルマンフィルタ
- Adamに置けるモメンタム
- 強化学習の報酬
- ゲーム理論におけるFicticious Play
- 株価におけるテクニカル分析移動平均（ARMA）
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














