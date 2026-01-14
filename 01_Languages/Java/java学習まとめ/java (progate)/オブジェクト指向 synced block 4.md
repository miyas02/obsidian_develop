### コンストラクタ

newを使ってインスタンスを呼び出した後に自動で呼び出される特別なメソッド
条件(1)コンストラクタ名はクラス名と同じにする
条件(2)戻り値を書いてはいけない

```java
Person.java
class Person {
	Person(){ 
		System.out.println("インスタンスが生成されました"); //インスタンスが生成された直後に実行
		}
}
```

```java
main.java
Person person1 = new Person();
```