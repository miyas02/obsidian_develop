# 文章問題
## 〇
- Pythonはソースファイルの最終更新日時をコンパイル済みのバージョンと比較し、再コンパイルが必要か判断する。これは完全に自動的に行われる。
- コンパイル済みのモジュールはプラットフォーム非依存なので、ひとつのライブラリを異なるアーキテクチャのシステム間で共有できる。
- モジュール
- 実行中のスクリプトのあるディレクトリは、検索パスの最初、標準ライブラリのパスよりも前方に置かれる。
- 仮想環境を作成、管理するのに使われるスクリプトはpyvenvである。
- PEP8では、クラスや関数には一貫した命名を行うべきであり、クラスには「CamelCase」を、関数やメソッドには「lower_case_with_underscores」を使うべきとされている。
- データ型の一般性が高いため、Pythonの対応可能な問題領域はPerlと比べて広い。
- 比較演算子inおよびnot inは、シーケンスに値が存在するかチェックする。演算子isおよびis notは、2つのオブジェクトを比較して完全に同一であるかを調べる。
- モジュールがインポートされるとき、インタープリタは「ビルトインモジュール → sys.path変数で得られるディレクトリのリスト」の順に検索する。
- モジュールの読み込みを高速化するために、Pythonはコンパイル済みのモジュールを__pycache__ディレクトリに「module.バージョン名.pyc」の名前でキャッシュする。
## ×
- 例外
「パーサは違反のある行を表示し、最初にエラーが検知された点に**下線が引かれる**。エラーは**矢印より前**のトークンが原因である。」
→誤り1：下線 → 正しくは小さな矢印（`^`）で示される
→**誤り2：「矢印より前」と「下線」が混在していて矛盾している**
正しくは「**小さな矢印で示され、エラーは矢印より前のトークンが原因**」です。
- コメント行は独立させず、該当コードについての説明であることが明示されるよう、同じ行に記述すべきである。
	コメントは行を独立させて記述するべき
- 
# memo
- 大文字と小文字は**小文字のほうが大きい**ので判定はTRUEになる
`'PHP' < 'Perl' < 'Python'`
- 対話モードでは、最後に表示した式を変数`_`（アンダースコア）に代入してある。
```python
>>> 1 + 2
3
>>> _
3 # 直前の結果が_に入っている 
```
対話モード
引用符あり
`\n`は`\n`のまま表示
`\t`は`\t`のまま表示
`print()`
引用符なし
`\n`は改行として出力
`\t`はタブとして出力

辞書
`enumerate()` : インデックスとキーをを取得
`items()` : キーと値を取得

`^`対称差どちらか一方だけに含まれる

`dict()` のキーワード引数には**文字列リテラルは使えません**。識別子（変数名）でなければならないので `"carrot"=80` は `SyntaxError` になります。
```python
# ❌ 文字列リテラルはキーワード引数に使えない
dict("carrot"=80)   # SyntaxError

# ✅ 識別子なら使える
dict(carrot=80, tomato=100)  # → {'carrot': 80, 'tomato': 100}
```

**ディクショナリの作り方まとめ**
```python
# 辞書リテラル
{"carrot": 80, "tomato": 100}

# dict()にキーワード引数（識別子のみ）
dict(carrot=80, tomato=100)

# タプルのリストから
dict([("carrot", 80), ("tomato", 100)])

# 辞書内包表記
{k: v for k, v in [("carrot", 80), ("tomato", 100)]}
```

```python
todo_app/ 
	__init__.py 
	model/ 
		__init__.py 
		base.p

# baseをインポート
import todo_app.model.base
#または
from todo_app.model import base # ✅ fromを使う
```

NameError
定義していない変数を使用
`print(python_version) ` 

`except as`
`raise from`
```python
except ZeroDivisionError as e: # except as
	raise ValueError("Illegal argument") from e # raise from
```
osモジュール
```python
help(os) # osモジュールのドキュメントを表示
dir(os) # osモジュールを一覧で表示
os.getpwd # 現在ディレクトリ
os.chdir(dirname)
```
sqllite3モジュール
`import sqlite3`

`logging` のデフォルトのログレベルは **`WARNING`** なので、WARNING以上のレベルだけ出力されます。
```python
import logging logging.warning("Key not found") # WARNING → 出力 ✅ 
logging.critical("Key not found") # CRITICAL → 出力 ✅ 
logging.debug("Key not found") # DEBUG → 出力しない ❌ 
logging.error("Key not found") # ERROR → 出力 ✅ 
logging.info("Key not found") # INFO → 出力しない ❌
```

