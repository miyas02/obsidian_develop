# Serilog

C# 向けの構造化ログライブラリ。ファイル・コンソール・DB など多様なシンクへ出力できる。

---

## インストール

```bash
# コアパッケージ
dotnet add package Serilog

# シンク（必要なものを追加）
dotnet add package Serilog.Sinks.Console
dotnet add package Serilog.Sinks.File

# ASP.NET Core 統合
dotnet add package Serilog.AspNetCore
```

---

## 基本設定

### コードによる設定

```csharp
using Serilog;

Log.Logger = new LoggerConfiguration()
    .MinimumLevel.Debug()
    .WriteTo.Console()
    .WriteTo.File("logs/app.log", rollingInterval: RollingInterval.Day)
    .CreateLogger();

Log.Information("アプリ起動");
Log.Warning("警告メッセージ: {Value}", 42);
Log.Error(ex, "エラーが発生しました");

// アプリ終了時にフラッシュ
Log.CloseAndFlush();
```

### appsettings.json による設定

```bash
dotnet add package Serilog.Settings.Configuration
```

```json
{
  "Serilog": {
    "MinimumLevel": {
      "Default": "Information",
      "Override": {
        "Microsoft": "Warning",
        "System": "Warning"
      }
    },
    "WriteTo": [
      { "Name": "Console" },
      {
        "Name": "File",
        "Args": {
          "path": "logs/app.log",
          "rollingInterval": "Day"
        }
      }
    ]
  }
}
```

```csharp
var configuration = new ConfigurationBuilder()
    .AddJsonFile("appsettings.json")
    .Build();

Log.Logger = new LoggerConfiguration()
    .ReadFrom.Configuration(configuration)
    .CreateLogger();
```

---

## ASP.NET Core への統合

```csharp
// Program.cs
using Serilog;

// 起動前のエラー捕捉用ブートストラップロガー
Log.Logger = new LoggerConfiguration()
    .WriteTo.Console()
    .CreateBootstrapLogger();

try
{
    var builder = WebApplication.CreateBuilder(args);

    builder.Host.UseSerilog((context, services, config) =>
        config.ReadFrom.Configuration(context.Configuration)
              .ReadFrom.Services(services)
              .Enrich.FromLogContext());

    var app = builder.Build();

    // HTTPリクエストのログを自動記録
    app.UseSerilogRequestLogging();

    app.Run();
}
catch (Exception ex)
{
    Log.Fatal(ex, "アプリの起動に失敗しました");
}
finally
{
    Log.CloseAndFlush();
}
```

---

## ログレベル

| レベル | 用途 |
|--------|------|
| `Verbose` | 最も詳細。開発時のみ |
| `Debug` | デバッグ情報 |
| `Information` | 通常の動作ログ |
| `Warning` | 異常ではないが注意が必要な事象 |
| `Error` | エラー（処理は継続可能） |
| `Fatal` | 致命的エラー（処理継続不可） |

```csharp
Log.Verbose("詳細: {Detail}", detail);
Log.Debug("デバッグ: {Value}", value);
Log.Information("ユーザー {UserId} がログインしました", userId);
Log.Warning("リトライ {Count} 回目", retryCount);
Log.Error(ex, "DB接続エラー: {ConnectionString}", connStr);
Log.Fatal(ex, "致命的エラー");
```

---

## 構造化ログ

プロパティ名を `{PropertyName}` で指定すると、値がオブジェクトとして保存される。

```csharp
// 文字列に展開される
Log.Information("ユーザー名: {Name}", "Alice");

// オブジェクトを構造化して記録（@ デストラクチャリング）
var order = new { Id = 1, Amount = 1000 };
Log.Information("注文: {@Order}", order);

// ToString() の結果を記録（$ シリアライズ）
Log.Information("注文: {$Order}", order);
```

---

## コンテキスト情報の付加（Enrich）

```bash
dotnet add package Serilog.Enrichers.Environment
dotnet add package Serilog.Enrichers.Thread
```

```csharp
Log.Logger = new LoggerConfiguration()
    .Enrich.FromLogContext()           // LogContext.PushProperty で追加したプロパティ
    .Enrich.WithMachineName()          // マシン名
    .Enrich.WithThreadId()             // スレッドID
    .Enrich.WithProperty("App", "MyApp") // 固定プロパティ
    .WriteTo.Console()
    .CreateLogger();

// スコープ付きプロパティ
using (LogContext.PushProperty("RequestId", Guid.NewGuid()))
{
    Log.Information("リクエスト処理開始");
    // このスコープ内のログには RequestId が自動付与される
}
```

---

## 出力テンプレート

```csharp
.WriteTo.Console(outputTemplate:
    "[{Timestamp:HH:mm:ss} {Level:u3}] {Message:lj}{NewLine}{Exception}")

.WriteTo.File("logs/app.log",
    outputTemplate: "{Timestamp:yyyy-MM-dd HH:mm:ss.fff zzz} [{Level:u3}] {Message:lj}{NewLine}{Exception}",
    rollingInterval: RollingInterval.Day,
    retainedFileCountLimit: 7)
```

### 主なフォーマットトークン

| トークン | 内容 |
|----------|------|
| `{Timestamp:HH:mm:ss}` | タイムスタンプ |
| `{Level:u3}` | ログレベル（3文字大文字: INF, WRN, ERR） |
| `{Message:lj}` | メッセージ（リテラルJSON） |
| `{Exception}` | 例外スタックトレース |
| `{SourceContext}` | ロガーのソースコンテキスト |
| `{NewLine}` | 改行 |

---

## DI（依存性注入）との連携

```csharp
// ILogger<T> として注入
public class OrderService
{
    private readonly ILogger<OrderService> _logger;

    public OrderService(ILogger<OrderService> logger)
    {
        _logger = logger;
    }

    public void Process(int orderId)
    {
        _logger.LogInformation("注文処理開始: {OrderId}", orderId);
    }
}
```

ASP.NET Core で `UseSerilog()` を設定していれば、`ILogger<T>` は自動的に Serilog にルーティングされる。

---

## 主要シンク一覧

| パッケージ | 出力先 |
|-----------|--------|
| `Serilog.Sinks.Console` | コンソール |
| `Serilog.Sinks.File` | ファイル（ローリング対応） |
| `Serilog.Sinks.Seq` | Seq（構造化ログサーバー） |
| `Serilog.Sinks.Elasticsearch` | Elasticsearch |
| `Serilog.Sinks.MSSqlServer` | SQL Server |
| `Serilog.Sinks.ApplicationInsights` | Azure Application Insights |
| `Serilog.Sinks.Slack` | Slack |

---

## よくあるパターン

### ファイルローリング設定

```csharp
.WriteTo.File(
    path: "logs/app-.log",
    rollingInterval: RollingInterval.Day,   // Day / Hour / Minute / Month / Year / Infinite
    retainedFileCountLimit: 30,             // 保持するファイル数（null で無制限）
    fileSizeLimitBytes: 10 * 1024 * 1024,  // 1ファイルの上限サイズ（10MB）
    rollOnFileSizeLimit: true              // サイズ超過時もローリング
)
```

### 環境ごとにレベルを変える

```json
{
  "Serilog": {
    "MinimumLevel": {
      "Default": "Warning",
      "Override": {
        "MyApp": "Debug",
        "Microsoft.AspNetCore": "Warning"
      }
    }
  }
}
```

### 例外の記録

```csharp
try
{
    // 処理
}
catch (Exception ex)
{
    // 第1引数に例外を渡す
    Log.Error(ex, "処理中にエラーが発生しました: {OrderId}", orderId);
}
```
