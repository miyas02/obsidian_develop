**プレースホルダ**

SQL文の中に記述された「?」のこと。後から値を設定できる。
’%キーワード%’の部分に「?」を記述しておき、リクエストパラメータから取得したキーワードを後から置き換える。
プレースホルダに値を設定するには、PreparedStatementクラスのsetStringメソッドやsetIntメソッドを使う。

**setStringメソッド**
`void setString(int parameterindex, String x)`
第2引数で指定された文字列を、第1引数で指定された番号のプレースホルダに設定します