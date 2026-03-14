# 概要
フォルダをコンテナ内で（またはコンテナにマウントして）開き、Visual Studio Codeの全機能セットを活用できます。
プロジェクト内の[devcontainer.jsonファイル](https://qiita.com/sugo_mzk/items/db5b9ba73649db87e707#devcontainerjson%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%81%AE%E4%BD%9C%E6%88%90)が、明確に定義されたツールとランタイムスタックを持つ開発コンテナにVS Codeがアクセス（または作成）する方法を指示します。
# システム要件
- ローカルにインストールされたDocker
- Visual Studio Codeのインストール
- Dev Containers拡張機能のインストール
	- VS Codeで他のリモート拡張機能と一緒に作業する予定がある場合は、[Remote Development拡張機能パック](https://aka.ms/vscode-remote/download/extension)のインストールを選択できます。
# 手順
## init
- 左下の`><`を選択
- **コンテナで再度開く**を選択
- **ワークスペースに構成を追加する**を選択
	- （オプション）**ユーザー データ フォルダーに構成**を追加するを選択すると、ローカルのみに反映
- **ベーステンプレート**を選択
- **Features**を選択
## Reopen
- 左下の`><`を選択
- **コンテナで再度開く**を選択
# Sample
## claude codeをコンテナに追加したdevcontainers.json
```json
{
    "name": "Node.js & TypeScript",
    "image": "mcr.microsoft.com/devcontainers/typescript-node:4-24-bookworm",
    "postCreateCommand": "npm install -g @anthropic-ai/claude-code",
    "customizations": {
        "vscode": {
            "extensions": [
                "anthropic.claude-dev"
            ]
        }
    },
    "remoteUser": "node"
}
```