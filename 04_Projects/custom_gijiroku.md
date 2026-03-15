- リポジトリ
[miyas02/vscode_plugin_gijiroku](https://github.com/miyas02/vscode_plugin_gijiroku)
- 0.0.1 Release
[Release custom-gijiroku0.0.2 · miyas02/vscode_plugin_gijiroku](https://github.com/miyas02/vscode_plugin_gijiroku/releases/tag/custom-gijiroku0.0.2)
# Base Design
## 概要
visual studio codeのプラグイン
コードブロック「giji」と記載すると、議事録用に文字列変換する
文字列変換はvscode設定統合で設定可能。
## Function List

| 機能ID | カテゴリ  | 機能名                 | 機能概要                                        |
| ---- | ----- | ------------------- | ------------------------------------------- |
| F01  | 初期化   | インストール時のPlugin初期化処理 | プラグインインストール時の処理<br>・README表示（インストール時１回のみ）   |
| F02  | コンフィグ | コンフィグ読み込み           | vscode設定統合から読み込み                            |
| F03  | コンフィグ | コンフィグデシリアライズ        | コンフィグ情報をデシリアライズ                             |
| F04  | コンフィグ | コンフィグバリデーション        | 置換文字列の正規表現が正しいか                             |
| F05  | 文字列置換 | 置換対象文字列検索           | コードブロック「giji」内をコンフィグで定義したreplace_targetで検索  |
| F06  | 文字列置換 | 置換文字列生成             | replace_targetをreplace_textの正規表現で置換した文字列を生成 |
| F07  | 文字列置換 | 文字列置換実行             | replace_targetを生成した置換文字列で置換                 |
## Data Design
### Data Diagram
| **No** | **テーブル名**  | **プロパティ名**     | **概要・用途**   |
| ------ | ---------- | -------------- | ----------- |
| 1      | vscode設定統合 | replace_target | 置換対象の文字列を定義 |
| 2      | vscode設定統合 | replace_text   | 置換法則を定義     |
# Detail Design
## F01 インストール時のPlugin初期化処理

# 実装
## F01 インストール時のPlugin初期化処理

