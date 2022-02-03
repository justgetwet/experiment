---
layout: post
title: 交差検証
tags: Python アイリスの分類
---

分類器の汎化性能を、交差検証と呼ばれる方法で測定する。

### 交差検証（Cross validation）

irisをロジスティック回帰で分類する場合を考える。training setでモデルの学習、test setでモデルの評価を行う。

```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression

iris = load_iris()
X, y = iris.data, iris.target
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0, stratify=y)

clf = LogisticRegression(solver='liblinear')

clf.fit(X_train, y_train)
score = clf.score(X_test, y_test)
print("Test set score:", score)
```

このスコアははじめに分割したtraining setに依存している。本来は未知のデータに対する予測精度、つまり汎化精度を高めることが目的なので交差検証を行う。

交差検証では、data setを $k$ 個に分割し、モデルの学習と評価を $k$ 回行い、得られた $k$ 個の評価値の平均をとった値を最終的なスコアと扱う。

1. データを $k$ 個のブロックに分ける。これを fold という。
2. 最初の分割1をtest set、残りをtraining setとし、モデルの学習と評価を行う。
3. 分割2をtest set、残りをtraining setとし、モデルの学習と評価を行う。
4. この過程を、分割3,4,5をtest setとして繰り返す。
5. 得られた $k$ 個のスコアの平均値をモデルの評価値とする。

Sikit-learnでは、上記の流れを `model_selection.cross_val_score()` で実行できる。

```python
from sklearn.model_selection import cross_val_score

clf = LogisticRegression(solver='liblinear')

scores = cross_val_score(clf, X, y)
print("scores", scores, "Average score:", scores.mean())
#->
scores [1. 0.96666667 0.93333333 0.9 1.] Average score: 0.9600000000000002
```

#### データの分割方法

単純な方法では、データの最初から $1/k$ ずつ分割。

層化 $k$ 分割交差検証(Stratified $k$ -fold cross-validation)では、各分割内のクラスの比率が全体の比率と同じになるように分割する。

`cross_val_score()`はパラメーター `cv` を用いて分割方法を指定できる。

```python
from sklearn.model_selection import KFold
from sklearn.model_selection import StratifiedKFold
# 単純な方法
kfold = KFold(n_splits=3)
print(cross_val_score(clf, X, y, cv=kfold))
# 層化 k 分割交差検証
stratifiedkfold = StratifiedKFold(n_splits=3)
print(cross_val_score(clf, X, y, cv=stratifiedkfold))
# ->
[0. 0. 0.]
[0.96 0.96 0.94]
```

一般的には，回帰には単純な kk 分割交差検証，クラス分類には層化 kk 分割交差検証が用いられる。`cross_val_score()` のパラメータ `cv` に何も指定しない場合はこの選択基準で分割方法が選択される。
