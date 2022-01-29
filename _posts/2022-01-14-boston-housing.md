---
layout: post
title: ボストン住宅価格
tags: Python 線型回帰
---

skleanの組み込みデータセットboston Housingを線形回帰の練習に使う。データセットを調べると、代表的な配布元はCarnegie Mellon University([lib.stat.cmu.edu/datasets/boston](http://lib.stat.cmu.edu/datasets/boston))。1978年のEconomics & Management誌に掲載された論文のデータであり、自動車排出ガス等の窒素酸化物（NOx）がボストン市内の住宅価格に与える影響を分析したものらしい。

![boston acorn street](https://source.unsplash.com/UpYF6ibFud0/600x400)

### sklearnによる線形回帰

線形回帰モデルで特徴量の重み係数をアウトプットすると、確かにNOXが住宅価格に大きくマイナスの影響を与えている。しかしデータの標準化、重みの正則化を一切していないので、もちろん評価は保留する。

```python
from sklearn.datasets import load_boston
from sklearn.linear_model import LinearRegression, Ridge, Lasso

boston = load_boston()
X = boston.data
y = boston.target

model = LinearRegression()
model.fit(X, y)

for feature_name, w in zip(boston.feature_names, model.coef_):
    print(f"{feature_name:10} {round(w, 5)}")
# ->
CRIM       -0.10801
ZN         0.04642
INDUS      0.02056
CHAS       2.68673
NOX        -17.76661
RM         3.80987
AGE        0.00069
DIS        -1.47557
RAD        0.30605
TAX        -0.01233
PTRATIO    -0.95275
B          0.00931
LSTAT      -0.52476
```
