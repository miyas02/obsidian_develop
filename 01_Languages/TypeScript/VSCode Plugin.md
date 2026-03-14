マークダウンライブラリ：[[Markdown-it]]
# 事前準備
（推奨）devcontainers構築する。
# 環境構築
- terminalでコマンドを実行
	`npm install -g yo generator-code`
	`yo code`
- 対話形式の設定を行う
```bash
What type of extension?: New Extension (TypeScript)
Extension name: 好きな名前
Identifier: そのままエンター
Description: 拡張機能の説明
Initialize a git repository?: Yes/No どちらでも
Bundle the source code with webpack?: No（最初はシンプルに）    
Which package manager to use?: `npm`
```
- 実行する
タブの実行でプログラムを実行する。
実行するとサンドボックスのウィンドウがでる。
コマンドパレット：`Hello world`と入力して、通知にhelloworldが出力されたら環境構築完了。
# ディレクトリ構造
- `src/extension.ts`
	メインのロジックを記述
- `package.json`
	拡張機能の動作を定義する。
# markdown-it
[markdown-it]([markdown-it/markdown-it:正しく作られたMarkdownパーサー。100%CommonMark対応、拡張機能、構文プラグイン、高速対応](https://github.com/markdown-it/markdown-it?tab=readme-ov-file#simple))
## 概要
vscode拡張のmarkdownファイルの拡張を行うライブラリ
## 環境構築
- コマンドを実行
	`npm install markdown-it`
- typescriptの場合は下のコマンドも実行
	`npm install --save-dev @types/markdown-it`
## 実行
 コマンドを実行
	`npm run compile`
デバッグを実行
## package.json
```json
"contributes": {
  "markdown.markdownItPlugins": true,
  "markdown.previewScripts": [
    "./main.js"
  ],
}
```
## extension.ts
```ts
import * as vscode from 'vscode';
//plubin実行
export function activate(context: vscode.ExtensionContext) {
    console.log('--- My Markdown Plugin is Loading! ---');
    return {
	    //markdown-it
        extendMarkdownIt(md: any) {
            console.log('--- extendMarkdownIt is called! ---');
        }
    };
}
```
## ナレッジ
`npm run compile` 実行すると`out/extension.js`が生成（上書き）
## オブジェクト
`state` markdownファイル全部が入ったオブジェクト
`token` markdownを分解した最小単位
`token.content` 生のテキストデータ

# プラグイン設定（VSCode設定統合）
ユーザーがVSCodeの設定画面から変更できる方式。
## package.json
```json
"contributes": {
  "configuration": {
    "title": "拡張機能名",
    "properties": {
      "extensionName.settingKey": {
        "type": "array",
        "default": [],
        "description": "設定の説明"
      }
    }
  }
}
```
## extension.ts で読み込み
```ts
const config = vscode.workspace.getConfiguration('extensionName');
const value = config.get<string[]>('settingKey');
```
