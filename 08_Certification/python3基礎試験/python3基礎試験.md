## 1. 数値と演算
### 演算子の優先順位
1. **べき乗**: `**`（例: $5^2$ は `5**2` で $25$）
2. **乗算・除算・剰余**: `*`, `/`, `//` (切捨て除算), `%` (余り)
3. **加算・減算**: `+`, `-`
### 除算の性質
- `/` を使うと、計算結果は常に**浮動小数点数（float）**になります。
- 整数のみの結果が欲しい場合は `//` を使用します。結果は切り捨て。
## 短絡演算子
**and**: 最初に偽になった評価結果が最終結果。全部真なら最後の値を返す。
**or**: 最初に真が最終結果
```python
result1 = 0 and 1 and 2 # result1 = 0 最初の偽の0
result2 = 1 and 2 and 3 # result2 = 3 すべて真なので最後の値の3
result3 = 0 or 1 or 2 # result3 = 1 最初の真の1
result4 = 0 or false or "" # result4 = "" すべて偽なので最後の値
```
## 2. 文字列とフォーマット
文字列の操作と、データの表示形式を指定する方法です。
**文字列はイミュータブルなので、インデックスで代入しようとすると `TypeError` になります。**
### 複数行の文字列リテラル
複数行で記述する場合は、( )で囲んで、改行しながら列挙する。
```python
text = (
	"Usage: "
	"-h help"
	"-v version"
)
print(text)
```
トリプルクォート`"""`を使うと、リテラル中の開業が改行文字として反映される。
```python
text = """spam
ham
eggs
"""
print(text)
# 改行される
```
### スライス
- `word[開始位置:終了位置]` のように指定します。インデックスは「文字と文字の間」を指すと考えると理解しやすくなります。
- 例: `slice = word[2:5]` は3文字目から5文字目の直前までを取り出します。
- `word[-5:]`のようにマイナスでも指定できる。
	- マイナスを指定すると末尾からカウントする。
	- スライス位置で-5以降の文字をきりとる。(末尾から5番目を含む文字列を取得する)
- `word[:]` 全体のコピー
- `word[1000:]` 範囲外でも空白を返す
- `word[開始 : 終了 : ステップ]`
- **`word[10] = 'a'`** → `TypeError`。文字列はイミュータブルなので代入不可です。
- **`word['B']`** → `TypeError`。インデックスは整数でなければなりません。
- `for month in months[:]:` monthsのコピーを生成してforを回している
### フォーマット指定子（f-string / formatメソッド）
`f"{値:フォーマット指定子}"` の形式で記述します。
`:` **区切り文字**

