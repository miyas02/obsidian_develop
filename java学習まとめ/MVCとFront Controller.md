---

---
**Model
**アプリケーションに必要なデータを保持し、データに対する操作を行う。
JavaBeansを使用する。

**View
**モデルのデータを取得し、ユーザに対して表示します。ビューには主にJSPを使用する。

**Controller
**ユーザからの入力を受け取って、モデルの生成や設定を行う。ビューへのフォワードも行う。
servletを使う

### FrontControllerを適用したWebアプリケーション

Front Controllerパターンでは、機能ごとにサーブレットを作成する代わりに、**アクション**と呼ばれるJavaのクラスを作成します。
アクションはサーブレットではありません。Webアプリケーションの中で、サーブレットはフロントコントローラーだけです。

フロントコントらーらは全機能に共通の処理を行います。**リクエストの処理を行い、要求された機能に対応するアクションを生成して実行します。**

### すべてのアクションのスーパークラスである、Actionクラスを作成する

```java
Action.java
package tool;

import javax.servlet.http.*;

public abstract class Action { Actionクラスの宣言。Actionクラスは抽象メソッドのexecuteを含むので、抽象クラスになる
	public abstract String execute(
	execteメソッドは抽象メソッドにして具体的な処理は記述しない。
	Actionクラスのサブクラスにおいて、executeメソッドをオーバーライドして、各機能の処理を記述する
		HttpServletRequest request, HttpServletResponse response
	) throws Exception;
}
```

**フロントコントローラのクラス**

このフロントコントローラは、○○.actionのようなURLを受け取ると、chapter○○.○○Actionのようなパッケージ名.クラス名の形式に、文字列処理を使って加工する

そして、上記のクラスのインスタンスを生成し、executeメソッドを呼び出して、アクションを実行する。

アクションはjavaクラス。

```java
FrontController.java
package tool;

import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.*;
import javax.servlet.annotation.WebServlet;

@WebServlet(urlPatterns={"*.action"}) 
URLパターンの指定。フロントコントローラのサーブレットを、末尾が.actionで終わるURLに対応付ける。Search.actionのようなURLを開くと、フロントコントローラが実行される
public class FrontController extends HttpServlet {

	public void doPost(
		HttpServletRequest request, HttpServletResponse response
	) throws ServletException, IOException {
		PrintWriter out=response.getWriter();
		try {

			----パスを取得して加工する----
			String path=request.getServletPath().substring(1);
			pathの取得。getServletPathメソッドを使って、フロントコントローラが呼び出されたパスを取得する
			substringを使って先頭の1文字を削除する
			String name=path.replace(".a", "A").replace('/', '.');
			引き続きパスを加工する。.aをAに、/を.に置換する
			----------------------------------

				Action action=(Action)Class.forName(name).
				getDeclaredConstructor().newInstance();
						アクションの生成。アクションのクラス名を使ってインスタンスの作成

			String url=action.execute(request, response);
			アクションの実行
			request.getRequestDispatcher(url).
				forward(request, response);
				フォワードの実行。取得したフォワード先のURLを、forwardメソッドに渡して、指定されたフォワード先にフォワードする
		} catch (Exception e) {
			e.printStackTrace(out);
		}
	}

	public void doGet(
		HttpServletRequest request, HttpServletResponse response
	) throws ServletException, IOException {
		doPost(request, response);
	}
}
```

## 検索アクションの作成

データベースの検索を行うアクションを作成しましょう。Actionクラスのサブクラスとして、SerchActionクラスを宣言します。
そしてフロントコントローラを使って、SerchActionクラスのインスタンスを生成し、Executeメソッドを実行する。

### 入力用JSPファイルの作成

```java
serch.jsp
<%@page contentType="text/html; charset=UTF-8" %>
<%@include file="../header.html" %>

<p>検索キーワードを入力してください。</p>
<form action="Search.action" method="post">
<input type="text" name="keyword">
<input type="submit" value="検索">
</form>

<%@include file="../footer.html" %>
```

## 検索アクションの作成

```java
SerchAction.java
package chapter23;

import bean.Product;
import dao.ProductDAO;
import tool.Action;
import javax.servlet.http.*;
import java.util.List;

public class SearchAction extends Action {
	public String execute(
		HttpServletRequest request, HttpServletResponse response
	) throws Exception {

		String keyword=request.getParameter("keyword");
		serch.jspで入力された検索キーワードをリクエストパラメータから取得する。

		ProductDAO dao=new ProductDAO();
		List<Product> list=dao.search(keyword);
		検索の実行。productDAOクラスのserchメソッドを使う。

		request.setAttribute("list", list);
		検索を実行した結果をリクエスト属性に設定する。

		return "list.jsp";
		以下に作成するlist.jspを返す。

	}
}
```

## 出力用ファイルの作成

```java
<%@page contentType="text/html; charset=UTF-8" %>
<%@include file="../header.html" %>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

<c:forEach var="p" items="${list}">
	<p>${p.id}：${p.name}：${p.price}</p>
</c:forEach>

<%@include file="../footer.html" %>
```

## 追加アクションの作成

データベースにデータを追加するアクションを作成する

### 入力用のJSPファイルの作成

```java
<%@page contentType="text/html; charset=UTF-8" %>
<%@include file="../header.html" %>

<p>追加する商品を入力してください。</p>
<form action="Insert.action" method="post">
商品名<input type="text" name="name">
価格<input type="text" name="price">
<input type="submit" value="追加">
</form>

<%@include file="../footer.html" %>
```

### 追加アクションの作成

```java
package chapter23;

import bean.Product;
import dao.ProductDAO;
import tool.Action;
import javax.servlet.http.*;
import java.util.List;

public class InsertAction extends Action {
	public String execute(
		HttpServletRequest request, HttpServletResponse response
	) throws Exception {

		String name=request.getParameter("name");
		Integer price=Integer.parseInt(request.getParameter("price"));
		リクエストパラメータの取得

		Product p=new Product();
		p.setName(name);
		p.setPrice(price);
		ProductDAO dao=new ProductDAO();
		dao.insert(p);
		追加の実行。productDAOクラスのinsertメソッドを使う

		List<Product> list=dao.search("");
		request.setAttribute("list", list);
		リクエスト属性の設定。検索を実行した結果を、リクエスト属性に設定する。

		return "list.jsp";
		フォワード先のlist.jspを返す
	}
}
```