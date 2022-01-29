---
layout: post
title: ハイパーパラメータのチューニング
tags: Python アイリスの分類
---

Scikit-learnのSVC()には、正規化パラメーター $C$ とカーネルのバンド幅 $γ$ の２つのパラメータが存在する。それぞれ 0.001, 0.01, 0.1, 1, 10, 100 を試してみると 6 x 6 = 36通りの組み合わせがある。グリッドリサーチでは、この36通り全てについてモデルの学習と評価を行う。

### 単純なグリッドリサーチ

```python
from sklearn.svm import SVC

params = [0.001, 0.01, 0.1, 1, 10, 100]

best_score = 0
best_parameters = {}

for gamma in params:
    for C in params:
        svm = SVC(C=C, gamma=gamma)
        svm.fit(X_train, y_train)
        score = svm.score(X_test, y_test)
        if score > best_score:
            best_score = score
            best_parameters = {"C": C, "gamma": gamma }

print(best_score, best_parameters)
#=> 0.9736842105263158 {'C': 100, 'gamma': 0.001}
```
