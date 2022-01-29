---
layout: post
title: 重回帰による予測
tags: Python 線型回帰
---

特徴量が複数ある線形回帰。

### 数式

$$ y = w_{1}x_{1} + w_{2}x_{2} +,... + w_{m}x_{m} + b $$

$w_{0} = 1, \; x_{0} = b$ と置く

$$ \Sigma^M_{m=0}w_{m}x_{m} $$

$$ y = \bf{w}^{T}x   $$

$$ L = (t_{1}-y_{1})^2 + (t_{2}-y_{2})^2 +,... + (t_{N}-y_{N})^2 $$

$$ L = \bf(t - y)^T(t - y) $$

$$ \bf w^Tx \rm = \bf x^Tw $$

$$ \bf x^T_{n} \rm =  [x_{n0}, x_{n1}, x_{n2},..x_{nM}](n=1..N)  $$

NxM行列 $\bf X$

$$ y = \bf Xw $$

$$ L = \bf t^Tt \rm - 2 \bf t^TXw \rm + \bf w^TX^TXw $$

$$ \bf w \rm = \bf X^{-1}t $$

### コード

```python
import numpy as np
from sklearn.datasets import load_boston
from sklearn.linear_model import LinearRegression

boston = load_boston()
X = np.c_[boston.data[:,4], boston.data[:,5]] # NOX, RM
y = boston.target.reshape(-1, 1)

def _LinearRegression(X, y):
    Xb = np.c_[np.ones(X.shape[0]), X] # バイアス項の追加
    a = np.dot(Xb.T, Xb)
    b = np.dot(Xb.T, y)
    w = np.linalg.solve(a, b)

    return w[1:].reshape(1, -1), w[0]

w, b = _LinearRegression(X, y)
print(w, b)

model = LinearRegression()
model.fit(X, y)
print(model.coef_, model.intercept_)
#->
[[-18.97061905   8.15665581]] [-18.20588464]
[[-18.97061905   8.15665581]] [-18.20588464]
```
