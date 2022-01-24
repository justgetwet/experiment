## ホップ代数の圏

余可換とは限らないホップ代数の圏において、ニューランダー-ニーレンバーグの6次元球面 S6 は、8元数の単位球面における、アラン・コンヌ (Alain Connes) の非可換幾何の意味での「非可換空間」上の自然な概複素構造の直交補空間である。

### Code

Install a Highlighter,  `gem install rouge`

```ruby
class MegaGreeter
  attr_accessor :names
  # Create the object
  def initialize(names = "World")
    @names = names
  end
end
```

### Table

|Syntax|Description|Type|Style|Another Column|
|---|---|---|---|---|
|Header|Title|HTML Element|Simple|More Boring info|
|Paragraph|Text|HTML Element|Simple|And Some More Stuff|

## Juggler シュミレータ

Python で Juggler のシュミレーションを書いてみる。特定の Game 数を設定毎に

#### random.choice()によるシュミレーション

juggler.py

```
import numpy as np

def imJuggler():
    p = np.empty((7, 6))
    p[0] = 273.1, 269.7, 269.7, 259.0, 259.0, 255.0 # big
    p[1] = 439.8, 399.6, 331.0, 315.1, 255.0, 255.0 # reg
    p[2] = 6.02, 6.02, 6.02, 6.02, 6.02, 5.78 # grape
    p[3][:] = 33.3   # cherry
    p[4][:] = 1024.0 # bell
    p[5][:] = 1024.0 # crown
    p[6][:] = 7.3    # replay

    prb = [(*q, 1-q.sum()) for q in 1/p.T]
    pay = 252, 96, 8, 1, 14, 10, 3, 0

    return np.array(prb), np.array(pay)
```

```shell
>>> from juggler import imJuggler
>>> p, coins = imJuggler()
>>> p[0]
array([0.00366166, 0.00227376, 0.16611296, 0.03003003, 0.00097656,
        0.00097656, 0.1369863 , 0.65898216])
>>> coins
array([252, 96, 8, 1, 14, 10, 3, 0])
```

もにょもにょ

```
def simulator(s, game):

  rng = np.random.default_rng()

  p, coins = imJuggler()
  result = rng.choice(coins, game, p=p[s-1])

  payout = result.sum()
  invest = game * 3

  return payout / invest * 100

if __name__ == '__main__':

  game = 10**7
  for s in [1,2,3,4,5,6]:
    rate = simulator(s, game)
    print(s, round(rate, 1))

# 1 97.7
# 2 98.9
# 3 100.8
# 4 102.2
# 5 104.8
# 6 107.1
```


## numpy.random の使い方

### 連続一様分布

numpy の乱数

Return random floats in the half-open intarval [0.0 1.0).

```shell
numpy.random.random_sample() # 0以上1未満の連続一様分布の乱数を返す
numpy.random.random() # 上記の簡略化
numpy.random.rand() # MATLABユーザ用に簡略化

rng = numpy.random.default_rng()
rng.rondom()
```

## 何ゲームハマるのか 

シュミーレーションで当たりを得るため、どのくらいの大きさの試行数を用意すればよいか。

20倍ハマりは5億回に1度くらい、数千万回の試行ではあたることも。

確率質量関数(Probability Mass Function, PMF)と呼ばれる関数を用いて

```
from fractions import Fraction
from scipy.stats import binom

p = 1/99.9
k=0 # 当たりなし
n=100*30 # 30倍ハマり

pmf = binom.pmf(k, n, p)
print(pmf)
# -> 7.805668686310283e-14

x = Fraction(str(pmf))
d = x.denominator/x.numerator
print(d)
# -> 12811202219661.428

i = int(d)
s = str(i)
print(len(s))
# -> 14
```
30倍ハマりは、1.28百兆試行に1回くらい


## 何ゲーム目で当たるのか