| **指定子** | **内容**    | **例 (x=15)**       | **出力結果**       |
| ------- | --------- | ------------------ | -------------- |
| `d`     | 10進数      | `f"{x:7d}"`        | `15` (7桁幅・右寄せ) |
| `b`     | 2進数       | `f"{x:b}"`         | `1111`         |
| `x`     | 16進数      | `f"{x:x}"`         | `f`            |
| `.Nf`   | 小数点第N位まで  | `f"{3.14159:.2f}"` | `3.14`         |
| `%`     | 100倍して%表示 | `f"{0.15:%}"`      | `15.000000%`   |
- **formatメソッド**: `.format(x, y)` を使う場合、`{}` 内に直接変数名を記述することはできず、引数のインデックスやキーワード引数名が必要になります。
## コーディングスタイル
### コード
- 国際的な環境で使用する予定のコードでは、PythonのデフォルトであるUTF-8か、さらにプレーンなASCIIが常に最良である。
- docstringの1行目は、常にオブジェクトの目的の短く簡潔な要約を記述し、大文字で始まりピリオドで終わる行とすべきである。
- docstringに2行目以降がある場合、2行目は空行としてようやくと他の記述を視覚的に分離すべきである。
- PEP 8では、演算子の周囲やカンマの後ろにはスペースを入れるが、カッコのすぐ内側にはスペースを入れるべきではないとされる。
### インデント
インデントは空白４つが推奨で、タブは使用しないこと
### 文字数
ソースコードの幅は、79文字を超えないように折り返すこと
### アノテーション
関数の引数の型と戻り値の型を記述できる。強制力はない。
アノテーションは `__annotations__` に辞書として格納され、実行時の動作には影響しません（型チェックは行われない）。
`__annotations__` に辞書として格納される
```python
def func(a: int, b: str) -> bool: 
          # ↑↑↑ ↑↑↑          ↑↑↑↑ 
          # 引数の型 引数の型 戻り値の型
  return True
```
## 3. 判定と繰り返し
### 文字列と数値の比較
```python
'David' != 0 # → True （文字列と整数は型が違うので等しくない）
'David' == 0 # → False 
'David' == 'David' # → True
```
### 真偽値の評価
- 整数の `0` は **偽（False）**、それ以外（`1`, `-5` など）は **真（True）** と判定されます。
### タプルの評価
- `(1, 2, 3, 4) > (1, 2, 5)` 
タプルは先頭から順番に比較します。`1==1`、`2==2`、次は3と5を比較→ `3 < 5` なので `False`
`(1, 2) > (1, 2)`はすべての評価が等しいのでfalse
- `1 > -1 == (1-2)` 
`(1 > -1) and (-1 == -1)` と評価されます。
- `(1, 2) > (1, 2, -1)`
`1==1、2==2` → 左側の要素が尽きる → 短い方が小さい → False
### range関数
- `range(start, stop, step)`: `stop` に指定した値の**直前**までを生成します。
- `range(5)` と書くと `0, 1, 2, 3, 4` が生成されます。
- **`for i in range(Zen[0:5]):`** → `range()` にリストは渡せないので `TypeError`(rangeには数字)
### emurate関数
- リストや文字列などの反復可能たいからインデックスと要素を同時に取得できる。
`for i, elem in enumerate(iterable):`
i : 0始まりの通し番号
elem : 反復可能たいの要素
iterable : 任意の反復可能体
- if文でインデックスの要素を取得
```python
for i, c in enumerate("WORD"):
    if i == 2:
        print(c)
```
### for-else文
`else` ブロックはループが `break` されずに正常終了したときだけ実行されます。
```python
# リストの中に偶数があるか探す
numbers = [1, 3, 5, 7, 9]
for n in numbers:
    if n % 2 == 0:
        print(f"偶数が見つかった: {n}")
        break
else: # breakされずにループが終わったとき実行される
    print("偶数は見つからなかった")
```
## 4. 関数
再利用可能なコードブロックの定義方法です。
### スコープ
関数の外側で定義されたグローバル変数への代入は、`global`文を使う
関数内で定義されている変数は、関数の外側では参照できません。
関数の内部では、関数の外側で定義したグローバル変数には値を直接代入できない。
#### nonlocal/global
globalとnonlocalは予約語
globalはファイル全体（クラスの外も含む）がスコープ
nonlocalはひとつ外
```python
def do_nonlocal(): 
	nonlocal loc # 1つ外側（scope()）のlocを参照 
	loc = "nonlocal" # scope()のlocを書き換える
def do_global(): 
	global loc # グローバルスコープのlocを参照 
	loc = "global" # グローバルのlocを作成・書き換え 
					# scope()のlocには影響しない
```
### 引数のルール
- **順番**: 位置引数を先に書き、その後にキーワード引数を記述します。
#### キーワード引数
```python
def location(city, state='NewYork', country='USA'):

# `city` はキーワード引数で渡しているので順番が逆でも問題なく、`country` は省略してデフォルト値 `'USA'` が使われます。
location(state='Jakarta', city='Cikini')

# zipcodeという引数は定義されていないからエラー
location(city='chiyoda', state='Tokyo', zipcode='1000004')

# キーワード引数の後に位置引数を置けないので `SyntaxError`
location(state='California', country='USA', 'San Francisco')

# `city` に位置引数 `'Geelong'` とキーワード引数 `'Melbourne'` の両方が渡されるので `TypeError`
location('Geelong', city='Melbourne')
```
#### **デフォルト引数の評価**:
- 関数が**定義された時点**で一度だけ評価されます。
- listとかsetとか辞書は１回だけ生成されて、そのlistに値を追加する処理になる
- デフォルト引数に変数を定義すると、デフォルト日キスの関数定義時点の変数が代入される。
- デフォルト引数に値を渡せば、位置引数と同じ処理になる
```python
def greet(name = 'an'): # デフォルト引数
    print(name)
    
def culc(a, b, squares=[], cubes=[]): # デフォルト引数にlistを定義
    squares.append(a ** 2)  
    cubes.append(b ** 3)  
    return squares, cubes  
  
print(culc(2,2))  
print(culc(3,3))  
print(culc(4,4)) #  ([4, 9, 16], [8, 27, 64])  

i = 6  
def f(arg = i): # デフォルト引数に変数を定義 i=6になる
    print(arg)
i = 7
f() # 6が表示
f(8) # デフォルト引数が上書きされて8が表示　
```
#### **`*`がついた仮引数**
- `*`がついた仮引数には、指定済みの実引数を除く位置引数のタプルがはいる。
- `**`がついた仮引数には、指定済みの実引数を除くキーワード引数のディクショナリが入る。
- 順番
- 
`**が先はSyntaxError（*が先でなければならない）`
```python
def f(**arguments, *keywords) # SyntaxError
```
#### **アンパック**: リストやタプルの先頭に `*` をつけると、要素をバラバラにして引数に渡せます。
    ```python
    def func(*args):
        print(args)
    words = ["spam", "ham"]
    func(*words)  # アンパックして渡す
    ```
