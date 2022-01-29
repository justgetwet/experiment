---
layout: post
title: 分類における最小二乗法
---

学習データ $X, t$ が与えれらたとき、分類器の出力 $y$ と教師信号 $t$ の二乗和誤差関数を最小化するようにパラメータ行列 $W$ を学習する。

```python
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns
from sklearn.datasets import load_iris

iris = load_iris()

X = iris.data[:100, :2]
y = iris.target[:100]

def f(x, W):
    Wt = W.T
    a = - (Wt[0, 1]-Wt[1, 1]) / (Wt[0, 2]-Wt[1, 2])
    b = - (Wt[0, 0]-Wt[1, 0]) / (Wt[0, 2]-Wt[1, 2])
    return a * x + b

T = np.array([[1, 0] if t else [0, 1] for t in y])

Xb = np.c_[np.ones((len(X), 1)), X]
Xi = np.linalg.inv(np.dot(Xb.T, Xb))
W = np.dot(Xi, Xb.T).dot(T)
print(W)

sns.set()
ax = plt.figure().gca()
ax.scatter(X[:50, 0], X[:50, 1])  # setosa
ax.scatter(X[50:, 0], X[50:, 1])  # versicolor

x = X[:, 0]
x = np.linspace(x.min(), x.max(), 1000)
y = [f(xp, W) for xp in x]
ax.plot(x, y, color="purple")

ax.set_xlabel("sepal length")
ax.set_ylabel("sepal width")
plt.show()
```
