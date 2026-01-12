---

---
![[JDBC データベース操作 synced block]]

![[JDBC データベース操作 synced block 1]]

![[JDBC データベース操作 synced block 2]]

![[JDBC データベース操作 synced block 3]]

```java
~省略
import javax.naming.InitialContext; データベースを使用するためのインターフェースやクラス
import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;

@WebServlet(urlPatterns={"/chapter14/all"})
public class All extends HttpServlet {
	public void doGet (
		HttpServletRequest request, HttpServletResponse response
	) throws ServletException, IOException {
		PrintWriter out=response.getWriter();
		Page.header(out);
		try {
			InitialContext ic=new InitialContext(); InitialContextの取得
			DataSource ds=(DataSource)ic.lookup( データベースへ接続するためのDataSourceオブジェクトの取得。引数にはリソース名
				"java:/comp/env/jdbc/book");
			Connection con=ds.getConnection(); DataSourceクラスのgetConnectionメソッドでコネクションの取得。データベースに接続する。

			PreparedStatement st=con.prepareStatement( SQL文を実行するためのPreparedStatementオブジェクトを取得する
			"select * from product");
			ResultSet rs=st.executeQuery(); 作成したSQL文は、executeQueryメソッドで実行し、結果をResultSetオブジェクトとして取得する

			while (rs.next()) { nextメソッド。次の行が存在したらtrueを返して移動して、なければfalseを返す。
				out.println(rs.getInt("id"));　getIntメソッド。現在のカーソル行にある指定された列の値を、文字列として取得。引数には列名を指定
				out.println("：");
				out.println(rs.getString("name"));
				out.println("：");
				out.println(rs.getInt("price"));
				out.println("<br>");
			}

			st.close(); データベースからの切断
			con.close();
		} catch (Exception e) {
			e.printStackTrace(out); エラーメッセージの表示
		}
		Page.footer(out);
	}
}
```

### 商品を検索するサーブレット

![[JDBC データベース操作 synced block 4]]

![[JDBC データベース操作 synced block 5]]

```java
~省略

@WebServlet(urlPatterns={"/chapter14/search"})
public class Search extends HttpServlet {
	public void doPost (
		HttpServletRequest request, HttpServletResponse response
	) throws ServletException, IOException {
		PrintWriter out=response.getWriter();
		Page.header(out);
		try {
			InitialContext ic=new InitialContext();
			DataSource ds=(DataSource)ic.lookup(
				"java:/comp/env/jdbc/book");
			Connection con=ds.getConnection();

			String keyword=request.getParameter("keyword"); リクエストパラメータの取得

			PreparedStatement st=con.prepareStatement( PreparedStatmentメソッドでSQL文を作成
				"select * from product where name like ?"); like演算子の後にプレースホルダを記述
			st.setString(1, "%"+keyword+"%"); setStringメソッドでプレースホルダを検索文字列に置き換えている
　　　　　　　　　　　　　　　　　　　　プレースホルダは1つしかないので、第1引数は「１」になる。
			ResultSet rs=st.executeQuery(); 検索を実行し、結果の値を取得する

			while (rs.next()) {
				out.println(rs.getInt("id"));
				out.println("：");
				out.println(rs.getString("name"));
				out.println("：");
				out.println(rs.getInt("price"));
				out.println("<br>");
			}

			st.close();
			con.close();
		} catch (Exception e) {
			e.printStackTrace(out);
		}
		Page.footer(out);
	}
}
```

### 商品を登録するサーブレット

検索のSQL文は`executeQuery`メソッドで実行するが、
追加、更新、削除など、データベースの内容を変更するSQL文には`executeUpdate`メソッドで実行する

追加、更新、削除の場合、処理が成功したかどうか知ることができない。これを確認したい場合は、executeUpdateメソッドの戻り値を使う。
executeUpdateメソッドは、発行したSQL文により変更された行を返す。何も処理が行われなければ「０」を返す。
次のようにして、変更に成功した場合の処理を記述できる。

`int Line = st.executeUpdate( );
if (LIne > D) {
変更に成功した時の処理
}`

![[JDBC データベース操作 synced block 6]]

```java
~省略
@WebServlet(urlPatterns={"/chapter14/insert"})
public class Insert extends HttpServlet {

	public void doPost (
		HttpServletRequest request, HttpServletResponse response
	) throws ServletException, IOException {
		PrintWriter out=response.getWriter();
		Page.header(out);
		try {
			InitialContext ic=new InitialContext();
			DataSource ds=(DataSource)ic.lookup(
				"java:/comp/env/jdbc/book");
			Connection con=ds.getConnection();
			
			String name=request.getParameter("name");
			int price=Integer.parseInt(request.getParameter("price"));
			
			PreparedStatement st=con.prepareStatement(
				"insert into product values(null, ?, ?)"); SQL文にはプレースホルダが2つある
			st.setString(1, name); リクエストパラメータから取得した値でプレースホルダを置き換えている
			st.setInt(2, price);
			int line=st.executeUpdate(); executeUpdateメソッドを実行して処理した行数を取得する
			
			if (line>0) { 処理した行数が1以上のときの記述
				out.println("追加に成功しました。");
			}
			
			st.close();
			con.close();
		} catch (Exception e) {
			e.printStackTrace(out);
		}
		Page.footer(out);
	}
}
```