dictには`**`を先頭に着けると、key,valueでアンパックされる
```python
def concat (arg1, arg2, sep="/"):
	return sep.join([arg1, arg2])
words = ["foo", "bar"]
options = {"sep": "_"} # key=sep,value=_のdict
print(concat(*words, **options) #arg1,arg2にwordsをアンパック、sepにoptionsのアンパック
```
### docstring
- 関数内に記述する説明文です。 `関数名.__doc__` でアクセス可能です。
### 変数のスコープ
ifやfor内の変数も関数内であれば共通。
関数内の変数（ローカル変数)は関数外と別。
## 5. コレクション操作
`a = {} # → 空辞書 <class 'dict'>` 
`b = set() # → 空集合 <class 'set'>`
### リスト
- **pop( )**: リストの最後の要素を取り出し、同時にその要素をリストから削除します。
- `pop(i)`index `i` の要素を削除して返す
- **zip( )**:複数のイテラブルから要素を取り出し、対応する要素をペアとしてまとめる。
- `remove(x)`最初に見つかった値 `x` を削除
```python
# 左は3つ、右は2つ
for i, j in zip([1, 10, 100], [1, 2]):
	print(i * j)
# 結果：
# 1 (1*1)
# 20 (10*2)
# (100 はペアがいないので無視される！エラーにならない)
```
#### リスト内包表記
`[ 式 for 変数 in イテラブル ]`
**リスト内包表記は「新しいリストを作る」構文**なので、`式`の結果が自動的にリストに追加される。
```python
# 元のコード（3行）
cubes = []
for x in range(5):
	cubes.append(x ** 3)
# リスト内包表記（1行） 
cubes = [x ** 3 for x in range(5)]
```
- forを続けて書くとネストする
	`combs = [(a, b) for a in [1,2,3] for b in [3,2,5] if a != b]`
- 戻り値をカンマ区切りで2個記述はNG
	- タプルで受け取る
