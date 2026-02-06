---

---
# 単体テスト基本

- テストクラスはテスト対象クラスと1対1の関係
- テストメソッドはテスト対象メソッドと1対多の関係
    - 各メソッドは正常系・異常系・境界値などの複数のテストメソッドに分ける
- テスト対象はpublicなメソッド。privateなメソッドはpublicなメソッドを経由して検証される。
- 期待値は関数の戻り値
# 単体テスト手順

1. 環境構築
    1. テストソリューションの作成と依存関係の追加
2. テスト対象のクラスのテストクラスを作成
3. テスト対象メソッドを呼び出すテストメソッドを作成
    2. 1つのテスト対象メソッドに対して、複数のテストパターンを網羅するために、複数のテストメソッドを作成する
    3. Arrange(準備), Act(実行), Assert(確認)を行う
4. Assertで期待値と合致しているか確認

# 環境構築

5. (テスト対象)ソリューション → 追加 → 新しいプロジェクト
6. xUnit(C#)を選択
7. 名前を「ソリューション名.Tests」
8. Testsの依存関係→プロジェクトの参照の追加→テスト対象のソリューションを追加
## テストプロジェクトをgitに追加
テストプロジェクトを新規作成すると、別のプロジェクトとして作成されるので、フォルダ構成を調整しgitに追加する
### 手順
1. フォルダを整理する
2. ソリューションファイルを作成
`dotnet new sln`
3. 既存プロジェクトの移動
	- 普通にエクスプローラーで移動
```diff
フォルダ構成
修正前
-project_root
-├── .git
-├── models
-├── services
-└── ...
-project_test
-├── project_test.csproj
-└── unitTest.cs
修正後
+project_root
+├── .git
+├── src
+│   ├── models
+│   ├── services
+│   └── ...
+└── tests
+    ├── .git
+    ├── project_test.csproj
+    └── unitTest.cs
```

# AAAパターン

```c#
[Fact] //テストメソッドに[Fact]をつける
public void AddTest()
{
    // Arrange（準備）
    var calculator = new Calculator();

    // Act（実行）
    var actual = calculator.Add(1, 2);

    // Assert（確認）
    Assert.Equal(3, actual);
}
```

- **Arrange（準備）**：テスト対象のオブジェクト等を準備するフェーズ
- **Act（実行）**：テスト対象のメソッドを実行するフェーズ
- **Assert（確認）**：実行結果が想定通りであることを確認するフェーズ

# internalメソッドの単体テスト

テスト対象プロジェクトのAssemblyInfo.csに以下を記載

`[assembly: InternalsVisibleTo("テストプロジェクト(ex. matome_phase1.scraper.Tests)")]`

# メソッド

```c#
//Assert
Assert.Equal(expecte, actual) //期待値と実際値合致しているかを出力
Assert.Equal(3, list.Count); // 要素数が3であることを確認
Assert.NotEmpty(list);
```

[https://drive.google.com/file/d/1Y836TN2_J7DldVeer0vxsyh79Hlqqh7C/view?usp=drive_web](https://drive.google.com/file/d/1Y836TN2_J7DldVeer0vxsyh79Hlqqh7C/view?usp=drive_web)
# bunit
## 概要
**Blazor用の単体テストライブラリ**
### パラメータ
**パラメーター（`[Parameter]`）** をコンポーネントへの「入力」として扱い、その結果表示される HTMLや発生するイベントを「出力」として検証する のが基本的な考え方です。
コンポーネント(.razorと.razor.csのセット)をブラックボックスとして扱います。
### パラメーター以外にテストできること
`[Parameter]` 以外にも、以下のような要素をテストできます。
- **クリックなどの操作**: `cut.Find("button").Click();` して、その後どう変化したか。
- **依存サービスの注入**: `[Inject]`されているサービスが正しく呼ばれたか。
## 期待値
**レンダリングされた結果**
htmlのタグ、cssクラス、テキストコンテンツ、子コンポーネントetc
```cs
//act
var expect = bunit.Render<Nameplate>();
//assert
var div = expect.Find("div");
Assert.Contains("bg-gray-200", div.ClassName);
```
親コンポーネントをテストする場合、**「子コンポーネントのプロパティが期待通りになっているか」** をチェックする。
## AAA
1. **入力 (Arrange)**: `[Parameter]` に値をセットする
2. **実行 (Act)**: コンポーネントを描画する (Render)
3. **検証 (Assert)**: 期待通りの HTML クラスが付いているか / 文字が表示されているか確認する