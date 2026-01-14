### コンストラクタに情報を渡す

newでインスタンスを作る際、`new クラス名( )`の( )には引数を渡すことができる
その引数は直後に呼び出されるコンストラクタに受け渡される

```java
Person.java
	class Person { 
		public String name; //インスタンスフィールド、ここに「Susuki」とセットしたい
		Person(String name){//コンストラクタ
			this.name = name; //this.nameはPersonクラスのインスタンスフィールド。nameは引数
			}
		}
```

```java
main.java
	Person person1 = new Person("Suzuki");//引数nameに「Suzuki」が渡される
```