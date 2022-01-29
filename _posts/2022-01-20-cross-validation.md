---
layout: post
title: 交差検証
tags: Python アイリスの分類
---

モデルの評価と改良

### 交差検証（Cross validation）

irisをロジスティック回帰で分類する場合

```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression

iris = load_iris()
X, y = iris.data, iris.target
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)

clf = LogisticRegression(solver='liblinear')

clf.fit(X_train, y_train)
score = clf.score(X_test, y_test)
print("Test set score:", score)
```


```python
from sklearn.model_selection import cross_val_score

clf = LogisticRegression(solver='liblinear')

scores = cross_val_score(clf, X, y)
print("scores", scores, "Average score:", scores.mean())
#->
scores [1. 0.96666667 0.93333333 0.9 1.] Average score: 0.9600000000000002
```
