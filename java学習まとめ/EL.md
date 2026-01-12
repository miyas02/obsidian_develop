---

---
ELはExpression Languageの略で、式言語という意味。
ELを使うと、Beanやプロパティを取得する処理を、**非常に簡潔に記述できる。**

## ELによるBeanとプロパティの取得

### EL式の記法

EL式は、JSPの式(<%=…%>)のように、値をWebページに出力するために使うことができる。

EL式は、${~}のように記述する。~の部分にはいろいろな式を記述することができるが、式の結果がどうあれ、==**最終的に出力されるのはString型の値**==である。
String型以外のオブジェクトはtoStringメソッドでString型に変換され、intやcharなどのプリミティブ型も文字列に変換される。

また、==**式を評価する際に、数値として評価可能な文字列は、数値演算の対象にすることができない**==。
たとえば”123”のような文字列に対して、+のような演算子を適用すると、自動的に数値に変換されて演算が行われる。文字列が属性、プロパティ、変数に格納されている場合も同様である。

**Beanのプロパティを取得する方法**

${属性名.プロパティ名}

属性にBeanが設定されているときに、上記のように記述するだけで、属性に設定されたBeanを取得し、さらにBeanのプロパティを取得することができる。
特にBeanのクラス名についてまったく記述しないで済むことに注目する。
他の方法と比較する

- スクリプレットと式を使う場合

Beanクラスのインポート、Beanを取得する処理、ゲッターを使ったプロパティを取得する処理や型変換が必要である。

- アクションタグを使う場合

useBeanアクションとgetPropertyアクションが必要。useBeanアクションではBeanのクラスを明示する必要がある。

### ELとスコープ

ELは、基本的に各スコープに保存された属性やリクエストパラメータを処理するためのものなので、これらを扱うための便利な機能がある。

ELは属性の内容がBeanかどうかに関わらず、スコープの指定を省略することができる。これは、Bean名に一致する属性名を、複数のスコープから自動的に検索するため。
ELは次のような順序で属性名を探す。

1. ページ属性
2. リクエスト属性
3. セッション属性
4. アプリケーション属性

属性名が見つかったら、属性に設定された値(Bean)を取得し、EL式に記述されたプロパティなどを出力する。

### ELを使ったプログラム

ELを使ってBeanを取得するプログラム
サーブレット側が生成したBeanをリクエスト属性に保存し、フォワード先のJSPから取得して表示する。

**JSPにフォワードするサーブレット**

```java
package chapter21;

import bean.Product;
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.*;
import javax.servlet.annotation.WebServlet;

@WebServlet(urlPatterns={"/chapter21/el"})
public class EL extends HttpServlet {

	public void doGet (
		HttpServletRequest request, HttpServletResponse response
	) throws ServletException, IOException {

		Product p=new Product();
		p.setId(1);
		p.setName("まぐろ");
		p.setPrice(100);

		request.setAttribute("product", p);

		request.getRequestDispatcher("el.jsp")
			.forward(request, response);
	}
}
```

**フォワード先のJSP**

```java
<%@page contentType="text/html; charset=UTF-8" %>
<%@include file="../header.html" %>

<p>${product.id}：${product.name}：${product.price}</p>
ELを使ったプロパティの取得。
サーブレットがリクエスト属性に設定したBeanの属性名はproductなので、EL式のBean名にはproductを指定する。
スコープの指定は記述していない

<%@include file="../footer.html" %>
```

## ELによるメソッドの呼び出し

次のように記述すると、任意のクラスのインスタンスメソッドを呼び出すことができる。

`${オブジェクト.メソッド(引数)}`

`${クラス.メソッド(引数)}`

### メソッドを呼び出すプログラム

ELによるメソッド呼び出しを使って、乱数を生成して出力するプログラム

```java
<%@page contentType="text/html; charset=UTF-8" %>
<%@include file="../header.html" %>

${Math.random()}
ELを用いて、java.lang.Mathクラスのrandomメソッドを呼び出す

<%@include file="../footer.html" %>
```