---
layout: post
title: ハイパーパラメータの調整
tags: Python アイリスの分類
excerpt_separator: <!--more-->
---

グリッドリサーと呼ばれる方法で、分類器のハイパーパラメータを調整し、汎化性能を向上させる。

<!--more-->

Scikit-learnのSVC()には、正規化パラメーター $C$ とカーネルのバンド幅 $γ$ の２つのパラメータが存在する。それぞれ 0.001, 0.01, 0.1, 1, 10, 100 を試してみると 6 x 6 = 36通りの組み合わせがある。グリッドリサーチでは、この36通り全てについてモデルの学習と評価を行う。

### 単純なグリッドリサーチ

単純な方法は，gamma と C それぞれについて for ループを用いて実装することができる。

```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.svm import SVC

X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)

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
# output
0.9736842105263158 {'C': 100, 'gamma': 0.001}
```

上記の方法では、パラメータ選択の際に test set を使用しているので、汎化精度を評価するために、training set に対して分割を行い、新たに validation set を作成する必要がある。

```python
X_trainval, X_test, y_trainval, y_test = train_test_split(X, y, random_state=0, stratify=y)
X_train, X_valid, y_train, y_valid = train_test_split(X_trainval, y_trainval, random_state=1)
print(X_train.shape, X_valid.shape, X_test.shape)
# output
(84, 4) (28, 4) (38, 4)
```

validation set を用いたグリッドサーチ

```python
params = [0.001, 0.01, 0.1, 1, 10, 100]

best_score = 0
best_parameters = {}

for gamma in params:
    for C in params:
        svm = SVC(C=C, gamma=gamma)
        svm.fit(X_train, y_train)
        score = svm.score(X_valid, y_valid)
        if score > best_score:
            best_score = score
            best_parameters = {"C": C, "gamma": gamma }

svm = SVC(**best_parameters)
svm.fit(X_trainval, y_trainval)
print("best parameters:", best_parameters)
print("Test set score with best parameters:", svm.score(X_test, y_test))
# output
best parameters: {'C': 100, 'gamma': 0.01}
Test set score with best parameters: 1.0
```

より頑健な汎化性能の見積を行うために、`cross_val_score()`による交差検証を組み合わせる。

```python
for gamma in params:
    for C in params:
        svm = SVC(C=C, gamma=gamma)
        scores = cross_val_score(svm, X_trainval, y_trainval, cv=5)
        score = scores.mean()
        if score > best_score:
            best_score = score
            best_parameters = {"C": C, "gamma": gamma }

svm = SVC(**best_parameters)
svm.fit(X_trainval, y_trainval)
print("best parameters:", best_parameters)
print("Test set score with best parameters:", svm.score(X_test, y_test))
# output
best parameters: {'C': 100, 'gamma': 0.01}
Test set score with best parameters: 1.0
```

上記のコードを、`GridSearchCV` で書き直す。

```python
from sklearn.model_selection import GridSearchCV

# パラメータを dict 型で指定
param_grid = {'C': [0.001, 0.01, 0.1, 1, 10, 100],  'gamma' : [0.001, 0.01, 0.1, 1, 10, 100]}
# validation set は GridSearchCV が自動で作成してくれるため，
# training set と test set の分割のみを実行すればよい
X_train, X_test, y_train, y_test = train_test_split(iris.data, iris.target, random_state=0)

grid_search = GridSearchCV(SVC(), param_grid, cv=5)
# fit 関数を呼ぶことで交差検証とグリッドサーチがどちらも実行される
grid_search.fit(X_train, y_train)
print("best parameters:", grid_search.best_params_)
print("Test set score:", grid_search.score(X_test, y_test))
print("Best cross-validation:", grid_search.best_score_)
# output

```
