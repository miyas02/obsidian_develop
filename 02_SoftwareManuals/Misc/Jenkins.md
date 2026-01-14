---

---
# Pipeline

### SCMをポーリング

リポジトリを監視するだけで、スケジュールに従ってリポジトリを問い合わせ(ポーリング)し、前回ビルド時点と差分があればジョブ(Pipeline)をキューに入れて実行します。差分がなければビルドを起動しません。

```javascript
pipeline {
	agent any //実行するサーバー, anyで指定なし
	options {
		skipDefaultCheckout() //ポーリングする際の自動チェックアウトをスキップ
	}
	stages {
		stage("sample") { //stage
			steps {
				bat 'echo "test jenkins"'
			}
		}
	}
}	
```
