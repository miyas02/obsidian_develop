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
## Dockerfile + docker-composeで構成
`Dockerfile`と`docker-compose.yml`を作成して、`docker compose up --build`を実行  
`Dockerfile` → イメージの設計図（何をインストールするか）
`docker-compose.yml` → コンテナの実行設定（環境変数、ポートなど）  
```bash
docker compose up --build
```
### ディレクトリ構成
```
project/
├── docker-compose.yml           # サービス定義
├── docker-compose.override.yml  # ローカル開発用上書き（任意）
├── .env                         # 環境変数
├── app/
│   ├── Dockerfile               # アプリのDockerfile
│   └── src/                     # ソースコード
└── db/
    └── init/                    # DB初期化スクリプト
```
### Dockerfile
#### 基本構文

| 命令         | 説明                     |
| ---------- | ---------------------- |
| FROM       | ベースイメージを指定する           |
| WORKDIR    | コンテナ内の作業ディレクトリを設定する    |
| COPY       | ホストのファイルをコンテナにコピーする    |
| RUN        | イメージビルド時にコマンドを実行する     |
| EXPOSE     | コンテナが使用するポートを宣言する      |
| ENV        | 環境変数を設定する              |
| CMD        | コンテナ起動時のデフォルトコマンドを指定する |
| ENTRYPOINT | コンテナの実行コマンドを固定する       |

#### サンプル（.NET アプリ・マルチステージビルド）
```dockerfile
# ── ビルドステージ ──────────────────────────
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app

COPY *.csproj ./
RUN dotnet restore

COPY . ./
RUN dotnet publish -c Release -o /out

# ── 実行ステージ ─────────────────────────────
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build /out .
EXPOSE 8080
ENTRYPOINT ["dotnet", "MyApp.dll"]
```

**マルチステージビルドのメリット**
- 本番イメージにSDKが含まれないため、イメージサイズが大幅に削減される
- ビルド用依存関係と実行用依存関係を分離できる
- セキュリティリスクを低減できる
### docker-compose.yml
#### 主要設定項目

| キー          | 説明                                   |
| ----------- | ------------------------------------ |
| services    | 各サービス（コンテナ）の定義                       |
| build       | Dockerfileのパスを指定してビルド                |
| image       | DockerHubなどのイメージを直接指定                |
| ports       | ホスト:コンテナのポートマッピング                    |
| volumes     | データの永続化・ホストとの共有                      |
| environment | 環境変数の設定                              |
| env_file    | .envファイルから環境変数を読み込む                  |
| depends_on  | 起動順序の依存関係を定義                         |
| networks    | コンテナ間のネットワーク設定                       |
| restart     | コンテナの再起動ポリシー（always / on-failure など） |

#### サンプル（アプリ + DB構成）
```yaml
version: '3.9'

services:
  app:
    build: ./app
    ports:
      - "8080:8080"
    environment:
      - DB_HOST=db
      - DB_PORT=5432
    depends_on:
      db:
        condition: service_healthy
    networks:
      - app-network
    restart: on-failure

  db:
    image: postgres:16
    ports:
      - "5432:5432"
    env_file:
      - .env
    volumes:
      - db-data:/var/lib/postgresql/data
      - ./db/init:/docker-entrypoint-initdb.d
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "postgres"]
      interval: 5s
      retries: 5
    networks:
      - app-network

volumes:
  db-data:

networks:
  app-network:
```

### .env ファイル
機密情報や環境依存の設定を管理します。`.gitignore` に追加してリポジトリにコミットしないようにしてください。

```env
POSTGRES_USER=myuser
POSTGRES_PASSWORD=mypassword
POSTGRES_DB=mydb
```

### 主要コマンド

| コマンド                           | 説明                      |
| ------------------------------ | ----------------------- |
| `docker compose up -d`         | バックグラウンドで全サービス起動        |
| `docker compose up --build`    | イメージをビルドして起動            |
| `docker compose down`          | 全サービス停止・コンテナ削除          |
| `docker compose down -v`       | ボリュームも含めて削除             |
| `docker compose ps`            | サービスの稼働状況確認             |
| `docker compose logs -f`       | 全サービスのログをリアルタイム表示       |
| `docker compose logs -f app`   | 特定サービスのログを表示            |
| `docker compose exec app bash` | 実行中のコンテナにシェル接続          |
| `docker compose restart app`   | 特定サービスを再起動              |
| `docker compose pull`          | イメージを最新に更新              |
| `docker system prune -f`       | 未使用リソースを一括削除            |

### 起動手順
#### 初回起動
1. リポジトリをクローンする
```bash
git clone <repository-url>
cd project
```
2. `.env` ファイルを作成する
```bash
cp .env.example .env
# .env を編集して各値を設定する
```
3. イメージをビルドして起動する
```bash
docker compose up --build -d
```
4. 起動確認
```bash
docker compose ps
docker compose logs -f
```

#### 更新時の手順
```bash
git pull
docker compose up --build -d
```

#### 停止・削除
- サービス停止（データ保持）：`docker compose down`
- サービス停止＋ボリューム削除（データもリセット）：`docker compose down -v`

### トラブルシューティング

| 症状            | 対処法                                   |
| ------------- | ------------------------------------- |
| コンテナが起動しない    | `docker compose logs` でエラー内容を確認する     |
| ポートが既に使用されている | `ports` のホスト側ポート番号を変更する               |
| 変更が反映されない     | `docker compose up --build` で再ビルドする   |
| DBに接続できない     | `depends_on` の `healthcheck` 設定を確認する  |
| ディスク容量が不足     | `docker system prune -f` で不要リソースを削除する |
| Windowsで権限エラー | Docker Desktopを管理者として実行する             |


### 補足
#### 開発環境と本番環境の切り替え
```bash
# 開発環境（デフォルト）
docker compose up -d

# 本番環境
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

#### よく使うイメージ

| イメージ                                   | 用途                |
| ---------------------------------------- | ----------------- |
| `mcr.microsoft.com/dotnet/aspnet:8.0`   | .NET 8 実行環境       |
| `mcr.microsoft.com/dotnet/sdk:8.0`      | .NET 8 SDK（ビルド用）  |
| `postgres:16`                            | PostgreSQL データベース |
| `mysql:8.0`                              | MySQL データベース      |
| `redis:7`                                | Redis キャッシュ       |
| `nginx:alpine`                           | Nginx リバースプロキシ    |

# クローンしたリポジトリにある`Dockerfile`を使って環境構築
- **リポジトリに`docker-compose.yml`が含まれている場合**
	- ビルドと起動を同時に行う
- **コマンドを実行** : `docker-compose up -d`
	`docker-compose.yaml`ファイルがあるディレクトリでコマンド実行
- **コマンドで実行確認** : `docker-compose ps`
- `devcontainers`を実行
	- コマンドパレットでdevcontainers: reopenを選択
