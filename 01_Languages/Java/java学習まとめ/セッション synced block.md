**セッションを使ったアクセスカウンタアプリ
同じユーザからのみカウントアップする**

```java
package chapter17;

import tool.Page;
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.*;
import javax.servlet.http.HttpSession; HttpSessionインターフェースをインポートする
import javax.servlet.annotation.WebServlet;

@WebServlet(urlPatterns={"/chapter17/count"})
public class Count extends HttpServlet {

	public void doGet (
		HttpServletRequest request, HttpServletResponse response
	) throws ServletException, IOException {
		PrintWriter out=response.getWriter();
		Page.header(out);

		HttpSession session=request.getSession(); 
		getSessionメソッドを使って、HttpSessionオブジェクトを取得する
		セッションがまだ開始されていない場合は、セッションを開始する。セッションがすでに開始されている場合は、既存のセッションを取得する

		Integer count=(Integer)session.getAttribute("count");
		セッション属性からカウンタの値を取得している。
		getAttributeメソッドの戻り値はObject型なので、Integer型に変換する

		if (count==null) count=0; 
		countがnullの場合は最初のアクセスということになる。nullなら0を代入
		
		count++; カウンタを+1する
		session.setAttribute("count", count); setAttributeメソッドで再びセッション属性に保存する

		out.println("<p>"+count+"</p>");
		out.println("<p>"+session.getId()+"</p>");
		現在のcountの値とともに、getIdメソッドを使ってセッションIDを取得し、ページに表示する

		Page.footer(out);
	}
}
```