# Function List

| 機能ID | 画面ID | 大カテゴリ | 中カテゴリ | 機能名 | 機能概要 | ユースケース |
| ---- | ---- | ----- | ----- | --- | ---- | ------ |
|      |      |       |       |     |      |        |
# Screen Design
## Screen List

| 画面ID | 画面名 | URLパス | 概要  |
| ---- | --- | ----- | --- |
|      |     |       |     |
## Screen Sequence Diagram
```plantuml
@startuml
'設定
hide empty description
skinparam state {
  BackgroundColor White
  BorderColor Black
}

'画面定義
state "ログイン画面\n(S-001)" as Login
state "ダッシュボード\n(S-002)" as Dashboard
state "商品一覧\n(S-003)" as ProductList
state "商品登録\n(S-004)" as ProductEdit

'遷移
[*] --> Login

Login --> Dashboard : ログイン成功
Login --> Login : エラー表示

Dashboard --> ProductList : メニュー選択
Dashboard --> Login : ログアウト

ProductList --> ProductEdit : 新規登録ボタン
ProductList --> ProductEdit : 編集ボタン
ProductEdit --> ProductList : 保存/キャンセル

@enduml
```
## Wire Frame
```plantuml
@startsalt
{+
  <b>商品登録・編集 (S-004)</b>
  --
  {
    商品ID   | "P001      " | (X) 自動採番
    商品名   | "商品名を入力          "
    カテゴリ | ^食品^
    単価     | "1200" 円
    在庫数   | "50" 個
    ステータス | (X) 販売中 | ( ) 停止中
    商品説明 | {+
               .
               .
               .
               }
  }
  [  キャンセル  ] | [  保存内容を確認  ]
}
@endsalt
```
# Data Design
## ER Diagram
```mermaid
erDiagram
    %% 論理モデルのイメージ
    USER ||--o{ ORDER : "注文する"
    USER {
        string userID PK
        string name
        string mailaddress
    }
    ORDER ||--|{ ORDER_DETAIL : "含む"
    ORDER {
        string OrderID PK
        string UserID FK
        date OrderDay
    }
    ORDER_DETAIL {
        string OrderID PK, FK
        string ItemID PK, FK
        int Quntity
    }
```
## Entity Diagram
|**No**|**論理テーブル名**|**物理テーブル名**|**種類**|**概要・用途**|**発生頻度・想定件数**|
|---|---|---|---|---|---|
|1|会員マスタ|M_USER|マスタ|会員情報を管理する|低・10万件|
|2|商品マスタ|M_PRODUCT|マスタ|商品情報を管理する|低・1,000件|
|3|注文データ|T_ORDER|トランザクション|注文ヘッダ情報|高・月1万件増加|
|4|注文明細|T_ORDER_DETAIL|トランザクション|注文ごとの商品内訳|高・月3万件増加|
# Logic Design
## Class Diagram
```mermaid
classDiagram
	class classA {
		fieldA: string
		fieldB : classB
	}
	class classB {
		fieldC : boolean
		fieldD : char
	}
	classA *-- classB
```
