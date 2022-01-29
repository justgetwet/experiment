---
layout: post
title: 回帰モデルの評価
tags: Python 線型回帰
---

kjjk

### 決定係数 R2

R2は、観測値（教師データ）$t$ と予測値 $y$ の相関係数で、0～1の値を取る。

$$ R2 = 1 - \frac{\Sigma_{n}(t_{n}-y_{n})^2}{\Sigma_{n}(y_{n}-\bar y)^2} $$

#### 自由度調整済み決定係数 R*2

nはデータの個数、pは特徴量変数の個数。特徴量変数を増やすと必然的に決定係数が増加するため、その影響を調整した指標。

$$ R^{*}2 = 1 - \frac{\frac{\Sigma_{n}(t_{n}-y_{n})^2}{n-p-1}}{\frac{\Sigma_{n}(y_{n}-\bar y)^2}{n-1}} $$

### RMSEとMAE

Root Mean Squared Error (RMSE)

$$ RMSE^2 = \frac{\Sigma_{n}(t_{n}-y_{n})^2}{n} $$

Mean Absolute Error (MAE)

$$ MAE^2 = \frac{(\Sigma_{n}(t_{n}-y_{n}))^2}{n^2} $$

誤差平均が0、標準偏差 $\sigma$ の正規分布に従うモデル、正規分布に従うようなノイズのみが誤差として残る場合、MAE に対する RMSE の比は、1.253近くになる。

$$ \frac{RMSE}{MAE} = \sqrt{1+\frac{(1-\frac{2}{\pi})\sigma^2}{(\sqrt{\frac{2}{\pi}}\sigma)^2}} $$

$$ = \sqrt{\frac{\pi}{2}} \approx 1.253 $$

### Observed-Predicted Plot (yyplot)

yyplot は、横軸に実測値(yobs)、縦軸に予測値(ypred)をプロットしたもの。プロットが対角線付近に多く存在すれば良い予測が行われていると見る。

### 残差プロット

# 残差のプロット
ax.scatter(y_pred, y_pred - y_test, marker = 'o')
# y = 0 の赤い直線をプロット
ax.hlines(y = 0, xmin = -10, xmax = 50, linewidth = 2, color = 'red')

ax.set_xlabel('y_pred')
ax.set_ylabel('y_pred - y_test')
ax.set_title('Residual Plot')
