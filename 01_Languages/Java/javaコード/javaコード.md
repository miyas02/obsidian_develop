---

---
<!-- Column 1 -->
# コレクション

![[javaコード synced block]]

![[javaコード synced block 1]]

### map

## 繰り返し

> [!note]+ **拡張for文**
> ```java
> String name[] = {"Suzuki", "Katou", "Yamada"};
> 
> for (String str: name){
>   System.out.println(str);
> 			}
> //3回繰り返す
> ```

### 変換

> [!note]+ Switch
> ```java
> int num = 2;
> 
> switch(num){
>   case 1:
>     System.out.println("Aクラス");
>     break;
>   case 2:
>     System.out.println("Bクラス");
>     break;
>   case 3:
>     System.out.println("Cクラス");
> }
> ```

> [!note]+ [substring](https://java-code.jp/795)
> ```java
> string変数.substring(0,1); //(begin + 1文字目 , end文字目)
> ```
> 

> [!note]+ contains 含まれていたら(true)判定
> xにaが含まれていたら(true)判定
> 
> ```java
> 変数x.contains(a);
> ```

> [!note]+ char変数のscannerでの書き込み
> ```java
> char a = sc.next().charAt(0);
> ```

> [!note]+ ”==”と”equals”の違い
> intは “==” Stringは”equals”
> 値が同じであっても**参照先が違う**ので"=="で比較した場合は"false"を返す
> 

> [!note]+ nextLine( )の挙動
> 改行も入力したと判定される
> 
> ```java
> int n = sc.nextInt();
> sc.nextLine(); //ブランクを設ける
> String s = sc.nextLine();
> ```

> [!note]+ break
> forループを抜けるにはbreakを使う
> 二重ループはラベル付きbreakを使う

> [!note]+ containsKey( )
> [指定したKeyが存在するか確認し、存在したらtrueを返す](https://www.sejuku.net/blog/19172)

> [!note]+ keySet
> [link](https://flytech.work/blog/10007/)

> [!note]+ [挿入ソート](https://paiza.jp/works/mondai/sort_naive/sort_naive__insertion/edit?language_uid=java)
> データ列を「整列済み」と「未整列」の2つに分け、「未整列な配列」からデータを1つ取り出し、「整列済み配列」の適切な位置に挿入することを繰り返す手法です

> [!note]+ LABEL 多重ループを抜ける
> ```java
> LABEL01:{
>   for(int i=1;i<=5;i++) {
>     for(int j=1;j<=5; j++) {
>       System.out.println(i + "*" + j + "=" + (i*j));
>       if(j>3) break LABEL01;
>     }
>   }
> }
> 
> ```
> 

> [!note]+ メソッド
> ```java
> static int x,y //変数の定義
> public static void メソッド名 (返り値){
> 
> }
> ```

> [!note]+ 例外処理
> ```java
> try {
>     
>     // 例外が発生する可能性のある処理
> 
> } catch(Exception e) {
> 
>     // エラーが発生した場合の処理
> 
> } finally {
> 
>     // 必ず最後に実行する処理
> 
> }
> ```

> [!note]+ StringBuilder
> [https://www.javadrive.jp/start/stringbuilder/index3.html](https://www.javadrive.jp/start/stringbuilder/index3.html)
> 
> 複数の文字列や他の値を連結して、最後に文字列として出力すること。
> 
> インスタンスの作成
> `StringBuilder sb = new StringBuilder();`
> 
> 

> [!note]+ べき乗
> シフト

> [!note]+ Fileの作成
> ```java
> File file = new File("ファイルパス");
> 
> try(FileWriter writer = new FileWriter(file)){
> 	String content = "書き込む内容"
> 	writer.write(content);
> }catch(IOException e){
> 	e.printStackTrace();
> }
> ```

[[Archive]]

<!-- Column 2 -->
### set

