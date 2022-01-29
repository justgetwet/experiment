---
layout: post
title: Redge回帰
tags: Python 線型回帰
---

virginica

```python
conda install -c districtdatalabs yellowbrick
```

alpha value 0.01 - 10 の間?

```python
print(np.logspace(-2, 1, 20))
#->
[ 0.01        0.0143845   0.02069138  0.02976351  0.04281332  0.06158482
  0.08858668  0.1274275   0.18329807  0.26366509  0.37926902  0.54555948
  0.78475997  1.12883789  1.62377674  2.33572147  3.35981829  4.83293024
  6.95192796 10.        ]
```

Rigde回帰のalpha値

```python
import matplotlib.pyplot as plt
from yellowbrick.regressor import AlphaSelection

alphas = np.logspace(-2, 1, 500)
ridgeCV = RidgeCV(alphas=alphas)
visualizer = AlphaSelection(ridgeCV)
visualizer.fit(X_train, y_train)
visualizer.show()
```

![alphas]({{site.baseurl}}/assets/images/yellowbrick_alphas.jpg)

```python
# visualizer.show()
visualizer.finalize()
visualizer.ax.set_xlabel("alpha")
visualizer.ax.set_ylabel("error")
plt.savefig("alphas.jpg")
```
