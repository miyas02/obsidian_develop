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
# claudeの設定を追加
### システム要件
**Node.js**：ホスト側にインストール済みであること
**Docker Desktop**：起動済みであること
### 手順
1. **ホストに Claude Code をインストール**
	cmd： `npm install -g @anthropic-ai/claude-code`
2. **ホストで認証を実行**
	cmd： `claude`
3. **1年間有効な OAuth トークンを取得します**
	`claude setup-token`
4. **環境変数にトークンを永続保存**
```cmd
[System.Environment]::SetEnvironmentVariable(
  "CLAUDE_CODE_OAUTH_TOKEN",
  "sk-ant-oat01-YOUR_TOKEN_HERE",
  "User"
)
```
5. **~/.claude.json にトークンを追加**
```powershell
$json = Get-Content "$env:USERPROFILE\.claude.json" | ConvertFrom-Json
$json | Add-Member -Name "oauthToken" `
         -Value "sk-ant-oat01-YOUR_TOKEN_HERE" `
         -MemberType NoteProperty
$json | ConvertTo-Json | Set-Content "$env:USERPROFILE\.claude.json"
```
確認コマンド：`Get-Content "$env:USERPROFILE\.claude.json"`
6. **devcontainer.json を設定**
```json
{
  "name": "Node.js & TypeScript",
  "image": "mcr.microsoft.com/devcontainers/typescript-node:4-24-bookworm",
  "postCreateCommand": "npm install -g @anthropic-ai/claude-code",
  "mounts": [
    "source=${localEnv:USERPROFILE}/.claude,target=/home/node/.claude,type=bind",
    "source=${localEnv:USERPROFILE}/.claude.json,target=/home/node/.claude.json,type=bind"
  ],
  "remoteEnv": {
    "CLAUDE_CODE_OAUTH_TOKEN": "${localEnv:CLAUDE_CODE_OAUTH_TOKEN}"
  },
  "customizations": {
    "vscode": {
      "extensions": ["anthropic.claude-dev"]
    }
  },
  "remoteUser": "node"
}
```
# Sample
## claude codeをコンテナに追加したdevcontainers.json
```json
{
    "name": "C# (.NET)",
    "image": "mcr.microsoft.com/devcontainers/dotnet:2-10.0-noble",
    "features": {
        "ghcr.io/devcontainers/features/node:1": {
            "version": "lts"
        }
    },
    "mounts": [
       "source=${localEnv:USERPROFILE}/.claude,target=/home/vscode/.claude,type=bind",
        "source=${localEnv:USERPROFILE}/.claude.json,target=/home/vscode/.claude.json,type=bind"
    ],
    "remoteEnv": {
        "CLAUDE_CODE_OAUTH_TOKEN": "${localEnv:CLAUDE_CODE_OAUTH_TOKEN}"
    },
    "postCreateCommand": "npm install -g @anthropic-ai/claude-code @linear/sdk && claude mcp add --transport stdio linear -- npx -y @modelcontextprotocol/server-linear",
    "customizations": {
        "vscode": {
            "extensions": [
                "anthropic.claude-dev"
            ]
        }
    },
    "remoteUser": "vscode"
}
```