`pip install -r requirements.txt`
`requirements.txt` に書かれたパッケージを**まとめてインストールするコマンド**
- オプションの傾向

| 形式      | 文字数  | 例                            |
| ------- | ---- | ---------------------------- |
| `-` 1つ  | 1文字  | `-r`, `-m`, `-c`             |
| `--` 2つ | 複数文字 | `--upgrade`, `--requirement` |
デフォルト引数：関数を定義するときに使う
キーワード引数：関数を呼び出すときに使う

**`yield` とは**
`return` に似ていますが、**関数を終了させずに値を1つずつ返す**キーワードです。`yield` を使った関数は**ジェネレータ関数**になります。

`testmod` : docstring に書いたテストを自動実行する機能
```python
import doctest

def add(a, b):
    """
    2つの数を足す関数

    >>> add(1, 2) # テストコード
    3
    >>> add(0, 0)
    0
    >>> add(-1, 1)
    0
    """
    return a + b

doctest.testmod() # 実行
```

デフォルト以外のエンコーディングを使う場合は、ソースコードの最初の行に「# -*- coding: encoding -*-」という特殊コメント行を追加します。

 float型を含む計算は常にfloat型を返す。
 
`print(i, end=',')`のように、endのキーワード引数を使うと、print文の末尾の改行を抑制し、改行を他の文字に変更することができます。

- try else
	tryで例外が発生しなかったら、elseが実行
```python
try: requests.get(url) 
	print('success') 
except Exception: 
	print('error') 
else: break
```


- 引数に`*`を付けると、**可変長のタプル**として渡されます。
`def job(*, kwarg):`
引数の`*, 引数1`でのちの引数をキーワード引数に指定（位置引数不可）（アンパックとは関係なし）
`引数1, /`で前の引数を位置引数で指定（キーワード引数不可）
```
def func(a, /, b, *, c): # aには位置引数のみ、cにはキーワード引数のみ、bはどちらも可
```

- lambdaの引数と式の変数は同じにする
× `even = filter(lambda x: y % 2 == 0, num)`
〇 `even = filter(lambda y: y % 2 == 0, num)`

インタープリタでドキュメンテーションを参照する場合は、help()関数を使う。

デフォルト値付き引数のアノテーションは以下の記述で表す。
```python
def job(kwarg: str = 'kwarg'):
    pass
```

```
collections.dequeは先頭と末尾の要素をピンポイントで取得するため、処理が高速です。
```

```python
json = json.dumps({'one': 1, 'two': 2})

print(type(json)) # str
```

シーケンスとは**順序を持つデータの集まり**のこと

# 覚えるリスト
for-else文
`else` ブロックはループが `break` されずに正常終了したときだけ実行されます。
- デフォルト引数
	- listとかsetとか辞書は１回だけ生成されて、そのlistに値を追加する処理になる
	- デフォルト引数に変数を定義すると、デフォルト引数の関数定義時点の変数が代入される。
	- デフォルト引数に値を渡せば、位置引数と同じ処理になる
- docstring
	- 関数内に記述する説明文です。 `関数名.__doc__` でアクセス可能
- リスト
	- `remove(x)`最初に見つかった値 `x` を削除
-  ラムダ式
`[lamd 引数: 式]`という書き方をし、式の結果が返り値になる。
`lambda a: a**3 # aを受け取って a**3 を返す無名関数`
`cube = lambda a: a**3 # 変数に代入`
- あるモジュールがインポートされるときにインタープリタが検索する順序
`import モジュール名`と記述したときに、Pythonがどこを探すかの順番
1. ビルトインモジュール
	Python本体に組み込まれているもの(sys, os, mathなど)
2. `sys.path` で得られるディレクトリ（入力スクリプトのディレクトリ、PYTHONPATH、インストールデフォルト）
シンボリックリンクを置いてあるディレクトリ」は検索順序に含まれません。（リンク先の実体があるディレクトリを探す）
- sys.path
`sys.path` はPythonが**モジュールを探しに行くディレクトリのリスト**です。
 sys.pathが初期化されている場所は、入力スクリプトのあるディレクトリ、PYTHONPATHであり、**インストールごとのデフォルトも含まれる
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
###  ライブラリ
[[python3基礎試験]] ライブラリを覚える