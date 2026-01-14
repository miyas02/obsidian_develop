---

---
![[Java Beans synced block]]

![[Java Beans synced block 1]]

```java
package chapter15;

import bean.Product; //Productクラスのインポート
import tool.Page;
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.*;
import javax.servlet.annotation.WebServlet;

@WebServlet(urlPatterns={"/chapter15/bean"})
public class Bean extends HttpServlet {

	public void doGet (
		HttpServletRequest request, HttpServletResponse response
	) throws ServletException, IOException {
		PrintWriter out=response.getWriter();
		Page.header(out);

		Product p=new Product();//productクラスのオブジェクトを生成

		p.setId(1);//setterで値の設定
		p.setName("まぐろ");
		p.setPrice(100);

		out.println(p.getId());//getterで値の取得
		out.println("：");
		out.println(p.getName());
		out.println("：");
		out.println(p.getPrice());

		Page.footer(out);
	}
}
```

### DAO

Data Access Object とは、データベースを操作する処理をまとめたクラス

```java
(DAO.java)データベースへの接続を行うDAOクラス
package dao;

import javax.naming.InitialContext;
import javax.sql.DataSource;
import java.sql.Connection;

public class DAO {
	static DataSource ds; データソースを保存する変数ds

	public Connection getConnection() throws Exception {
		データベースへの接続を取得するためのgetconnectionメソッド
		DAOクラスのサブクラスは、このgetConnectionメソッドを使って、データベースへの接続を取得する
		if (ds==null) {
			InitialContext ic=new InitialContext(); データソースの初期化、最初の1回だけデータソースを取得
			ds=(DataSource)ic.lookup("java:/comp/env/jdbc/book");
		}
		return ds.getConnection(); 接続の取得
	}
}
```

```java
(productDAO.java)商品の検索と商品の追加を行う機能
package dao;

import bean.Product;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.List;
import java.util.ArrayList;

public class ProductDAO extends DAO { DAOクラスのサブクラスとして宣言

	public List<Product> search(String keyword) throws Exception { 商品の検索を行うサーチメソッド
																																例外はserchメソッドの呼び出しもとで処理するからthorow e宣言

		List<Product> list=new ArrayList<>(); 
		ResultSetオブジェクトはデータベースから切断されるときに破棄される
		そのため、結果をリストに詰め替えて返す、商品情報はproductクラスに保存するからproduct型

		Connection con=getConnection(); DAOクラスのgetConnectionメソッドを使って、データベースに接続

		PreparedStatement st=con.prepareStatement( SQL文の実行
			"select * from product where name like ?");
		st.setString(1, "%"+keyword+"%");
		ResultSet rs=st.executeQuery();

		while (rs.next()) { 検索結果から1行ずつ取り出し、各列の値を、新たに生成したProductオブジェクトのプロパティにsetterを使って書き込んでいます
			Product p=new Product();
			p.setId(rs.getInt("id"));
			p.setName(rs.getString("name"));
			p.setPrice(rs.getInt("price"));
			list.add(p);　リストに追加している
		}

		st.close();//SQLの終了
		con.close();

		return list; サーチメソッドの呼び出しもとに返す
	}

	public int insert(Product product) throws Exception { 追加を行うinsertメソッド
		Connection con=getConnection();

		PreparedStatement st=con.prepareStatement( SQL文の実行
			"insert into product values(null, ?, ?)"); データの追加を行うためのinsert文
		st.setString(1, product.getName()); 引数として受け取ったproductオブジェクトがらgetterを使ってname列とprice列の値を取り出せて、setString,setIntメソッドの第2引数に指定
		st.setInt(2, product.getPrice());
		int line=st.executeUpdate(); 作成したSQLの実行

		st.close();
		con.close();
		return line; insertメソッドの戻り値
	}
}
```

### 商品検索サーブレットの作成

```java
Search.java
package chapter15;

import bean.Product;
import dao.ProductDAO; productDAOのインポート
import tool.Page;
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.*;
import javax.servlet.annotation.WebServlet;
import java.util.List;

@WebServlet(urlPatterns={"/chapter15/search"})
public class Search extends HttpServlet {

	public void doPost (
		HttpServletRequest request, HttpServletResponse response
	) throws ServletException, IOException {
		PrintWriter out=response.getWriter();
		Page.header(out);
		try {
			String keyword=request.getParameter("keyword");

			ProductDAO dao=new ProductDAO(); productDAOのオブジェクトを生成
			List<Product> list=dao.search(keyword); サーチメソッドはリストを返すので、それを変数に取得している

			for (Product p : list) { 結果の表示
				out.println(p.getId());
				out.println("：");
				out.println(p.getName());
				out.println("：");
				out.println(p.getPrice());
				out.println("<br>");
			}

		} catch (Exception e) {
			e.printStackTrace(out);
		}
		Page.footer(out);
	}
}
```

### 商品追加のサーブレットの作成

```java
package chapter15;

import bean.Product;
import dao.ProductDAO;
import tool.Page;
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.*;
import javax.servlet.annotation.WebServlet;

@WebServlet(urlPatterns={"/chapter15/insert"})
public class Insert extends HttpServlet {

	public void doPost (
		HttpServletRequest request, HttpServletResponse response
	) throws ServletException, IOException {
		PrintWriter out=response.getWriter();
		Page.header(out);
		try {
			String name=request.getParameter("name"); リクエストから受け取った値を取得
			int price=Integer.parseInt(request.getParameter("price"));

			Product p=new Product(); データベースに追加するデータを保持するproductオブジェクトを作成
			p.setName(name); リクエストパラメータから取得した商品名と価格を、セッタを使って書き込んでいる
			p.setPrice(price);

			ProductDAO dao=new ProductDAO(); productDAOのオブジェクトを生成
			int line=dao.insert(p); productDAOのinsertメソッドを実行
			

			if (line>0) { 処理した行数が1行以上のとき記述
				out.println("追加に成功しました。");
			}
		} catch (Exception e) {
			e.printStackTrace(out);
		}
		Page.footer(out);
	}
}
```