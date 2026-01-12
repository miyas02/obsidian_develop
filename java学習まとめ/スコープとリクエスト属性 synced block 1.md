### データベース処理とリクエスト属性

サーブレットがデータベースから取得したデータをJSPに渡して、JSPで表示するプログラムを作成する。
ここで作成するの、サーブレット側で商品テーブルのすべての行を読みだしてProductオブジェクトのリストとし、フォワード先のJSPファイルで表示するためのプログラム。

```java
(Attribute2.java)
package chapter16;

import bean.Product;
import dao.ProductDAO;
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.*;
import javax.servlet.annotation.WebServlet;
import java.util.List;

@WebServlet(urlPatterns={"/chapter16/attribute2"})
public class Attribute2 extends HttpServlet {

	public void doGet (
		HttpServletRequest request, HttpServletResponse response
	) throws ServletException, IOException {
		PrintWriter out=response.getWriter();
		try {
			ProductDAO dao=new ProductDAO(); 
			productDAOクラスを用いて、全商品の情報を取得する。
			productクラスのオブジェクトを作成してから、serchメソッドを呼び出す
			searchメソッドは、商品のリストをList<Product>型で返す
			List<Product> list=dao.search("");  ("")ですべての行を検索する

			request.setAttribute("list", list); setAttributeメソッドを用いて、リクエスト属性に商品のリストを設定する。属性名はlist

			request.getRequestDispatcher("attribute2.jsp") jspファイルにフォワードする
				.forward(request, response);
		} catch (Exception e) {
			e.printStackTrace(out);
		}
	}
}
```

```java
(attribute2.jsp)
<%@page contentType="text/html; charset=UTF-8" %>
<%@include file="../header.html" %>

<%@page import="bean.Product, java.util.List" %> pageディレクティブを使って、(bean.Productクラス)と、リストのインターフェースをインポート

<!-- 下記の@SuppressWarningsはEclipseの警告を消すための記述です。 -->
<%
@SuppressWarnings("unchecked")

List<Product> list=(List<Product>)request.getAttribute("list"); getAttributeメソッドを使って、サーブレットで設定した商品のリストを取得する。
%>

<% for (Product p : list) { %>
	<p><%=p.getId() %>：<%=p.getName() %>：<%=p.getPrice() %></p>
<% } %> 拡張for文を用いて、リストに含まれるすべての商品を表示する。


<%@include file="../footer.html" %>
```