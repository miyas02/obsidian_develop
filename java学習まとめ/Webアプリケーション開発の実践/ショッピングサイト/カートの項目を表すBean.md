---

---
商品Beanが持つ情報に加えて、個数の情報が必要になる。

商品Beanと個数をプロパティにもつ、新しいBeanを作成する
項目Bean(Itemクラス)を作成する

### 項目Beanの作成

```java
Item.java
package bean;

public class Item implements java.io.Serializable {

	private Product product;
	Productオブジェクトをプロパティとしてもたせる。
	private int count;
	
	public Product getProduct() {
		return product;
	}
	public int getCount() {
		return count;
	}

	public void setProduct(Product product) {
		this.product=product;
	}
	public void setCount(int count) {
		this.count=count;
	}
}
```