```python 
[a, b for a in [3,2,1] for b in [6,5]] # NG
[(a, b) for a in [3,2,1] for b in [6,5]] # OK
```
#### リストのスライス
`[開始位置:終了位置`のように記述する。
開始位置と終了位置を省略すると、先頭と末尾になる。
スライス位置は文字列のスライスと同じ。
### タプル (Tuple)
- **特徴**: 要素の追加・変更ができないイミュータブルな配列です。
- **定義方法**:
    - `t = ("a", "b")`
    - `t = "a", "b"` カッコは省略可能
    - `t = "a",` 要素が1つの場合は末尾にカンマが必須
    - `t = tuple(["a", "b"])` リストから変換
	    - 以下はNG
		    - `t = tuple("a","b")` 反復可能体以外を引数にできない
### ディクショナリ (Dictionary)
- **削除**: `del dic['key']`
- キーにできるのは不変体の値のみ
	- tubleは不変なので指定ok
- **内包表記**: `{キーの式: 値の式 for 変数 in 反復可能体}`
- in演算子を使うことでキーの存在を確認できる。
	- `値 in ディクショナリ`
### ラムダ式
`[lamd 引数: 式]`という書き方をし、式の結果が返り値になる。
`map(関数, イテラブル)` : `ange(5)の各要素に lambda a: a**3 を適用する`
```python
list(map(lambda a: a**3, range(5)))
lambda a: a**3 # aを受け取って a**3 を返す無名関数
map(lambda a: a**3, range(5)) # range(5)の各要素に lambda a: a**3 を適用する

# 以下同じ意味
# mapを使う
list(map(lambda a: a**3, range(5))) 
# リスト内包表記
[a**3 for a in range(5)]

```
## 6. モジュールとパッケージ
- **モジュール**
	- pyファイル
- **パッケージ**
	- ディレクトリ
- **サブモジュール**
	- パッケージの`__init__.py`以外のpyファイル
### インポート方法
- `from モジュール import 関数`: 特定の関数のみをインポートし、モジュール名を省略して呼び出せます。
	- 指定した関数のみインポートされモジュール全体はインポートされない。
- `import モジュール`: `モジュール名.関数名()` の形式で呼び出します。
- `from モジュール import *`: `_` で始まる名前（内部用変数）以外のすべてをインポートします。
### 特殊変数・構成
- `__name__`: 実行中のモジュール名が自動代入されます。メインプログラムとして実行されているときは `"__main__"` となります。
- **相対パス**: パッケージ内では `.` や `..` を使って相対パスでインポートを記述できます。
### `__init__.py`
- `__init__.py`: ディレクトリをパッケージとして認識させるためのファイルです。インポート時に実行されます。
#### サブモジュールのインポート方法
`__init__.py`でサブモジュールをインポートするには相対パスで記載する。
### あるモジュールがインポートされるときにインタープリタが検索する順序
`import モジュール名`と記述したときに、Pythonがどこを探すかの順番
1. ビルトインモジュール
	Python本体に組み込まれているもの(sys, os, mathなど)
2. `sys.path` で得られるディレクトリ（入力スクリプトのディレクトリ、PYTHONPATH、インストールデフォルト）
シンボリックリンクを置いてあるディレクトリ」は検索順序に含まれません。（リンク先の実体があるディレクトリを探す）
### sys.path
`sys.path` はPythonが**モジュールを探しに行くディレクトリのリスト**です。
 sys.pathが初期化されている場所は、入力スクリプトのあるディレクトリ、PYTHONPATHであり、**インストールごとのデフォルトも含まれる
#### モジュールが定義している名前を確認
`import sys`
`dir(sys)`
#### sys.pathの初期化場所
`sys.path` の初期化場所は入力スクリプトのディレクトリ・PYTHONPATH・インストールデフォルトの3つです。

