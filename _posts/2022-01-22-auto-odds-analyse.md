---
layout: post
title: パリミューチュアル方式のベット戦略
tags: Python autorace
---

Parimutuel方式で、控除率が30%のベッティングモデル。

```python
import numpy as np

R = 6
BET = 100
deduction = 0.7
trial = 10**5
payout = 0
races = np.random.randint(1, R+1, size=(trial, BET))
for race in races:
    bet = np.sum(race==1)
    p = bet/BET
    odds = BET*deduction/bet
    win = rng.choice([1, 0], p=[p, 1-p])
    if win:
        payout += odds * 100
invest = trial * 100
print(payout/invest)
# -> 0.7071460578508697
```

割りを110%にするには、控除率の逆数の1.1倍 `1/deduction * 1.1` = 1.57142 を確率またはオッズに掛けること。

```python
>> print(df)
車番 単勝オッズ
 1	  12.0
 2	   1.8
 3	  10.5
 4	  11.9
 5	   3.1
 6	   3.2

>> [1/(df["単勝オッズ"][n]*(1/0.7)) for n in range(len(df))]
[0.058333333333333334,
 0.38888888888888884,
 0.06666666666666667,
 0.058823529411764705,
 0.22580645161290322,
 0.21874999999999997]
```

2番racerの1着推定率が、0.38888888 * 1/0.7 * 1.1 = 0.61111 と見積もることが出来ればベット可能だと考える。
