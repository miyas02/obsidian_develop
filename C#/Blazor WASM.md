# webアプリ実行
ホットリロードでサーバー実行
```bash
dotnet watch run
```
# 各ファイルの機能・役割
## wwwroot
Webサイトの公開ルート
ビルドプロセスを通さず、そのままブラウザへ配信される
のフォルダ内のパスが、そのままWebサイトのURLパスになる

**静的アセット **: css, javascript, アイコン, 設定ファイルなど

**アプリのエンドポイント** : index.html

## Layout
**MainLayout.razorアプリ全体の親テンプレート。**
**NavMenu.razorサイドナビゲーション。**
**MainLayout.razor.css**`MainLayout` 専用のスタイル。
### MainLayout.razor の仕組み
`**@Body**`** の役割:** ブラウザのURLに応じて切り替わる「各ページの中身」が、この場所に流し込まれます。
### 複数のレイアウトを使い分ける
LoginLayoutなど他のLayoutのrazorファイルを作成し、各ページで@layoutで指定
```c#
@page "/login"
@layout LoginLayout
```
## Pages
ユーザーがブラウザでアクセスする各画面（ページ）を格納する場所です。
このフォルダ内の `.razor` ファイルは、冒頭に `**@page "/パス"**` というルーティング設定を持つ
```c#
@page "/weather"
```
## Program.cs
アプリが実行されるときに**一番最初に読み込まれる**ファイル
アプリの初期設定と**依存関係（DI**）の登録を行う
## App.razor
「今どのURLが叩かれたか？」を監視し、表示するページを振り分ける役割
***
# Razorコンポーネント
以下3つの要素
- **ディレクティブ**
	- **@using**でimportするファイルを定義
	- **@page**でURLを定義。@page "/"でデフォルトページ指定
- **HTML/Razor構文(UI)**
	- UI部分
- **codeブロック(ロジック)**
	- ViewModelを記述
```C#
@* 1. ディレクティブ（設定） *@
@using App.Models

@* 2. HTML / Razor構文（見た目） *@
<div class="role-card">
    <div class="name-plate">@PlayerName</div>
</div>

@* 3. @codeブロック（ロジック） *@
@code {
    // 親から渡されるデータ（Parameter）
    [Parameter] public int PowerValue { get; set; } = 10000;    
    private void ExecuteAction()
    {
        Console.WriteLine($"{PlayerName} が能力を使いました");
    }
}
```
## Razorコンポーネントを組み込む
@usingで指定したnamespace内のrazorを組み込む

```C#
<RazorComponent parameter1 = 1> @* 別ファイルのRazorComponentを組み込む *@
```
