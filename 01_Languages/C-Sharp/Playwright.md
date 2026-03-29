# Playwright for .NET

Microsoft 製のブラウザ自動化・E2Eテストライブラリ。Chromium / Firefox / WebKit に対応。

---

## インストール

```bash
dotnet add package Microsoft.Playwright
dotnet add package Microsoft.Playwright.NUnit   # NUnit 用
dotnet add package Microsoft.Playwright.MSTest  # MSTest 用
```

ブラウザのインストール（初回のみ）:

```bash
# ビルド後に実行
pwsh bin/Debug/net8.0/playwright.ps1 install

# または dotnet tool 経由
dotnet tool install --global Microsoft.Playwright.CLI
playwright install
```

---

## 基本的な使い方

```csharp
using Microsoft.Playwright;

// Playwright インスタンス生成
using var playwright = await Playwright.CreateAsync();

// ブラウザ起動
await using var browser = await playwright.Chromium.LaunchAsync(new BrowserTypeLaunchOptions
{
    Headless = true  // false にすると画面表示
});

// ページ操作
var page = await browser.NewPageAsync();
await page.GotoAsync("https://example.com");

var title = await page.TitleAsync();
Console.WriteLine(title);

await browser.CloseAsync();
```

---

## ブラウザの種類

```csharp
// Chromium（Chrome / Edge）
await playwright.Chromium.LaunchAsync();

// Firefox
await playwright.Firefox.LaunchAsync();

// WebKit（Safari）
await playwright.Webkit.LaunchAsync();
```

---

## ページ操作

### ナビゲーション

```csharp
await page.GotoAsync("https://example.com");
await page.GoBackAsync();
await page.GoForwardAsync();
await page.ReloadAsync();
```

### クリック・入力

```csharp
// クリック
await page.ClickAsync("button#submit");
await page.ClickAsync("text=ログイン");

// テキスト入力
await page.FillAsync("input[name='email']", "user@example.com");
await page.FillAsync("input[name='password']", "password");

// キーボード操作
await page.PressAsync("input[name='search']", "Enter");
await page.Keyboard.PressAsync("Tab");

// セレクトボックス
await page.SelectOptionAsync("select#country", "JP");

// チェックボックス
await page.CheckAsync("input[type='checkbox']");
await page.UncheckAsync("input[type='checkbox']");
```

### 要素の取得

```csharp
// Locator（推奨）
var button = page.Locator("button#submit");
await button.ClickAsync();

// テキストで取得
var link = page.GetByText("詳細を見る");

// ロールで取得
var submitBtn = page.GetByRole(AriaRole.Button, new() { Name = "送信" });

// ラベルで取得
var emailInput = page.GetByLabel("メールアドレス");

// プレースホルダーで取得
var searchInput = page.GetByPlaceholder("検索キーワード");

// テストIDで取得（data-testid 属性）
var item = page.GetByTestId("product-card");
```

### 値の取得

```csharp
// テキスト内容
var text = await page.InnerTextAsync("h1");
var html = await page.InnerHTMLAsync(".content");

// 属性値
var href = await page.GetAttributeAsync("a.link", "href");

// 入力値
var value = await page.InputValueAsync("input[name='email']");

// タイトル・URL
var title = await page.TitleAsync();
var url = page.Url;
```

---

## 待機

```csharp
// ナビゲーション完了まで待機
await page.WaitForURLAsync("**/dashboard");

// 要素が表示されるまで待機
await page.WaitForSelectorAsync(".loading", new() { State = WaitForSelectorState.Hidden });

// ネットワークアイドルまで待機
await page.WaitForLoadStateAsync(LoadState.NetworkIdle);

// 特定のレスポンスを待機
var response = await page.WaitForResponseAsync("**/api/users");

// 任意の条件まで待機
await page.Locator(".result").WaitForAsync();
```

---

## スクリーンショット・PDF

```csharp
// スクリーンショット
await page.ScreenshotAsync(new PageScreenshotOptions
{
    Path = "screenshot.png",
    FullPage = true   // ページ全体をキャプチャ
});

// 特定要素のスクリーンショット
await page.Locator(".chart").ScreenshotAsync(new() { Path = "chart.png" });

// PDF（Chromium のみ）
await page.PdfAsync(new PagePdfOptions { Path = "page.pdf" });
```

---

## NUnit によるテスト

```csharp
using Microsoft.Playwright.NUnit;
using NUnit.Framework;

[Parallelizable(ParallelScope.Self)]
[TestFixture]
public class LoginTests : PageTest
{
    [Test]
    public async Task ログインできること()
    {
        await Page.GotoAsync("https://example.com/login");

        await Page.GetByLabel("メールアドレス").FillAsync("user@example.com");
        await Page.GetByLabel("パスワード").FillAsync("password");
        await Page.GetByRole(AriaRole.Button, new() { Name = "ログイン" }).ClickAsync();

        await Expect(Page).ToHaveURLAsync("**/dashboard");
        await Expect(Page.GetByText("ようこそ")).ToBeVisibleAsync();
    }
}
```

### PageTest が提供するもの

| プロパティ / メソッド | 内容 |
|----------------------|------|
| `Page` | 各テスト用の Page インスタンス |
| `Context` | BrowserContext インスタンス |
| `Browser` | Browser インスタンス |
| `Expect(locator)` | アサーションメソッド群 |

