---
layout: post
title: 単回帰による予測
tags: Python 線型回帰
---

単純線形回帰(simple linear regressin)は、特徴量が１つの線形回帰。２つのパラメータ$w$ (weight)と$b$ (bias)によって特徴づけられる式 $y = wx + b$ を予測モデルとする。ここでは一般的な最小二乗法で問題を解く。

### 数式

$$ y = wx + b $$

$$ D=\{(x_{1},t_{1}),(x_{2},t_{2}),...(x_{N},t_{N}) \} = {(x_{n},t_{n})}^{N}_{n=1} $$

中心化によるbias項の削減

$$ x_{c} = x - \overline{x} $$

$$ t_{c} = t - \bar{t} $$

$$ y = wx $$

二乗和誤差 (sum-of-squares error) が 0 のとき、$t = y$

$$ L = \Sigma^{N}_{n=1}(t_{n}-y_{n})^2 =  \Sigma^{N}_{n=1}(t_{n}-wx_{n})^2 $$

$$ w = \frac{\Sigma^{N}_{n=1}x_{n}t_{n}}{\Sigma^{N}_{n=1}x_{n}^2} $$

$$ b = - \overline{x}w + \bar{t} $$

### コード

boston housingから部屋数(RM)を使う。

```python
import matplotlib.pyplot as plt
import matplotlib.cm as cm
from sklearn.datasets import load_boston

boston = load_boston()
x = boston.data[:, 5]  # RM
t = boston.target

def simpleLinearRegression(x, t):
    xc = x - x.mean()
    tc = t - t.mean()
    w = xc.dot(tc) / (xc**2).sum()
    b = -x.mean()*w + t.mean()
    return w, b

w, b = simpleLinearRegression(x, t)

sns.set(style="whitegrid", palette="tab10")
ax = plt.figure().gca()
ax.scatter(x, t, c=(cm.tab10(9),))
ax.plot((x.min(), x.max()), (w*x.min()+b, w*x.max()+b))

ax.set_title("Housing Prices in Boston")
ax.set_xlabel("RM")
ax.set_ylabel("Price")
plt.show()
```

![Housing Prices]({{ site.baseurl }}/assets/images/simple_regression.jpg)