### モジュールのキャッシュ
モジュール読み込みの高速化のため、Pythonはコンパイル済みのモジュールを「__pycache__」ディレクトリに、例えば「module.バージョン名.pyc」のような名前でキャッシュする。
### パッケージ
パッケージとは、「ドット区切モジュール名」を使って、Pythonのモジュールを構築する方法である。
`import os.path # os パッケージの path モジュール`
## 7. ファイル入出力
`open(ファイル名, モード)` を使用します。
ファイルオブジェクトは反復可能体なので、`for` 文で1行ずつ処理できます。
ファイルオブジェクトを返す。
### モード一覧
- `r`: 読み込み専用
- `r+`: 読み書き両用
- `w`: 新規書き込み（上書き）
- `a`: 追加書き込み（追記）
- `b`: バイナリモード。単独で使用できない。**使用例**`wb,ab`
### ファイルモジュールのメソッド
`read()` : ファイルの内容を読み込む
`close()` : ファイル処理後に閉じる
`write()` : ファイルの書き込み
### Jsonモジュールのメソッド
`dump()` : pythonオブジェクトをJSONにシリアライズ
`dumps()` : dump + string オブジェクトからJson形式の文字列を生成。引数にファイルオブジェクトはいらない。
`json.load()` : json形式のデータを読み込んで、Pythonのオブジェクトとして取得する。

### JSON操作
- `json.dump(obj, file)`: 辞書などをファイルに書き込み。
- `json.dumps(obj)`: オブジェクトをJSON形式の文字列に変換。
- `json.load(file)`: JSON形式のファイルをPythonオブジェクトに変換（デシリアライズ）。
## 例外処理
- `SyntaxError`
	pythonの文法として正しくない。
- `KeyError`
	ディクショナリに存在しないキーを参照する
- `NameError`
	未定義の変数を使用する
- `KeyboardInterrupt
	`Ctrl+C` でユーザーがプログラムに割り込みをかける
- `raise`: 意図的にエラーを発生させます（他の言語の `throw` に相当）。
### else/finally
`else:` 例外が発生しなかった場合のみ実行 → 今回はスキップ 
`finally:` 例外の有無に関わらず必ず実行 → G が出力される
### キャスト
**ValueError** `int("abc")` : 文字列は受け付けるが、値が数字ではないからValueError
**TypeError** `int([1,2,3])` : リストは型として受け付けていないからTypeError
### argsv
- `except 例外 as v` で例外インスタンスを受け取り、`v.args` に引数が格納されます。
```python
except ValueError as v:
    print(v.args)  # 例外生成時の引数がタプルで入っている
```
### exceptの順番
```python
try: 1 / 0 
except ZeroDivisionError: print('C') # ← マッチしたらここだけ実行
except ValueError: print('D') # ← 上がマッチしたのでスキップ
except: print('E') # ← 上がマッチしたのでスキップ
```
## クラス (Class)
- **インスタンス変数** : `__init(self)__`の中に書く
  クラス内のメソッドでクラス変数やインスタンス変数にアクセスするには `self.変数名` と記述します。
- **クラス変数** : クラス直下に書く。`static`みたいな指定子はいらない。
- **継承**: `class 子クラス(親クラス):` の形式で機能を継承します。
- **型判定**: `type(オブジェクト)` でデータ型を確認できます。
- **インスタンス化** : `duck = Duck()`
### コンストラクタ
`__init__(self)` 引数にselfを必ずつける。
`__init__(self, arg1, arg2)` : 引数を定義することも可能。(selfは必須)
## ライブラリ
### osモジュール
`os.getcwd` : カレントディレクトリを取得
`os.chdir(移動先)` : カレントディレクトリを移動
### glob
ファイルパスの一覧を再帰的に取得
`l = glob.glob('temp/*.txt')`
### sysモジュール
`argv` : コマンドライン引数の取得
**スクリプト名が必ず0番目に入る**
(例)
```python
python3 script.py one two three

