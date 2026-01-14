**サーブレットで生成したProducutオブジェクトをリクエストオブジェクトに保存し、フォワード先のJSPファイルで表示するもの**

```java
(Attribute.java)
package chapter16;

import bean.Product;
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.*;
import javax.servlet.annotation.WebServlet;

@WebServlet(urlPatterns={"/chapter16/attribute"})
public class Attribute extends HttpServlet {

	public void doGet (
		HttpServletRequest request, HttpServletResponse response
	) throws ServletException, IOException {
		Product p=new Product(); 商品のBean(Productオブジェクト)を生成。
		p.setId(1);
		p.setName("まぐろ");
		p.setPrice(100);
		
		request.setAttribute("product", p); setAttributeメソッドを用いて、リクエスト属性にBeanを設定する。属性名はproductとする

		request.getRequestDispatcher("attribute.jsp") JSPファイルにフォワードする
			.forward(request, response);
	}
}
```

```java
attribute.jsp
<%@page contentType="text/html; charset=UTF-8" %>
<%@include file="../header.html" %>

<%@page import="bean.Product" %> bean.productクラスのインポート。BeanのクラスをJSPで使用するため、pageディレクティブを使う

<% Product p=(Product)request.getAttribute("product"); %> 
スクリプレットでgetAttributeメソッドを使って、サーブレットで設定した商品のBeanを取得する
getAttributeメソッドの戻り値はObject型なので、Product型に変換する

<%=p.getId() %>：<%=p.getName() %>：<%=p.getPrice() %><br>
JSPの式を用いて、「商品番号：商品名：価格」の形式で商品番号を表示する。
取得したBeanに対してゲッタを呼び出し、商品情報を取得する

<%@include file="../footer.html" %>
```