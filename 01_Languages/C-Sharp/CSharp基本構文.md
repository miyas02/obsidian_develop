---

---
# 基本構文
## データ型
### Enum
内部的に0から始まる数字を持つ
```C#
public enum Colors {
 Red, //0
 Yellow, //1
 Blue, //2
 }
var red = Colors.Red.ToString(); // 文字列で"Red"を返す
int red = (int)Color.Red; // int型で0を返す
Enum.GetValues(typeof(Colors)) //要素の値をすべて取得
Enum.GetNames(typeof(Colors)) //要素の名前をすべて取得
```
### デリゲード
## メンバ
### プロパティ
**フィールド **データそのものを保持する「箱」
**プロパティ **その「箱」にアクセスするための「入り口（インターフェース）」
インターフェースはプロパティのみ実装可能
```c#
//プロパティ
public int Number {
    get { return _number; }
    set { _number = value; } //valueに右辺の10が入る
}
Number = 10; //setメソッドを実行
```

```c#
private int age; //フィールド
public int Age { get; set; }  //自動プロパティ. コンパイラが裏でフィールドを作ってくれる
```
### アクセサ
**init**
プロパティのsetの代わりにinitと記述すると、コンストラクタのみで値を設定可能(後続での値設定不可)
```c#
public int Age {get; init} //
```
### バッキングフィールド
プロパティの「中身を実際に保持している変数」 のこと。プロパティは窓口。
```c#
private int _age;  // ← バッキングフィールド（実体）
public int Age     // ← プロパティ（窓口）
{
    get { return _age; }  // バッキングフィールドの値を返す
    set //値変更時に処理を強制
		{
	    if (value < 0) value = 0; // マイナスは入れない
	    _age = value;
		}
}
Age = 20; //setメソッドを実行
```
### event
デリゲート専用のメンバ
イベント用に制限を加えたプロパティ
## 非同期処理
```
```
### Task~Result~をResultに変換
```
using var client = new HttpClient();
string ret = await client.GetStringAsync(url); //awaitを使用 非同期処理用
Task<string> tsk = client.GetStringAsync(url);
string task_result = tsk.Result //Task.Resultを使用 同期処理用
```
## その他の構文
### 文字列

