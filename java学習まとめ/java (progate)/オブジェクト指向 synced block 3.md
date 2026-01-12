### this

インスタンスメソッド内でインスタンスフィールドの値を使用する場合、「this」という特殊な変数を使う

```java
Person.java
class Person {
	public String name;//これをthisで指定
	public void hello(){
		System.out.println("こんにちは、私の名前は"+this.name+"です")
		//Personクラスのインスタンスフィールドの変数nameを使用する
	}
}
```

```java
main.java
Person person1 = new Person();
person1.name = "Suzuki";
person1.hello();
```