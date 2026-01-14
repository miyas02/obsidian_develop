**検索するためのSQL文**

指定した列に指定したキーワードを含む行だけを検索するには、次のようにwhere句でlike演算子を使用する
`whwre 列名 like ‘%キーワード%’
`%はlike演算子で使用できるワイルドカードの1種
例 `select * from product where name like ‘%軍艦%’;`