#### 文字列補間
```c#
//$を先頭に付けて、変数を文字列に埋め込む
string name = "taro";
string msg1 = $"hello {name} world";
//式も可
int a = 10;
int b = 20;
string msg = $"合計は {a + b} です。";
```
#### 逐語的文字列リテラル
`@"..."` と書くと、エスケープシーケンスを無視して、そのままの文字列として扱う
```c#
// 通常の文字列
string path1 = "C:\\Users\\miyagawa\\Documents\\file.txt";
// 逐語的文字列
string path2 = @"C:\Users\miyagawa\Documents\file.txt";
//改行そのまま書ける
string text = @"こんにちは。
これは複数行の
文字列です。";
//文字列補完($)と同時使用可
string name = "taro";
string text2 = $@"C:\Users\{name}\Documents\file";
```
### 合体演算子 ??
```c#
string? text = GetText();  // null かもしれない
string result = text;      // ⚠ CS8600: null になるかもしれないのに非null変数に代入
string result2 = text ?? "default"; // textがnullなら??のあとの値を返す
string result3 = text ?? throw new ArgumentNullException(nameof(input)); // nullなら例外をスロー
```
### 初期化子
```c#
public NodeSelector Selector {
    get; set;
} = new NodeSelector(); //初期化子 コンストラクタと一緒
```
### 修飾子
#### required
オブジェクト初期化時に必ず値を設定することをコンパイラに要求する
```c#
public required string Name { get; set; } // クラスをインスタンス化するときに、Nameに値を設定が必須
```
#### **nullable 型修飾子**
```c#
int? number = null;   // int 型だけど null を代入できる
```
#### readonly
この変数が別のオブジェクトを参照することを禁止する
```c#
private static readonly int number = 5;
// number = 3; コンパイルエラー
private readonly List<string> list = new List<string>();
list.Add("text"); //Addは可
// list = new List<string>(); オブジェクト自体の変更は負荷
```
### メソッド
#### 名前付き引数
```C#
//メソッドの定義
void DisplayProfile(string name, int age, string city) {
	Console.WriteLine($"{name} ({age}歳) - 出身: {city}");
}
// 呼び出し方（順番を入れ替えてもOK）
DisplayProfile(age: 25, city: "東京", name: "山田");
```
#### デフォルト引数
```c#
void メソッド名(型 引数名 = デフォルト値)
void SayHello(string name = "ゲスト") {
    Console.WriteLine($"こんにちは、{name}さん！");
}
SayHello("太郎");  // 出力: こんにちは、太郎さん！
SayHello();        // 出力: こんにちは、ゲストさん！
```
#### パラメータ修飾子
引数の渡し方を定義, 値渡し、参照渡しの制御
- **out
**引数を使って複数の値を返す
```c#
public static bool TryParse(string s, out int result) { //パラメータ修飾子 result
	try{
	    result = int.Parse(s); //パラメータ修飾子に値をセット
	    return true; //戻り値で値を返す
	} catch {
	    result = 0;
	    return false;
	}
}
//呼び出し側
TryParse("123", out int number);//outで新しく変数を宣言して値を受け取る
```
- **in
値の変更不可の参照渡し**
- **ref
参照渡し**
### Record
プロパティとコンストラクタと等価性(Equalsメソッドを値で比較できる)を自動で実装してくれるクラス
```c#
public record ItemsVM(AbstractScraperConfig Config, List<object> Items);
//上の一文でconfigとItemsのプロパティとコンストラクタが自動生成
//初期化時に値を設定するのみで、後続で値変更は不可
```
### 自作例外クラス
```c#
public class MyCustomException : Exception
{
    // メッセージ付きコンストラクタ
    public MyCustomException(string message) 
        : base(message) { }//コンストラクタ内の:baseの引数が例外発生時のメッセージとして表示される
}
```
### 明示的に例外をスロー
```c#
throw new Exception(Constants.ContentNotFound);
```
# 標準ライブラリ
### IEnumerable
「列挙できるもの」を表す最も基本的なインターフェイス。
 `foreach` で回せるのはすべて `IEnumerable` を実装している。
### Dictionary
```c#
        //privateフィールド
        private static readonly Dictionary<int, string> Messages = new()
        {
            { 1, "Content node not found in the document." },
            { 2, "Logic property not found in the config JSON." },
            { 3, "Invalid logic value in JSON configuration." },
        };
        Messages.GetValueOrDefault(2);
        Messages.Add(4, "four"); readonlyだのでAddできない
       
```
### TryParse
```c#
bool result = int.TryParse(string s, out int value);
//s: 変換したい文字列
//value: 変換が成功したときに格納される整数値
//戻り値（bool）: 変換が成功したかどうかを表す
//outは複数の戻り値を返すためのキーワード

//三項演算子を使用
int? itemPrice = int.TryParse(value, out int price) ? price : null;
```
# C#ライブラリ
### JSONデシリアライズ/シリアライズ
```c#
string json = JsonSerializer.Serialize(list);
string json = JsonSerializer.Serialize(Items, new JsonSerializerOptions { WriteIndented = true }); //改行あり
```
### JSONに出力
```c#
        string filePath = @"C:\work\MyApps\matome_phase1\output.json"; //出力パスの定義
        string json = JsonSerializer.Serialize(list, options); //Listをjsonにシリアライズ
        File.WriteAllText(filePath, json); //書き出し
```
### HTML取得
```c#
using System.Net.Http;
using System.Threading.Tasks;

public static async Task<string> FetchHtmlAsync(string url)
{
    using var client = new HttpClient();
    return await client.GetStringAsync(url);
}
```
# エラー解消ナレッジ

### Null リテラルまたは Null の可能性がある値を Null 非許容型に変換しています。(警告)

```c#
string? text = GetText();  // null かもしれない
string result = text;      // ⚠ CS8600: null になるかもしれないのに非null変数に代入
string result2 = text ?? "default"; // ??
```
# XMLドキュメントコメント
# Link
### Json to C#
jsonをC#クラスに変換
[https://json2csharp.com/](https://json2csharp.com/)