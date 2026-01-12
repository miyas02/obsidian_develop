### データベースへの接続

**(1)initialContextオブジェクトを生成する**

データソースを取得するにはJNDIという仕組みを使う。
JNDIを使うには**「javax.naming.InitialContext」**クラスのオブジェクトを生成し、これを窓口にしてコネクションを取得する

**(2)DataSourceオブジェクトを取得する**

InitialContextクラスの**lookupメソッド**を使って、データソースを取得する。
取得したテータソースは、**「javax.sql.DataSource」**インターフェースのオブジェクトです。

**lookupメソッド
**`**Object lookup(Name name)**`

**(3)Connectionオブジェクトを取得する**

DataSourceインターフェースのgetConnectionメソッドを使って、Connectionオブジェクトを取得する