---

---
```shell
# セットアップ
git init

# ステージング
$ git add ファイル名
# ファイルを一括してステージング
$ git add -A

# コミット
git commit -m "コミットメッセージ"

# リモートリポジトリの登録 originというニックネームでurl先のリモートリポジトリを登録
git remote add origin https://github.com/sample.git

# push
# originはリモートリポジトリのニックネーム（デフォルト)
git push -u origin master

# pull
git pull origin master

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

# 直前のコミットを上書き
git commit --amend -m "新しいコミットメッセージ"

# 変更を退避
git stash "退避"
git stash -u "untracked(未追跡ファイル)も含む"

# 別ブランチの一部のcommitだけを反映
git cherry-pick abc1234 #コミットID
```
### commit
- コミットタイミング
    - 「機能追加」「バグ修正」「リファクタリング」などを混ぜない。
    - 30分~2hくらいに一度のイメージ
### git ignore
- 作業コピーに.gitignoreを作成
- gitignoreに記載したファイル名を無視する
- .gitignoreは複数のディレクトリに置くことができる。
	- 深い階層の.gitignoreに書かれた指定の方が優先順位が高い。（後に解釈される）
- .gitignoreが無視するファイルは、まだインデックスに登録されていないものだけである。
	- 既にインデックスに登録したりコミットしたりしてしまっているファイルを再び無視するようにしたければ、シェルから`git rm --cached path/to/file`、または`git rm path/to/file`のコマンドを打とう。
- sourcetreeでファイルを右クリック→無視
#### sourceTreeで操作マニュアル
- sourcetreeでターミナルをクリック
- 以下コマンドでキャッシュを削除
`git rm --cached -r path/file`
- sourcetree上で+ファイルを右クリック→追跡をやめるを選択

既にインデックスに登録したりコミットしたりしてしまっているファイルを再び無視するようにしたければ、シェルから`git rm --cached path/to/file`、または`git rm path/to/file`のコマンドを打とう。
### Branch
- origin/developはリモートリポジトリのdevelopブランチ、developはローカルのdevelopブランチ
- ステージ済みの変更と未ステージの変更はチェックアウトするとそのままチェックアウト先に引き継がれる（競合の可能性あり)
- **branch名**
>feature
  - 新機能開発
>fix
refactor
experiment
  - 試験的
>ui
docs
### Merge
- Conflict
    - 作業コピーはコンフリクトマーカー付きのファイルになる
        - 作業コピーをステージング(add)すると、コンフリクトマーカー付きファイルをステージングしてしまう。
        - Conflictしたときは、作業コピーをそのままaddすることは基本ない。
    - ステージには同一ファイルのブランチAとブランチBとマージベース（共通祖先)の3つがエントリされる。
        - ステージから優先するエントリを選択する
### PullRequest
リモートにpushしたブランチを対象に、mainブランチにマージしてという依頼
リモートリポジトリに作業ブランチをpushした後に、PRを送る
## rebase
コミット履歴を書き換える。
基本はローカルリポジトリのコミットを対象に行う。基本はpushしたあとはNG。自分用の作業ブランチならOKかも。
- **コミット履歴を１本にする**
	- マージに似ている。マージと異なるのは、自分のブランチを切り取って、最新のメインブランチの先端に貼り直すイメージ
- **コミット履歴を整理する
	- インタラクティブ・リベース (`-i` オプション)

## chery pick
- 特定のコミットだけを別ブランチに適用する
### sourcetree操作
- logからピックしたいコミットを右クリック→チェリーピック