---

## アサーション（Expect）

```csharp
// 表示確認
await Expect(Page.Locator("h1")).ToBeVisibleAsync();
await Expect(Page.Locator(".error")).ToBeHiddenAsync();

// テキスト確認
await Expect(Page.Locator("h1")).ToHaveTextAsync("ダッシュボード");
await Expect(Page.Locator(".list")).ToContainTextAsync("アイテム1");

// URL確認
await Expect(Page).ToHaveURLAsync("https://example.com/dashboard");
await Expect(Page).ToHaveURLAsync(new Regex(".*/dashboard$"));

// タイトル確認
await Expect(Page).ToHaveTitleAsync("マイアプリ");

// 要素の状態
await Expect(Page.Locator("button")).ToBeEnabledAsync();
await Expect(Page.Locator("button")).ToBeDisabledAsync();
await Expect(Page.Locator("input[type='checkbox']")).ToBeCheckedAsync();

// 属性確認
await Expect(Page.Locator("input")).ToHaveValueAsync("初期値");
await Expect(Page.Locator("a")).ToHaveAttributeAsync("href", "/about");
```

---

## BrowserContext（セッション管理）

```csharp
// コンテキストを分けることでセッションを独立させられる
await using var context1 = await browser.NewContextAsync();
await using var context2 = await browser.NewContextAsync();

var page1 = await context1.NewPageAsync();
var page2 = await context2.NewPageAsync();

// ストレージ状態の保存（ログイン状態の保存）
await context1.StorageStateAsync(new() { Path = "auth.json" });

// 保存した状態で新しいコンテキストを開始
var loggedInContext = await browser.NewContextAsync(new()
{
    StorageStatePath = "auth.json"
});
```

---

## ネットワーク操作

```csharp
// リクエストのインターセプト
await page.RouteAsync("**/api/**", async route =>
{
    // モックレスポンスを返す
    await route.FulfillAsync(new RouteFulfillOptions
    {
        Status = 200,
        ContentType = "application/json",
        Body = """{"id": 1, "name": "テスト"}"""
    });
});

// リクエストをブロック
await page.RouteAsync("**/*.{png,jpg,svg}", route => route.AbortAsync());

// リクエストの内容を確認
page.Request += (_, request) =>
{
    Console.WriteLine($">> {request.Method} {request.Url}");
};
```

---

## playwright.config（設定ファイル）

NUnit / MSTest の場合は `.runsettings` または環境変数で制御する。

```xml
<!-- .runsettings -->
<RunSettings>
  <Playwright>
    <BrowserName>chromium</BrowserName>
    <LaunchOptions>
      <Headless>false</Headless>
      <SlowMo>100</SlowMo>
    </LaunchOptions>
  </Playwright>
</RunSettings>
```

環境変数でも設定可能:

```bash
PLAYWRIGHT_BROWSERS_PATH=0      # ブラウザのキャッシュパス
HEADED=1                        # ヘッドフルモード
BROWSER=firefox                 # 使用ブラウザ
PWDEBUG=1                       # デバッグモード（Inspector 起動）
```

---

## デバッグ

```bash
# Playwright Inspector を起動
PWDEBUG=1 dotnet test

# トレースを記録
```

```csharp
// トレース記録の開始
await context.Tracing.StartAsync(new()
{
    Screenshots = true,
    Snapshots = true,
    Sources = true
});

// テスト実行...

// トレース保存
await context.Tracing.StopAsync(new() { Path = "trace.zip" });
```

```bash
# トレースビューアーで確認
pwsh playwright.ps1 show-trace trace.zip
```

---

## よくあるパターン

### ページオブジェクトモデル（POM）

```csharp
public class LoginPage
{
    private readonly IPage _page;

    public LoginPage(IPage page) => _page = page;

    public async Task GotoAsync() =>
        await _page.GotoAsync("/login");

    public async Task LoginAsync(string email, string password)
    {
        await _page.GetByLabel("メールアドレス").FillAsync(email);
        await _page.GetByLabel("パスワード").FillAsync(password);
        await _page.GetByRole(AriaRole.Button, new() { Name = "ログイン" }).ClickAsync();
    }
}

// テストでの使用
var loginPage = new LoginPage(Page);
await loginPage.GotoAsync();
await loginPage.LoginAsync("user@example.com", "password");
```

### ファイルのアップロード・ダウンロード

```csharp
// ファイルアップロード
await page.SetInputFilesAsync("input[type='file']", "path/to/file.pdf");

// 複数ファイル
await page.SetInputFilesAsync("input[type='file']", new[] { "file1.pdf", "file2.pdf" });

// ダウンロード待機
var download = await page.RunAndWaitForDownloadAsync(async () =>
{
    await page.ClickAsync("a#download-link");
});
await download.SaveAsAsync("downloaded.pdf");
```

### ダイアログ処理

```csharp
// アラート・確認ダイアログを自動承認
page.Dialog += async (_, dialog) => await dialog.AcceptAsync();

// ダイアログのテキストを確認してから処理
page.Dialog += async (_, dialog) =>
{
    Console.WriteLine(dialog.Message);
    await dialog.DismissAsync(); // キャンセル
};
```
