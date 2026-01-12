---

---
> [!note]+ デフォルト引数
> ```java
> 店内の情報管理
> 各客の会計を求める
> 酒を頼んだら以降の食事が200円引き
> ビールは500円で[0]とだけ表記
> 
> 
> 未成年のお客さんを表すクラスを宣言したのち、そのクラスを継承して成年済みのお客さんのクラスを宣言する
> 
> import java.util.ArrayList;
> import java.util.Scanner;
> 
> class Customer { 未成年の客
> 
>     int amount; 会計
> 
>     Customer() { 初期値0
>         amount = 0;
>     }
> 
>     void takeFood(int m) { 食事代
>         amount += m;
>     }
> 
>     void takeSoftDrink(int m) {
>         amount += m;
>     }
> 
>     void takeAlcohol(int m) {}
> 
>     void takeAlcohol() {
>         takeAlcohol(500);
>     }
> }
> 
> class Adult extends Customer { 成年客
> 
>     boolean drunk;
> 
>     Adult() {
>         super();
>         drunk = false; 初期値
>     }
> 
>     void takeFood(int m) {
>         if (drunk) {
>             m -= 200;
>         }
>         super.takeFood(m);
>     }
> 
>     void takeAlcohol(int m) {
>         drunk = true;
>         amount += m;
>     }
> }
> 
> public class Main {
> 
>     public static void main(String[] args) {
>         Scanner sc = new Scanner(System.in);
> 
>         int n = sc.nextInt(); 客の人数
>         int k = sc.nextInt(); 注文数
> 
>         ArrayList<Customer> customers = new ArrayList<>(); Customerクラスのリストcustomer
> 
>         for (int i = 0; i < n; i++) {
>             int age = sc.nextInt(); 年齢
>             if (age < 20) {
>                 customers.add(new Customer()); 年齢が20以下ならcudtomeクラスのインスタンスを作成
>             } else {
>                 customers.add(new Adult());　20以上ならadaultクラスのインスタンスを作成
>             }
>         }
> 
>         for (int i = 0; i < k; i++) {
>             int index = sc.nextInt() - 1; 客の番号
>             String s = sc.next(); 注文内容
> 
>             if (s.equals("0")) { 注文がビール
>                 customers.get(index).takeAlcohol();
>             } else {
>                 int m = sc.nextInt();
>                 if (s.equals("food")) {
>                     customers.get(index).takeFood(m);
>                 } else if (s.equals("softdrink")) {
>                     customers.get(index).takeSoftDrink(m);
>                 } else if (s.equals("alcohol")) {
>                     customers.get(index).takeAlcohol(m);
>                 }
>             }
>         }
> 
>         for (Customer customer : customers) {
>             System.out.println(customer.amount);
>         }
>     }
> }
> ```

> [!note]+ 静的メンバ
> ```java
> import java.util.*;
> public class Main {
>     class Customer {
>         int amount; //会計
> 				int age;
> 				
>         
>     }
>     
>     public static void main(String[] args) {
>         // 自分の得意な言語で
>         // Let's チャレンジ！！
>         Scanner sc = new Scanner(System.in);
>         
>     }
> }
> ```

> [!note]+ 迷路
> 各地点に置かれた文字をつなげる
> 各地点２つのルートがある
> 
> ```java
> import java.util.Scanner;
> 
> class Point { 各地点のクラス
> 
>     char a; 各地点の文字
>     int[] road; ルート1とルート2が格納された配列
> 
>     Point(char a, int r1, int r2) {　コンストラクタ
>         this.a = a;
>         this.road = new int[] { r1, r2 };
>     }
> }
> 
> public class Main {
> 
>     public static void main(String[] args) {
>         Scanner sc = new Scanner(System.in);
> 
>         int n = sc.nextInt(); 地点の数
>         int k = sc.nextInt(); 移動の回数
>         int s = sc.nextInt(); 開始する点s
> 
>         Point[] points = new Point[n + 1]; Pointクラスの配列を作成
>         for (int i = 1; i <= n; i++) {
>             String a = sc.next();　地点の文字
>             int r1 = sc.nextInt();
>             int r2 = sc.nextInt();
>             points[i] = new Point(a.charAt(0), r1, r2); point配列(i)でインスタンスを作成
>         }
> 
>         int now = s; 開始地点
>         StringBuilder sb = new StringBuilder(); Stringbuilderインスタンスの作成
>         sb.append(points[now].a); stringbuilderに開始地点の文字を追加
> 
>         for (int i = 0; i < k; i++) {
>             int m = sc.nextInt() - 1; 選んだ道
>             now = points[now].road[m]; pointクラスのnowの番号の選んだ道の値がnowになる
>             sb.append(points[now].a); stringbulderにpointクラスのnowの文字を追加
>         }
> 
>         System.out.println(sb.toString());
>     }
> }
> ```
> 