```
import numpy as np
# import numpy.random as rng
rng = np.random.default_rng()

def whatnumber(p, size):
  arr = rng.random(size) < p
  idx = np.nonzero(arr)[0]
  if not idx.size:
    print("there is no hit")
    return
  return np.float64(idx[0] + 1)

func = np.frompyfunc(whatnumber, 2, 1)
p = 1/99.9
q = np.ones(10**5)*p
x = func(q, 5000)
m = np.mean(x)
print(m)
# -> 100.24686
```

## GCP Google Sheets for Python

公式の[Python Quickstart](https://developers.google.com/sheets/api/quickstart/python?hl=ja)では、認証まわりをフル稼働させているためか、まず動かない。諦めて[gspread](https://docs.gspread.org/en/latest/)ライブラリを使う。

### 準備編

tips ごうどうを変換 ≡ 縦3点リーダーはコピペ ⋮

#### 1. ライブラリのインストール

googleの認証まわりとそのライブラリはアップデートが激しいので注意が必要。巷でいわれる`oauth2client`では動かなかった。

```shell
pip install gspread google-auth-oauthlib
```

#### 2. GCPでプロジェクトを作成し、APIを有効にする

[GCP](https://console.cloud.google.com/)でプロジェクトを作成

≡ > APIとサービス > ライブラリ: Google Drive API と Google Sheets API を有効にする

≡ > APIとサービス > ダッシュボード: 有効化したAPIを確認する

#### 3. サービスアカウントを作成し、認証情報を取得する

≡ > APIとサービス > 認証情報 + 認証情を作成 > サービスアカウント

 サービスアカウント名を入力 > ロール > 基本 > 編集者 > 完了

右端の 操作 ⋮ > 鍵を管理 > 鍵を追加 > 新しい鍵を作成 > キーのタイプ > JSON > 作成

「秘密鍵がパソコンに保存されました」 > Download Folder

サービスアカウントのメールアドレスをコピーしておく。

#### 4. Google sheetの共有設定

[Google Drive](https://drive.google.com/)でsheetを作成

sheet右上の共有から、ユーザーやグループを追加の欄にメールアドレスを入力し、送信する

#### 5. テスト

```
# sheets_test.py
import gspread
from google.oauth2.service_account import Credentials

# この２つを記述しないとrefresh tokenを3600秒毎に発行し続けなければならない
scope = ['https://www.googleapis.com/auth/spreadsheets','https://www.googleapis.com/auth/drive']

credentials = Credentials.from_service_account_file("token.json", scopes=scope)
gc = gspread.authorize(credentials)

SPREADSHEET_KEY = 'xxxxxxxxxxxxxxxxxxxxxxxx'
workbook = gc.open_by_key(SPREADSHEET_KEY)

# シートの一覧を取得する。（リスト形式）
worksheets = workbook.worksheets()
print(worksheets)
# -> [<Worksheet 'シート1' id:0>]
```

## 継続率

### 海

### s と p

1000円で40game回せるアイム

- 1/127.5
- 割 105.5
- 174枚: 3480円

1000円で20game回せる海

- 1/119.8
- 割 108
- 1450玉: 5800円
- over 0.5

## conda

仮想環境を作成、削除

```shell
$ conda create -n 環境名 python=3.9.9
$ conda remove -n 環境名 --all
```

```shell
# mac
$ source activate 環境名
# windows
$ activate 環境名
# vscode terminal
$ conda activate 環境名
```

venv on Miniconda

```shell
(base) $ mkdir frog
(base) $ cd frog
(base) $ python -m venv .venv
(base $ .venv\Scripts\activate
(.venv) (base) $ python -m pip install --upgrade pip
(.venv) (base) $ pip install scipy
(.venv) (base) $ pip install matplotlib
(.venv) (base) $ pip install gspread
(.venv) (base) $ pip install oauth2client
(.venv) (base) $ pip install flask
```

```shell
(.venv) (base) $ pip freeze > requirements.txt
(.venv) (base) $ mkdir templates static
```

Procfile
```
web: python app.py
```

runtime.txt
```
python-3.9.1
```

app.py
```
from flask import Flask, render_template
import os

app = Flask(__name__)

@app.route('/')
def hello():
    return render_template("index.html", variable=3)

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=int(os.environ.get("PORT", 8888)), debug=True)
```