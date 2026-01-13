---

---
# Gitの基本概念
- **作業ディレクトリ (Working Directory)**: 実際にファイルを作成・編集する場所。
- **ステージングエリア (Staging Area/Index)**: コミットするファイルを一時的に置いておく場所。`git add`でここに登録される。
- **ローカルリポジトリ (Local Repository)**: 自分のPC上にある、変更履歴を保存する場所。`git commit`でステージングの内容がここに記録される。
- **リモートリポジトリ (Remote Repository)**: ネットワーク上(GitHubなど)にある、変更履歴を共有するための場所。`git push`でローカルの内容を送信する。
# コマンド
```
# コマンド
```shell
# セットアップ
git init

# 差分の確認 (Diff)
# 作業ディレクトリとステージングの差分
git diff
# ステージングと最新コミットの差分
git diff --staged (または --cached)
# 作業ディレクトリと最新コミットの差分
git diff HEAD

# ステージング
$ git add ファイル名
# ファイルを一括してステージング
$ git add -A

# ログの確認 (Log)
git log
git log --oneline --graph --all # グラフで履歴を簡潔に表示
git log -n 5 # 最新5件を表示

# コミット
git commit -m "コミットメッセージ"
git commit -v # 変更内容を確認しながらコミット

# リモートリポジトリの登録 originというニックネームでurl先のリモートリポジトリを登録
git remote add origin https://github.com/sample.git

# push
# originはリモートリポジトリのニックネーム（デフォルト)
git push -u origin master

# pull (fetch + merge)
git pull origin master

# fetch (リモートの変更を取得だけする)
# origin/masterなどの追跡ブランチが更新されるが、作業ディレクトリは変更されない
git fetch origin

# リモートリポジトリのニックネームを確認
git remote -v

# リモートリポジトリからローカルリポジトリをクローン
git clone https://github.com/sample.git

# ブランチの切り替え
git checkout <ブランチ名>
git switch <ブランチ名>
git checkout - #1つ前のブランチに切り替え

# 新しいブランチの作成
git checkout -b <新しいブランチ名>

# gitignoreの作成
touch .gitignore

# 直前のコミットを上書き (amend)
git commit --amend -m "新しいコミットメッセージ"

# 変更の取り消し (Reset vs Revert)
## Reset (過去に戻る - 履歴が消えることがあるため注意)
# ステージングを取り消す（作業ディレクトリはそのまま）
git reset HEAD <file>
# 直前のコミットを取り消す（ワークディレクトリの内容は維持）
git reset --soft HEAD^
# 直前のコミットと変更をすべて破棄する（危険！）
git reset --hard HEAD^

## Revert (打ち消しコミットを作る - 安全)
# 特定のコミットの内容を打ち消す新しいコミットを作成する
git revert <コミットID>

# 変更を退避 (Stash)
git stash save "退避メッセージ"
git stash -u # "untracked(未追跡ファイル)も含む"
git stash list # 退避した一覧を表示
git stash pop # 最新の退避を復元してリストから削除
git stash apply # 最新の退避を復元するがリストに残す
git stash drop stash@{0} # その退避を削除
git stash clear # 全ての退避を削除

# 別ブランチの一部のcommitだけを反映
git cherry-pick abc1234 #コミットID
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