> [!note]+ 格闘ゲーム
> 各プレイヤーは決まったhpと３種類の技を持っていて、技には強化系と攻撃の技があり、各攻撃技には発生フレームfとダメージaが設定されている
> •hpが0になるとプレイヤーは退場
> •プレイヤー１がプレイヤー２を技１で攻撃した場合、攻撃を受けたプレイヤー２は反撃の技２を選ぶ。
> •どちらか片方でも退場していたらなにも起こらない。
> •強化系の技を使った場合技の発生フレームを-3,攻撃力を+5する
> 残っていたプレイヤーの数を答える
> 
> ```java
> import java.util.Scanner;
> 
> class Player {　プレイヤーのクラス
> 
>     int hp; 
>     int[] f, a; 技のフレームと攻撃力は配列にする
> 
>     Player(int hp, int f1, int a1, int f2, int a2, int f3, int a3) { プレイヤーくらすのコンストラクタ
>         this.hp = hp;
>         this.f = new int[] { f1, f2, f3 }; フレームの配列
>         this.a = new int[] { a1, a2, a3 }; 攻撃力の配列
>     }
> 
>     void enhance() { 強化するメソッド
>         for (int i = 0; i < 3; i++) { ３回繰り返す
>             if (a[i] == 0 && f[i] == 0) { 攻撃力とフレームどっちもが0なら繰り返しを飛ばす
>                 continue;
>             }
> 
>             a[i] += 5; 条件が真なら、その技の攻撃力+5
>             f[i] = Math.max(f[i] - 3, 1); 
>         }
>     }
> 
>     boolean isAlive() { 生きているかどうか
>         return hp > 0;
>     }
> 
>     void takeDamage(int d) { ダメージを与えりメソッド
>         hp -= d;
>     }
> }
> 
> public class Main {
> 
>     public static void main(String[] args) {
>         Scanner sc = new Scanner(System.in);
> 
>         int n = sc.nextInt();　プレイヤーの人数
>         int k = sc.nextInt(); 攻撃回数
> 
>         Player[] players = new Player[n];
>         for (int i = 0; i < n; i++) {
>             int hp = sc.nextInt();
>             int f1 = sc.nextInt();
>             int a1 = sc.nextInt();
>             int f2 = sc.nextInt();
>             int a2 = sc.nextInt();
>             int f3 = sc.nextInt();
>             int a3 = sc.nextInt();
>             players[i] = new Player(hp, f1, a1, f2, a2, f3, a3); 
> 						プレイヤーのインスタンスの作成とコンストラクタへの代入 
> 						配列への格納
>         }
> 
>         for (int i = 0; i < k; i++) {　攻撃のやりとり
>             int p1Index = sc.nextInt() - 1; 攻撃者p1
>             int t1 = sc.nextInt() - 1; 攻撃技t1
>             int p2Index = sc.nextInt() - 1;
>             int t2 = sc.nextInt() - 1;
> 
>             Player p1 = players[p1Index]; 戦うプレイヤーp1をplayers配列から代入
>             Player p2 = players[p2Index];
> 
>             if (!p1.isAlive() || !p2.isAlive()) {　両方死んでいたら以降の作業を飛ばす
>                 continue;
>             }
> 
>             int f1 = p1.f[t1];　配列の番号で呼び出す
>             int a1 = p1.a[t1];
>             int f2 = p2.f[t2];
>             int a2 = p2.a[t2];
>             boolean p1Enhance = a1 == 0 && f1 == 0; 攻撃力とフレームの両方が0なら真
>             boolean p2Enhance = a2 == 0 && f2 == 0;
> 
>             if (p1Enhance && p2Enhance) { エンハンスが真だからenhanceメソッドの実行
>                 p1.enhance();
>                 p2.enhance();
>             } else if (p1Enhance) {
>                 p1.enhance();
>                 p1.takeDamage(a2);
>             } else if (p2Enhance) {
>                 p2.takeDamage(a1);
>                 p2.enhance();
>             } else {
>                 if (f1 < f2) { 両方攻撃技の分岐
>                     p2.takeDamage(a1); takedamageメソッドの実行
>                 } else if (f1 > f2) {
>                     p1.takeDamage(a2);
>                 }
>             }
>         }
> 
>         int numOfAlivePlayers = 0;
>         for (Player player : players) {
>             if (player.isAlive()) {
>                 numOfAlivePlayers++; 生き残りのカウント
>             }
>         }
> 
>         System.out.println(numOfAlivePlayers);
>     }
> }
> ```