---

---
### static SSR
[ASP.NET](http://asp.net/) MVCのRazor Pagesに近いモデル。
サーバ側で一度だけレンダリング処理が行われ、それがクライアントにl繰り返されるというもので、ページ表示後の対話型処理(ボタン桜花などのイベントハンドラ処理など）が不要なページで利用できる。
### 対話型WebAssembly (Interactive WebAssmbly)
Blazor WASMと同じ動作モデル。
動作に必要なモジュールすべてブラウザ側に取り込まれて動作する。
初回起動が遅い。
### 対話型Server (Interactive Server)
Blazor Serverと同じ動作モデル。
Razorページのコードブロックをサーバ側で実行し、SignalR回線を利用して描画をブラウザ側で行う。
サーバリソースが必要。
### 対話型Auto (Interactive Auto)
初回ページ呼び出しはServerとして動作しながら、バッググラウンドでWASMモジュールをダウンロードする。
# データバインドの基礎
データバインドには単方向と双方向の2種類ある
単方向はUI要素への書き込みで使う
双方向データバインドはUI要素からのデータの読み取りで使う
## 片方向データバインドによるUIへのデータの表示方法
### 単項目データの表示方法
データを画面に描画するには、Razorコンポーネントと呼ばれる.razorファイルの中に「@」を利用したコードを埋め込む。(Razor構文)
```c#
@using System.Globalization

<p>今日は @DateTime.Now です。</p>
<p>今日は@(DateTime.Now)です。</p> @* スペースを空けたくない場合はカッコで括る *@
<p>今日は @DateTime.Now.ToString("ggyy年MM月dd日(dddd)", dtf) です。</p> @* 和暦表示 *@
<p>指数表記 : @a.ToString("E2")</p>
<p>固定小数点表記 : @b.ToString("F3")</p>
<p>ゼロパディンク : @c.ToString("D8")</p> @* 00001234 *@
<p>16進数表記 : @("&H" + c.ToString("X4"))</p> @* &H04D2 *@
<p>特殊な通貨表記 : @d.ToString("C", nfi)</p> @* 1,234円 *@

@code {
    double a = -123.4567d;
    double b = -987.6543d;
    int c = 1234;
    decimal d = 1234m;
    NumberFormatInfo nfi = new CultureInfo("ja-JP").NumberFormat;
    DateTimeFormatInfo dtf = new CultureInfo("ja-JP").DateTimeFormat;

    protected override void OnInitialized() // ページ初期化処理
    {
        nfi.CurrencyDecimalDigits = 0;                // 小数点以下表示桁数
        nfi.CurrencyDecimalSeparator = ".";           // 小数点区切り子
        nfi.CurrencyGroupSizes = new int[] { 3 };     // 区切り桁数
        nfi.CurrencyGroupSeparator = ",";             // 桁区切り
        nfi.CurrencySymbol = "円";
        nfi.CurrencyPositivePattern = 1;              // 後ろに記号をつける
        dtf.Calendar = new JapaneseCalendar();        // 西暦カレンダを利用する場合は GregorianCalendar
    }
}
```
### 描画の更新タイミング
ボタンを押下した際に画面を書き換えたいとき、**イベントハンドラ**を実装する
イベントハンドラの登録は~button~要素に対して@onclick属性を付与する
イベントハンドラ内で変数currentCountを更新すると、自動的にデータバインド式が再評価され、変更されたUI部分が自動的に差分更新される。

```c++
<p>現在時刻 : @DateTime.Now</p>
<p>カウンター : @currentCount</p>

<button @onclick="btnIncrement_Click">数値増加</button>


@code {
    int currentCount = 0;

    private void btnIncrement_Click()
    {
        currentCount++;
    }
}
```

UI再描画はイベントハンドラの実行をトリガーにする。
イベントハンドラ以外のきっかけでは、UIの再描画が自動で行われないことがある。
`this.StateHasChanged()`を呼び出す
## 双方向データバインドによるUIからのデータ取り込み方法
UI要素とデータ変数を同期させるもの。
ユーザがUI要素に入力した情報を、UI要素に直接アクセスすることなく、データ変数を介してプログラム内で利用することができる。
### テキストボックスからデータを取り込む方法
string変数を定義
`<input>`タグの`@bind`により、双方向データバインドする
`@bind:event=”onipunt”`を指定すると、テキストボックス上で文字を入力するつど、すぐにデータ変数へ入力した内容が羽根井される
```c++
<input type="text" @bind="name1" placeholder="名前を入力" />
<p>@name1</p>

<input type="text" @bind="name2" placeholder="名前を入力" @bind:event="oninput" />
<p>@name2</p>

@code {
    string name1 { get; set; } = "";
    string name2 { get; set; } = "Nobuyuki";
}
```
### ドロップダウンリストからデータを取り込む方法
ドロップダウンリストは「画面に表示するための文字列」と、「処理に利用するデータの文字列」（キー値）を同時に持つことができる。
ドロップダウンに表示する文字列と、処理に利用するキー値をdict型のコレクションとして用意しておき、@foreachを利用してドロップダウンリストを描画する。
`<select>`タグに`@bind`属性を指定して、選択された項目のキー値を文字列型のデータ変数に取り出すことができる。
## イベントハンドラの記述
イベントハンドラ HTML要素に@onclick=関数名
メッセージ取得①のように、引数なしの関数をイベントハンドラとして利用することができる。
メッセージ取得②のように、MouseEventArgsなどの引数wおつけておき、イベント発生時の詳細情報を取得できる。
DB処理などの時間がかかる処理は、メッセージ③のようにイベントハンドラを非同期メソッドの形で記述できる。

```c#
<button @onclick="btnGetMessage1_Click">メッセージ取得①</button>
<button @onclick="btnGetMessage2_Click">メッセージ取得②</button>
<button @onclick="btnGetMessage3_Click">メッセージ取得③</button>

<button @onclick="@(() => Message = DateTime.Now.ToString())">メッセージ取得④</button>
<button @onclick="@(e => Message = DateTime.Now.ToString())">メッセージ取得⑤</button>
<p>メッセージ : @Message</p>

@code
{
    private string Message { get; set; } = "";

    private void btnGetMessage1_Click() {
        Message = "Hello World " + DateTime.Now;
    }
    private void btnGetMessage2_Click(MouseEventArgs e) {
        Message = "Hello World " + DateTime.Now + " " + e.ShiftKey;
    }
    private async Task btnGetMessage3_Click() { 
        await Task.Delay(3000);
        Message = "Hello World " + DateTime.Now;
    }
}
```
### イベントハンドラで受け取れるDOMイベントの引数
MouseEventArgs引数を付与するとマウス操作に関する詳細情報を取得することができる。
ChangeEventArgs引数を紆余すると入力に関する詳細情報が、KeyboardEventArgs引数を付与するとキーボード操作に関する詳細情報が取れます。
# データアクセス
### Entity Framework
マイクロソフトのO/Rマッピングベースのデータアクセス技術。
### O/Rマッピング
OOPとRDBMSの間のデータ変換を行う技術。
# メモリ
## インメモリ
プログラム内の変数やインスタンス
### ライフサイクル
F5で画面更新を行ったら消える
### 用途
- 計算途中の値
- 一時的なフラグ
## LocalStorage
ブラウザが提供する保存領域
KeyValue形式でデータを書き込む
### ライフサイクル
ブラウザを閉じてもPCを再起動しても残る
### 用途
ユーザー設定
ログインの保持
前回の設定