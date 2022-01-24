## 回帰分析

回帰、ロジスティック回帰をボストン住宅価格データセットを使いPythonで書いてみる。なおscikit-learnは使わない。

ボストン住宅価格データセットは、Pythonの機械学習ライブラリscikit-learnに含まれる統計学習用のサンプルデータ。

scikit-learnのドキュメントからデータが公開されている[url](http://lib.stat.cmu.edu/datasets/boston)を見ると、雑誌 Journal of Environmental Economics and Management の 1978年4月号に掲載された David Harrison Jr.,Daniel L Rubinfeld による Hedonic housing prices and the demand for clean air という記事のデータであることが分かる。


### ボストンの住宅価格


米国の北東部、半世紀前のボストン。50年前のデータで検討するため、架空の町ということで


50年前の大都市では大気汚染、工場・事業所と自動車等による窒素酸化物（NOx）の排出が問題になっており


### 単回帰

住宅価格 y が、自動車等の排出ガスである窒素酸化物（NOx）の濃度 x の影響を受けているという趣旨。


```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

df = pd.read_csv("./boston.csv", sep=" ")

x = df["NOX"].values
y = df["MEDV"].values

fig = plt.figure()
ax = fig.gca()
ax.scatter(x, y)

plt.show()
```

### 最小2乗法によるフィッティング


単回帰は、 物事が y = ax + b の関係であることを前提とする。

y = wa + b w:weight b:bias

```
x = df["NOX"].to_numpy()
y = df["MEDV"].to_numpy()

def simpleLinearRegression(x, y):
    n = len(x)
    a = ((np.dot(x, y)- y.sum() * x.sum()/n)/
        ((x ** 2).sum() - x.sum()**2 / n))
    b = (y.sum() - a * x.sum())/n

    return a, b

a, b = simpleLinearRegression(x, y)

fig = plt.figure()
ax = fig.gca()
ax.title.set_text("housing prices and Nox")
ax.scatter(x, y, color="pink")
ax.plot([x.min(), x.max()], [a*x.min()+b, a*x.max()+b])

plt.show()
```

### 回帰

y = a1x1 + a2x2 + b

y = w0 + w1x1 + w2x2

w = [w0, w1, w2] を求める




