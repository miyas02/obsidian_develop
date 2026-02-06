[演習シート]([Python学習記録の統合と分析 - Google スプレッドシート](https://docs.google.com/spreadsheets/d/1TVqeoB6elm-hZ8dEvFaTczCtUHQNkrDTrNg9xdGkC_M/edit?gid=1811912241#gid=1811912241))

# 数の操作
算術演算子はべき乗,乗算/除算、加算/減算の順
/を使うと結果は浮動小数点になる
`5**2は5の2乗で25`
文字列スライス`slice = word[2:5]`、文字の間を指定している
`f{値:フォーマット指定子}`指定子7dは7桁10進数出力。右寄せ7桁幅で表示される
「  15000」
フォーマット指定子
b 2進数
d 10進数
x 16進数
f .5fで小数第5位まで丸める
% 100倍してパーセント記号を表示
formatメソッド
文字列の詳細なフォーマットを行う
formatメソッドに与えた引数の変数名は直接記述できない。
.format(x, y, z) x,y,zを{}に記述できない。
リストのスライス [開始位置:終了位置]
# コーディングスタイル
インデントはタブではなく空白４つが推奨

## 条件と繰り返し
整数の評価は0が偽、それ以外が真
range関数
range(start, stop, step)
stopの直前まで
## 関数
位置引数のあとにキーワード引数
引数のデフォルト値は関数が手議された時点で評価される
引数のデフォルト値のリストは、値が加わるたびに評価される
アンパック リストとタプルの先頭に*をつけて関数に渡す
```
def func(*args):

words = {"spam","ham"}
func(*words) #アンパック
```
docstring
func.__doc__
## コレクション操作
### タプル
要素の追加できない
```
sample_tuple = ("spam","ham")
sample_tuple = "spam","ham"
sample_tuple = "spam",
sample_tuple = tuple(["spam","ham"]) #tuple()は引数にlist
```
### ディクショナリ
`del dic['key'] #削除`
内包表記
`{キーの式:値の式 for 変数　in 反復可能体} # 値を式に
## モジュール
モジュールに含まれる関数のみをインポートするにはfromを使用する
`from モジュール名 import 関数名`
importでモジュールをまるごと持ってくるが、`モジュール名.関数名`で記述する必要がある。
`import *`では_からはじまる名前はインポートできない
`__name__` モジュール名が自動で代入される
`__init__.py`
importすると読み込まれる
相対パスでインポートを記述する
## ファイル入出力
open関数
r 読み込み専用
r+ 読み書き療養
w 新規書き込み
a 追加書き込み
b バイナリモード
ファイルオブジェクトはリストと同じ反復可能体
json.dump( ) dictなどをファイル書き込み
json.dumps dictなどをjson形式の文字列に変換して返す
json.load( ) json形式をオブジェクトに変換（デシリアライズ）
## 例外
raise throwのかわり
# クラス
関数のなかでクラス変数にアクセスするには、`self.filedA`と記述
継承は `class Duck(Bird):`
オブジェクトの型判定`type( )`
