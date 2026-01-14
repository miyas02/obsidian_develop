**商品を追加するservlet**

カートに対する商品の追加処理は、次のような手順で行う

1. セッション属性からリストを取得
2. リストに商品Beanを追加する
3. セッション属性にリストを設定する

```java
(CartAdd.java)
package chapter17;

import bean.Product;
import tool.Page;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.List;
import java.util.ArrayList;
カートをリストで表現するので、ArrayListクラスをインポートする
import javax.servlet.ServletException;
import javax.servlet.http.*;
import javax.servlet.http.HttpSession;
import javax.servlet.annotation.WebServlet;

@WebServlet(urlPatterns={"/chapter17/cart-add"})
public class CartAdd extends HttpServlet {

	@SuppressWarnings("unchecked") doPostメソッドにSuppressWarningsアノテーションを付加する
	public void doPost (
		HttpServletRequest request, HttpServletResponse response
	) throws ServletException, IOException {
		PrintWriter out=response.getWriter();
		Page.header(out);

		String name=request.getParameter("name");
		int price=Integer.parseInt(request.getParameter("price"));

		HttpSession session=request.getSession(); 
		セッションがまだ開始されていない場合は、セッションを開始する。セッションがすでに開始されている場合は、既存のセッションを取得する

		List<Product> cart=(List<Product>)session.getAttribute("cart");
		セッション属性からcartの名を持つ属性を取得し、商品Beanのリストである変数cartに代入する

		if (cart==null) {
			cart=new ArrayList<Product>();
		}
		属性からリストが取得できない場合。
		getAttributeメソッドがnullを返したら、新規にからのリストを作成する

		Product p=new Product(); 
		Beanの作成とリストへの追加。
		商品を表すProductクラスのオブジェクトを作成し、リクエストパラメータから取得した商品名と価格を設定してリストに追加する。
		これが商品をカートに追加する処理になる
		p.setName(name);
		p.setPrice(price);
		cart.add(p);

		session.setAttribute("cart", cart); cartという名前にセッション属性にリストを設定する。setAttributeメソッドを使う

		out.println("カートに商品を追加しました。");
		Page.footer(out);
	}
}
```