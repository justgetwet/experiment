
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

### マルチンゲール法

確率0.5で2倍の配当があると仮定し、負けた場合は倍額をベットする。

ベットの係数は、$ 2^n $。

```
[2**n for n in range(10)]
[1, 2, 4, 8, 16, 32, 64, 128, 256, 512]
```

現実的なGameとして、1ベットを100円、10回目の増分ベット51200円で負けた場合はパンクとして損切をする。

つまり5倍ハマりまでは耐えて、次の負けでGame End

```
from scipy.stats import binom
print(binom.pmf(0, 10, 1/2))
# -> 0.0009765625
```

10回目の試行まで負け続ける、5倍ハマりの確率は、約0.1%、およそ1/1000。

100円のベットを1000試行続けたとすると、理論上の増加は50000円。利益と損失がイーブンになっているように見える。