sys.argv[0] # 'script.py' ← スクリプト名が必ず0番目に入る
sys.argv[1] # 'one'
sys.argv[2] # 'two'
sys.argv[3] # 'three'
```
### re
正規表現
- `sub( )`
マッチした部分を他の文字列に置換できる。
第一引数に正規表現パターン、第二引数に置換後の文字列、第三引数に処理対象の文字列を指定する。
### statisticsモジュール
`statistics.mean()` : データの平均を求める
`statistics.median()` : データの中央値を求める
`statistics.variance()` : データの不偏分散を求める
### urllib.requestモジュール
`urlopen()` : 引数のurlのwebサイトから情報を取得
### datetimeモジュール
`datetime()` : 日時を表すdatetime型オブジェクトを生成
	`dt.strftime("%Y-%m-%d")` : フォーマット(2000-12-31)
```python
dt1 = date(2001, 1, 1)
dt2 = date(2002, 2, 2)
diff = dt2 - dt1 # 397
```
### unittest
```python
import unittest
class TestApp(unittest.TestCase):
    def test_one(self):
        actual = ... # テスト対象の実行結果
        expected = ... # 期待値
        self.assertEqual(actual, expected)
```
`self.assertEqual( )` : 
### argparse
コマンドライン引数のバリデーションとヘルプ画面の作成を丸投げできるツール
```python
import argparse
# パーサー（解析器）を作る
parser = argparse.ArgumentParser()
# 2. 受け取りたい引数を登録する
parser.add_argument("echo", help="表示したい文字列") # 必須（位置引数
parser.add_argument("-n", "--num", type=int, default=1, help="繰り返す回数") #オプション（非必須)
# 3. 解析を実行する
args = parser.parse_args()
# あとは args.変数名 で中身を使える
for _ in range(args.num): print(args.echo)

# parser.add_argument("body", nargs="+")
# nargs="+" 1個以上の指定 リストで格納される
```
### textwrap
- `fill( )`
任意の文字数に収まるように単語の区切りで分割した文字列を返す。
```python
s = "Python can be easy to pick up whether you're a first time programmer or you're experienced with other languages"
result = textwrap.fill(s, 40) # 40文字で改行された文字列を返す
```
### pprintモジュール
`pprint()` : 1行が80文字を超えると要素ごとに改行
### mathモジュール
`tau`：円数率の2倍
### randomモジュール

| 関数               | 説明               | 例                                  |
| ---------------- | ---------------- | ---------------------------------- |
| `choice(seq)`    | シーケンスから1つランダムに選ぶ | `choice(['a','b','c'])` → `'b'`    |
| `sample(seq, k)` | 重複なしでk個選ぶ        | `sample(range(10), 3)` → `[3,7,5]` |
| `randrange(n)`   | 0以上n未満の整数        | `randrange(5)` → `0〜4`             |
| `randint(a, b)`  | a以上b**以下**の整数    | `randint(1, 6)` → `1〜6`            |
| `random()`       | 0.0以上1.0未満の小数    | `random()` → `0.374...`            |
| `shuffle(seq)`   | リストをその場でシャッフル    | `shuffle([1,2,3])`                 |
## 対話モード
入力履歴のファイル名 : `.python_history`
起動方法：terminalで`python`と入力
	最初にバージョン番号、ヘルプなどのコマンドが表示
一次プロンプト：`>>>`
二次プロンプト：`...`
対話型インタープリタでは文字列は引用符に囲まれ、特殊文字はバックスラッシュでエスケープされた状態で出力される。
print()関数では全体を囲む引用符が除去され、エスケープ文字や特殊文字がプリントされた状態で出力される。
## 仮想環境
### venvモジュール
`python2 -m venv ディレクトリ名`： 仮想環境を作成する
`source ディレクトリ名/bin/activate` 仮想環境を有効化する(Unix)
`ディレクトリ名\Scripts\activate.bat`：仮想環境を有効化する(Windows)
### サードパーティパッケージ
`python -m pip install パッケージ名`：外部パッケージのインストール①
`pip install パッケージ名`：外部パッケージのインストール②
`pip install パッケージ名==バージョン`：外部パッケージのインストール③
### pipコマンド
`pip install パッケージ名`：パッケージインストール
`pip show パッケージ名`：パッケージの詳細情報の表示
`pip list`：インストール済みパッケージ一覧