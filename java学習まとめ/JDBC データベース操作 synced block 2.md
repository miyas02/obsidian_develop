### SQL文の作成と実行

SQL文の作成と実行には、**「java.sql.PreparedStatement」**インターフェースを使う。
preparedStatementオブジェクトを取得するには、**「Connection」**インターフェースの**「prepareStatement」**メソッドを使う。

**PrepareStatementメソッド**
`PreparedStatement prepareStatement (String sql)
`SQL文をデータベースに送るためのpreparedStatementオブジェクトを生成する
<u>引数にはSQL文を指定する</u>

**SQL文を実行するにはpreparedStatementインターフェースのexecuteQueryメソッドを使う**
`ResultSet executeQuery( )`
PreparedStatementオブジェクトのSQL文を実行し、結果をResultSetオブジェクトとして返す

SQL文を実行すると、結果が格納されたjava.sql.ResultSetインターフェースのオブジェクトが得られます