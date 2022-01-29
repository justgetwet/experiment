---
layout: post
title: 標準化
tags: Python 線形回帰
---

平均0、分散1に正規化（標準化）する。

$$ x' = \frac{x - \bar x}{\sigma} $$

```python 
import pandas as pd
from sklearn.datasets import load_boston

boston = load_boston()
X = boston.data[:,:6]
col = boston.feature_names[:6]

df = pd.DataFrame(X, columns=col)
print(df.head())
# -> \
      CRIM    ZN  INDUS  CHAS    NOX     RM
0  0.00632  18.0   2.31   0.0  0.538  6.575
1  0.02731   0.0   7.07   0.0  0.469  6.421
2  0.02729   0.0   7.07   0.0  0.469  7.185
3  0.03237   0.0   2.18   0.0  0.458  6.998
4  0.06905   0.0   2.18   0.0  0.458  7.147
```

```python
def standardization(x, axis=0):
    x_mean = x.mean(axis=axis, keepdims=True)
    x_std = x.std(axis=axis, keepdims=True)
    return (x - x_mean) / x_std

std_X = standardization(X)
df = pd.DataFrame(std_X, columns=col)
print(df.head())
# ->
       CRIM        ZN     INDUS      CHAS       NOX        RM
0 -0.419782  0.284830 -1.287909 -0.272599 -0.144217  0.413672
1 -0.417339 -0.487722 -0.593381 -0.272599 -0.740262  0.194274
2 -0.417342 -0.487722 -0.593381 -0.272599 -0.740262  1.282714
3 -0.416750 -0.487722 -1.306878 -0.272599 -0.835284  1.016303
4 -0.412482 -0.487722 -1.306878 -0.272599 -0.835284  1.228577
```

```python
from sklearn.preprocessing import StandardScaler

sc = StandardScaler()
X_std = sc.fit_transform(X)
df = pd.DataFrame(std_X, columns=col)
print(df.head())
# ->
       CRIM        ZN     INDUS      CHAS       NOX        RM
0 -0.419782  0.284830 -1.287909 -0.272599 -0.144217  0.413672
1 -0.417339 -0.487722 -0.593381 -0.272599 -0.740262  0.194274
2 -0.417342 -0.487722 -0.593381 -0.272599 -0.740262  1.282714
3 -0.416750 -0.487722 -1.306878 -0.272599 -0.835284  1.016303
4 -0.412482 -0.487722 -1.306878 -0.272599 -0.835284  1.228577
```
