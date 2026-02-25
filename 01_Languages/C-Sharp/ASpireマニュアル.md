# 概要
分散アプリケーション管理用フレームワーク。
app本体,DB,APIなどの接続設定をする。
長所はブラウザの管理画面が使いやすい。
# 要点
- **AppHost.cs**
	最初に実行される。サービスを登録したりする。
- **LaunchSettings.json**
	起動構成ファイル。
- **AspireApp.ServiceDefaults**
	共有ライブラリ
# 作り方
### Visual Studioで作成
Visual Studioの`Aspireスターターテンプレート`で作成
### CLIで作成
#### ASpireCLIのインストール
- powershellで以下コマンドを実行
`irm https://aspire.dev/install.ps1 | iex`
- cmdで以下コマンドを実行し、aspireがインストールされていることを確認
`aspire --version`
- vscodeでASpire拡張をインストール
- vscodeコマンドパレットを開く`ctrl Shift P`
- `Aspire: New Aspire project`を選択
- このみのテンプレートを選択
# ディレクトリ構造
- **AspireApp.AppHost**
	Aspireプロジェクト
- **AspireApp.ApiServer**
	サンプルのバックエンドAPIプロジェクト
- **AspireApp.Web**
	webApp本体
- **AspireApp.ServiceDefaults**
	共有ライブラリ
	マイクロサービス開発では、すべてのサービスで「ログの出力設定」「リトライ処理」「生存確認（ヘルスチェック）」などを共通化する必要があります。これらを各プロジェクトにコピペするのは非効率なため、Aspire ではこのフォルダに集約しています。
## ファイル
- **AppHost.cs**
`Program.cs`と同じ。最初に実行されるmainファイル。
```cs
// builderの作成
var builder = DistributedApplication.CreateBuilder(args);

// aspireにサンプルのバックエンドAPIプロジェクトを追加
var server = builder.AddProject<Projects.sample_AspireApp_Server>("server")
    //ヘルスチェック（生存確認）用エンドポイントとして `/health` を指定
    .WithHttpHealthCheck("/health")
    //HTTPエンドポイントを外部からアクセス可能にする設定
    .WithExternalHttpEndpoints();

var webfrontend = builder.AddViteApp("webfrontend", "../frontend")
    .WithReference(server)
    .WaitFor(server);

//publish設定（デプロイ用パッケージ化）
//frontendフォルダをserverのwwwrootフォルダに含める設定
server.PublishWithContainerFiles(webfrontend, "wwwroot");

builder.Build().Run();
```
- **Properties/LaunchSettings.json**
プロジェクトを起動する際の設定（プロファイル）
`Aspireダッシュボード`をどう起動するか
起動構成ファイル
