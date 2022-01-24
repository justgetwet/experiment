<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
<script type="text/x-mathjax-config">
 MathJax.Hub.Config({
 tex2jax: {
 inlineMath: [['$', '$'] ],
 displayMath: [ ['$$','$$'], ["\\[","\\]"] ]
 }
 });
</script>

## 最小2乗法による分類

<!-- ありえない事態が起こったなら疑うべきは２つだけだ
前提条件が間違っているか、それともあんたの頭がいかれちまったか
狡噛慎也 -->


### pl

```shell
conda install plotly
conda install -c conda-forge python-kaleido
conda install -c anaconda psutil
```

#### Atom Markdown-preview-plus

### 回帰分析

[Chiner Tutorial 7. Regression](https://tutorials.chainer.org/ja/07_Regression_Analysis.html)

### 最尤推定

MLE(Maximum Likelihood Estimation)

観測値 $x = x_{1}\,...x_{N}$ について結合確率関数 $f(x \mid w)$ を$w$の関数とみなしたものを尤度関数(Likelihood function)といい、$L(w \mid x)$ で表す。

$x = x_{1}\,...x_{N}$ の標本の場合、

$$ L(w \mid x) = f(x \mid w) = \prod^N_{i=1} f(x_{i}|w) $$

フィッシャーは統計学的推計の基礎として事後確率を用いることに反対し、代わりに尤度関数に基づく推計を提案している。




### ロジスティック回帰

$$ y = w_{0}x_{0} + w_{1}x_{1} +\, ... + w_{m}x_{m} $$

$$ y = w^{T}x $$

$$ \sigma(t) = \frac{1}{1-\exp(-t)} $$

$$ \hat{p} = \sigma(w^{T}x) $$
