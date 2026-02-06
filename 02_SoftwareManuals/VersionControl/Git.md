---

---
# Gitの基本概念
- **作業ディレクトリ (Working Directory)**: 実際にファイルを作成・編集する場所。
- **ステージングエリア (Staging Area/Index)**: コミットするファイルを一時的に置いておく場所。`git add`でここに登録される。
- **ローカルリポジトリ (Local Repository)**: 自分のPC上にある、変更履歴を保存する場所。`git commit`でステージングの内容がここに記録される。
- **リモートリポジトリ (Remote Repository)**: ネットワーク上(GitHubなど)にある、変更履歴を共有するための場所。`git push`でローカルの内容を送信する。
# コマンド
## セットアップ
```shell
git init
```
## 状態・差分確認
```shell
# 状態の確認
git status

# 差分の確認 (Diff)
# 作業ディレクトリとステージングの差分
git diff
# ステージングと最新コミットの差分
git diff --staged (または --cached)
# 作業ディレクトリと最新コミットの差分
git diff HEAD
```
## ステージング
```shell
# ファイル単位
git add <ファイル名>
# 全変更を一括
git add -A
```
## コミット
```shell
git commit -m "コミットメッセージ"
git commit -v # 変更内容を確認しながらコミット

# 直前のコミットを上書き (amend)
git commit --amend -m "新しいコミットメッセージ"
```
## ログ確認
```shell
git log
git log --oneline --graph --all # グラフで履歴を簡潔に表示
git log -n 5 # 最新5件を表示
```
## リモート操作
```shell
# リモートリポジトリの登録
git remote add origin https://github.com/sample.git

# リモート一覧確認
git remote -v

# push
git push -u origin master

# pull (fetch + merge)
git pull origin master

# fetch
git fetch origin

# clone
git clone https://github.com/sample.git
```
## ブランチ操作
```shell
# 作成
git checkout -b <新しいブランチ名>

# 切り替え
git checkout <ブランチ名>
git switch <ブランチ名>
git checkout - # 前のブランチへ

# 一覧表示
git branch
```
## 変更の取り消し・退避
```shell
# Reset (注意: 履歴が消える場合あり)
## ステージングのみ取り消し
git reset HEAD <file>
## 直前コミット取り消し (ワークディレクトリ維持)
git reset --soft HEAD^
## 全て破棄 (危険)
git reset --hard HEAD^

# Revert (安全: 打ち消しコミット)
git revert <コミットID>

# Stash (退避)
git stash save "メッセージ"
git stash -u # 未追跡含む
git stash list
git stash pop
git stash apply
git stash drop stash@{0}
git stash clear
```
## その他
```shell
# gitignore作成
touch .gitignore

# Cherry Pick
git cherry-pick <commit_id>
```
# ベストプラクティス
## コミットの粒度とタイミング
- 「機能追加」「バグ修正」「リファクタリング」などを混ぜない。
- 30分~2hくらいに一度のイメージ。
## ブランチの命名規則 (Branch命名)
- **feature**: 新機能開発
- **fix**: バグ修正
- **refactor**: リファクタリング
- **experiment**: 試験的実装
- **ui**: UI変更
- **docs**: ドキュメントのみの変更
# 高度な機能と運用
## .gitignore
- Gitの追跡対象から外したいファイル（OSの生成ファイル、ビルド成果物、秘匿情報など）を指定する。
- 複数のディレクトリに置くことができ、深い階層の設定が優先される。
- **重要**: 既に追跡（コミット/ステージング）されているファイルは、単に.gitignoreに追加しても無視されない。キャッシュを削除する必要がある。

### キャッシュの削除 (追跡の解除)
追跡済みのファイルを.gitignoreに反映させたい場合：
```shell
# ファイル単位
git rm --cached path/to/file

# ディレクトリ以下再帰的
git rm --cached -r path/directory
```
#### SourceTreeでの操作
- ターミナルを開いて上記コマンドを実行する。
- または、変更ファイル一覧で対象ファイルを右クリック → 「追跡をやめる」を選択。
## Branch (運用)
- **origin/develop**: リモートリポジトリのdevelopブランチ。
- **develop**: ローカルのdevelopブランチ。
- ブランチを切り替える際、ステージ済みの変更と未ステージの変更は、切り替え先に引き継がれる（競合するファイルがある場合は切り替えられない）。
## Merge & Conflict
### マージ (Merge)
- `git merge <ブランチ名>` で他ブランチの変更を取り込む。
### コンフリクト (Conflict)
- マージ時に同じ行を修正していた場合などに発生。
- Gitは自動解決できないため、ファイルにコンフリクトマーカー（`<<<<<<<`, `=======`, `>>>>>>>`）を挿入して停止する。
- **対処法**:
    1. ファイルを開いてコンフリクト箇所を手動で修正する。
    2. 修正したら `git add` する（これが解消の合図になる）。
    3. `git commit` でマージコミットを作成する。
    - ※コンフリクト中は作業コピーをそのままaddしてはいけない（マーカーが残ったままになるため）。

## Rebase
- **概要**: コミット履歴を書き換える機能。
- **履歴を1本にする**: マージコミットを作らず、ベースのブランチの先に自分の変更を付け替える。直列な履歴になる。
- **履歴を整理する**: `git rebase -i` (インタラクティブモード) で、過去のコミットをまとめたり、メッセージを修正したりできる。
- **注意**: リモートリポジトリにPush済みのコミットに対してRebaseを行ってはならない（チームメンバーの履歴と整合しなくなるため）。自分専用の作業ブランチでのみ行う。
## Cherry Pick
- 別のブランチから特定のコミットだけを選んで現在のブランチに取り込む。
```shell
git cherry-pick <commit_id>
```
### SourceTreeでの操作
- Logビューから取り込みたいコミットを右クリック → 「チェリーピック」を選択。
## Pull Request
- Github等の機能。リモートにPushした自分の開発ブランチを、メインブランチ（main/master/develop）にマージしてもらうよう依頼するもの。
- コードレビューの場としても使われる。
## Tags
- リリースポイントなどを記録するために使用する静的なポインタ。
```shell
# タグの作成 (軽量タグ)
git tag <tagname>

# 注釈付きタグの作成 (推奨)
git tag -a <tagname> -m "タグのメッセージ"

# タグの一覧表示
git tag

# タグをリモートに送信
git push origin <tagname>

# ローカルのタグを削除
git tag -d <tagname>

# リモートのタグを削除
git push origin :<tagname>
```
# Troubleshooting
## commitを取り消したい
- 直前のコミットの場合: `git reset --soft HEAD^` (変更内容はステージに残る)
- 完全に無かったことにしたい場合: `git reset --hard HEAD^` (変更内容も消えるため危険)
- リモートにプッシュ済みの場合: `git revert <commit_id>` (内容を打ち消す新しいコミットを作る)
## ブランチを切り替えられない
- 変更中のファイルがある場合、チェックアウトできないことがある。
- 対処: `git stash` で一時退避するか、コミットしてから切り替える。
## Detached HEAD状態
- 特定のコミット（ブランチの先端以外）をチェックアウトしている状態。
- ここでコミットしてもどのブランチにも属さないため、後で参照できなくなる可能性がある。
- 変更を保存したい場合は、新しいブランチを作成する: `git checkout -b <new-branch-name>`
# SourceTree操作
## カスタム操作
- ツール -> オプション -> カスタム操作 -> 追加
- 設定例
>実行するスクリプト : `git`
  パラメータ : `clean -fd`