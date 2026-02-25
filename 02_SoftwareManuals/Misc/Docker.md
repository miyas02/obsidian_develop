# 概要
- **Dockerfile（個別の設計図）**
コンテナの「中身」を定義するファイルです。
役割: 単一のDockerイメージを作成する。
- **docker-compose.yaml（全体の構成図）**
複数のコンテナの「つながり」を定義するファイルです。
役割: 複数のコンテナの管理、ネットワークやストレージ（Volume）の定義。
- **devcontainer.json(開発環境設定)**
VS Codeなどのエディタで、コンテナを「開発環境」して使うための設定ファイルです。
**役割:** コンテナ内での開発体験（DX）のカスタマイズ。
# 使い方
1. **開発中もコンテナの中で作業する (Dev Containers方式)**：`devcontainer.json` を使うモダンな方法。
2. **成果物（実行ファイル）だけをコンテナにする (Dockerfile方式)** ：デプロイや配布を目的とした、従来の方法。
## 1. Dev Containers方式
- vscodeのコマンドパレットで`Dev Containers: Add Dev Container Configuration Files...` を選択。
- `dotnet new webapi -n MyApp`コマンドをターミナルで実行
- 左下に><が表示されていたらコンテナにリモート接続済み
### 補足
blazorプロジェクト作成コマンド
`dotnet new blazor -n MyBlazorApp`
# クローンしたリポジトリにある`Dockerfile`を使って環境構築
- **リポジトリに`docker-compose.yml`が含まれている場合**
	- ビルドと起動を同時に行う
- **コマンドを実行** : `docker-compose up -d`
	`docker-compose.yaml`ファイルがあるディレクトリでコマンド実行
- **コマンドで実行確認** : `docker-compose ps`
- `devcontainers`を実行
	- コマンドパレットでdevcontainers: reopenを選択
