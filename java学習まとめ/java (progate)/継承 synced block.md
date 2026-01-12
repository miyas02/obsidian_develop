既存のクラスのフィールドやメソッドを別のクラスに引き継ぐことを**「継承」**という。
継承されるクラスを**「スーパークラス」**、継承してできる新しいクラスを**「サブクラス」**と呼ぶ。

継承を用いて新しくサブクラスを定義するときは、**「extends」**を用いて
`「class サブクラス名 extends スーパークラス名」`としてクラスを定義する。

サブクラスはスーパークラスのフィールドとメソッドを引き継いでいる。
よってcarクラスにはまだなにも定義されいていないが、carクラスのインスタンスに対して、Vehicleクラスのインスタンスメソッドを呼び出すことができえる

```java
Vehicle.java
class Vehicle {
	public  void setName(String name) {
	}
	public void setColor(String color) {
	}
}
```

```java
Car.java
class Car extends Vehicle { Vehicleクラスのインスタンスメソッドを引き継いでいる
} 
```

```java
Main.java
class Main {
	public static void main(String[] args){
		Car car  = new Car();
		car.setName("フェラーリ");
		car.setColor("赤"); スーパークラスのインスタンスメソッドを呼び出す
```