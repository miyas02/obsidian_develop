# GitHub Actions x Google Drive 連携マニュアル
## 1. Google Cloud 側の設定（認証情報の作成）
Google Driveへプログラムからアクセスするための「サービスアカウント」を作成します。
1. **Google Cloud Console** ([console.cloud.google.com](https://console.cloud.google.com/ "null")) にアクセスします。
2. 新しいプロジェクトを作成（または既存のものを選択）します。
3. **「APIとサービス」 > 「ライブラリ」** で「Google Drive API」を検索し、**有効化**します。
4. **「APIとサービス」 > 「認証情報」** を開き、**「認証情報を作成」 > 「サービスアカウント」** を選択します。
    - 名前（例: `github-actions-sync`）を入力して作成します。
5. 作成されたサービスアカウントのメールアドレス（`xxx@xxx.iam.gserviceaccount.com`）をコピーしておきます。
6. **「キー」** タブを開き、**「鍵を追加」 > 「新しい鍵を作成」** を選択し、**JSON**形式でダウンロードします。
    - このファイルを後でGitHubに使用します。
## 2. Google Drive 側の設定（共有とID取得）
同期先のフォルダを準備し、プログラムが書き込めるようにします。
1. **Google Drive** で同期先のフォルダを新規作成（または選択）します。
2. フォルダを右クリックし、**「共有」** を選択します。
3. 手順1でコピーした **サービスアカウントのメールアドレス** を入力し、権限を **「編集者」** に設定して共有します。
4. フォルダを開き、ブラウザのURLから **フォルダID** をコピーします。
    - `https://drive.google.com/drive/folders/【ここの英数字の文字列】`
## 3. GitHub Secrets の設定（機密情報の登録）
GitHubリポジトリに認証情報を安全に登録します。
1. リポジトリの **Settings > Secrets and variables > Actions** を開きます。
2. **New repository secret** をクリックし、以下の2つを登録します。
|   |   |
|---|---|
|**名前**|**値**|
|`GDRIVE_JSON`|ダウンロードしたJSONキーの中身をすべて貼り付け|
|`GDRIVE_FOLDER_ID`|コピーしたフォルダID（英数字の文字列）を貼り付け|
## 4. GitHub Actions ワークフローの作成
以下の内容を `.github/workflows/sync_to_gdrive.yml` として保存します。
```
name: Sync Obsidian Vault to Google Drive
on:
  push:
    branches:
      - master
  workflow_dispatch:  # 手動実行ボタンを有効化
jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Install rclone
        run: sudo curl [https://rclone.org/install.sh](https://rclone.org/install.sh) | sudo bash
      - name: Configure rclone
        env:
          GDRIVE_JSON: ${{ secrets.GDRIVE_JSON }}
        run: |
          mkdir -p ~/.config/rclone
          # 環境変数経由で書き出すことでマスク(***)による破損を回避
          echo "$GDRIVE_JSON" > ~/.config/rclone/service-account.json
          
          cat <<EOF > ~/.config/rclone/rclone.conf
          [gdrive]
          type = drive
          service_account_file = /home/runner/.config/rclone/service-account.json
          scope = drive
          EOF
      - name: rclone sync
        run: |
          rclone sync . gdrive:${{ secrets.GDRIVE_FOLDER_ID }} \
            --filter "+ *.md" \
            --filter "+ *.pdf" \
            --filter "+ *.png" \
            --filter "+ *.jpg" \
            --filter "- .git/**" \
            --filter "- .obsidian/**" \
            --filter "- **" \
            --drive-use-trash \
            -v
```
## 5. 運用と管理
### 手動実行の方法
1. GitHubリポジトリの **「Actions」** タブをクリックします。
2. 左側の **「Sync Obsidian Vault to Google Drive」** を選択します。
3. **「Run workflow」** ボタン（青色）をクリックして開始します。
### NotebookLM との連携
1. NotebookLMでノートブックを作成し、ソースとして **Google Driveのフォルダ** を指定します。
2. Obsidianでメモを更新し、GitHubへプッシュ（またはActionsを手動実行）します。
3. Google Driveの更新を確認後、NotebookLMのソース画面で **「再読み込み（更新）」** アイコンをクリックすると、最新のメモがAIに反映されます。
## 💡 トラブルシューティング
- **Failed to queue workflow run**: GitHub側の障害（GitHub Statusを確認）または、YAMLのインデントミス、全角スペースの混入を疑ってください。GitHub上のエディタで貼り付け直すと解決することがあります。
- **Empty token found**: サービスアカウントの設定（`service_account_file`）が正しく読み込めていません。上記YAMLの `Configure rclone` ステップが最新か確認してください。
- **Permission Denied**: Google Driveフォルダの共有設定で、サービスアカウントに「編集者」権限があるか再確認してください。