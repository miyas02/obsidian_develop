---

---
**匿名クラス**

クラス名を指定せずに、クラス定義とインスタンス化を1つの式として記述したクラスのこと。
匿名クラスはあるサブクラスまたは、インターフェースを実装したクラス。

**インターフェース**

- 変数宣言は初期化する必要がある。しないとコンパイルエラー
- メソッドのアクセス修飾子はpublicのみ。何もつけないと勝手にpublicがつく
- メソッドにfinalを付けるの禁止
- 抽象メソッドにstatic禁止
- staticな具象メソッドは定義可能
    - アクセス修飾子はpublicまたはprivate
- defaultをつければ、具象メソッドを定義可能
- privateな具象メソッドも定義可能

**ネストクラス**

- 非staticクラス
    - staticメンバを持てない
    - 外側のクラスのインスタンス変数、static変数にアクセスできる
- staticクラス
    - staticメンバを持てる
    - 外側のクラスのインスタンス変数にアクセスできない

**アノテーション**

**関数型インターフェース**

抽象メソッドが1つだけ定義されたインターフェース

関数型インターフェースを引数に受け取るメソッドは、呼び出し側（ユーザー）が定義した振る舞い（処理）を、メソッド内で実行するためにある

**ラムダ式
**`(実装するメソッドの引数) -> {処理;}
`ラムダ式全体が関数型インターフェースの抽象メソッドの処理を実装したインスタンス

ラムダ式の左辺（つまり、ラムダ式の**引数の型**）は、**代入先の関数型インターフェースのジェネリクス型パラメータ**と一致する

**ストリームAPI
**コレクション、配列、I/Oリソースなどを集計操作するAPI

### **ストリーム生成**

**Collectionインタフェース**

```java
default Stream<E> stream()
```

**Arraysクラス**

```java
static<T> Stream<T> stream(T[] array)
static IntStream stream(int[] array)
```

**Streamインターフェース**

```java
static<T> Stream<T> of(T t)
static<T> Stream<T> of(T… values)
static<T> Stream<T> empty()
static<T> Stream<T> generate(Supplier<T> s)
```

**IntStreamインターフェース**

```java
static IntStream of(int… values)
static IntStream iterate(int seed, IntUnaryOperator f)
static IntStream iterate(int seed, IntPredicate hasNext, IntUnaryOperator next)
static IntStream range(int startInclusive, int endExclusive)
static IntStream rangeClosed(int startInclusive, int endInclusive)
```

### **終端操作**

```java
boolean allMatch(Predicate<? super T> predicate)
boolean anyMatch(Predicate<? super T> predicate)
boolean noneMatch(Predicate<? super T> predicate)
<R,A> R collect(Collector<? super T,A,R> collector)
//Collectorsクラスのstaticメソッドを引数に指定
<R> R collect(Supplier<R> supplier, BiConsumer<R,? super T> accumulator, BiConsumer<R,R> combiner>
long count()
Optional<T> findAny() 
Optional<T> findFirst()
void forEach(Consumer<? super T> action)
Optional<T> min(Comparator<? super T> comparator)
Optional<T> max(Comparator<? super T> comparator)
T reduce(T identity, BinaryOperator<T> accumulator)
Optional<T> reduce(BinaryOperator<T> accumulator)
//reduce → 集約処理,加算、乗算、文字列結合、最小値、最大値のような集約処理に使う
//T identityは初期値、初期値を指定しない場合はOptional<T>が返る
Object[] toArray()
<A> A[] toArray(IntFunction<A[]> generator)
```

### **中間操作**

```java
Stream<T> filter(Predicate<? super T> predicate)
Stream<T> distinct()
Stream<T> limit(long maxSize)
Stream<T> skip(long n)
<R> Stream<R> map(Function<? super T,? extends R> mapper)
<R> Stream<R> flatMap(Function<? super T,? extends Stream<? extends R>> mapper)
Stream<T> sorted()
Stream<T> sorted(Comparator<? super T> comparator)
Stream<T> peek(Consumer<? super T> action)
```

### **Optionalクラス**

```java
static<T> Optional<T> empty()
static<T> Optional<T> of(T value)
T get()
boolean isPresent()
boolean isEmpty()
void ifPresent(Consumer<? super T> consumer)
T orElse(T other)
T orElseGet(Supplier<? extends T>
<X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier) throws X extends Throwawble
```

### **Comparable**

自分自身の並び順を定義
構造体クラスなどにimplements Comparabeで実装

```java
class Structure implements<Structure> {
//~~省略~~
  @Override
  public int compareTo(Person other) {
      return Integer.compare(this.id, other.id); // id順
  }
}
Collections.sort(list) // compareToに従ってソート
```

### **Comparator**

外部から並び順を定義

```java
//使用例
list.sort(Comparator.comparing(p -> p.name)); //名前順でソート
//comparingメソッド
static <T,U extends Comparable<? super U>> Comparator<T> comparing(Function<? super T,? extends U> keyExtractor)
```

**コレクション一覧**

> [!note] 📌
> Collection(インターフェース) Listやsetの共通インターフェース
Collections(ユーティリティクラス) コレクション操作の補助(ソート、同期化など）
Collector(インターフェース) collect()の中で使う集約のロジック
Collectors(ユーティリティクラス) Collectorの便利な実装を提供

**Collectorsクラスのstaticメソッド**

Collectorインスタンスを返す
streamAPIのcollectメソッドの引数に渡して使う

```java
Stream<String> stream = Stream.of("sampleA","sampleB","sampleC");
List<String> list = stream.collect(Collectors.toList());
```

```java
//<String,?,List<String>>はStream<String>からList<String>を生成
static<T> Collector<T,?,List<T>> toList()
static Collector<CharSequence,?,String> joining()
static Collector<CharSeqquence,?String> joining(CharSequence delimiter)
static <T> Collector<T,?,Integer> summingInt(ToIntFunction<? super T> mapper)
static<T> Collector<T,?,Double> averagingInt(ToIntFunction<? super T> mapper)
static<T> Collector<T,?,Set<T>> toSet()
static<T,K,U> Collector<T,?,Map<K,U>> toMap(Function<? super T,? extends K>key, Function<? super T,? extends U> value)
//Stream<String> stream1 = Stream.of("sampleA","sampleB","sampleC");
//Map<String, String> map1 = stream1.collect(Collectors.toMap(s -> s, s -> s.toUpperCase()) //引数1がキー,引数2がvalueのmapを生成
static<T,K,U> Collector<T,?,Map<K,U>> toMap(Function<? super T,? extends K> keyMapper, Function<? super T,? extends U> valueMapller,BinaryOperator<U> mergeFunction)
//Map<Integer, String> map2 = stream1.collect(Collectors.toMap(s -> s.length(), s -> s, (s1, s2) -> s1 + " : " + s2)); //引数1がキー,引数2がvalue,引数3がキーが重複した時の処理
static<T,K> Collector<T,?,Map<K,List<T>>> groupingBy(Function<? super T,? extends K> classifier)
//Map<String, List<String>>map = stream1.collect(Collectors.groupingBy(s -> s.substring(0, 1)); //Functionのapply()の引数に要素を渡して、マップのキーとなる値を返すような処理を指定する
//上の処理は頭文字をキーとして、グルーピングしている
static<T,K> Collector<T,?,Map<K,List<T>>> groupingBy(Function<? super T,? extends K> classifier, Collector<? super T,A,D> downstream)
//第2引数ではグループ化したリストに対して行いたい処理を指定する
//Map<String, Set<String>>map1 = stream1.Collect(Collectos.groupingBy(s -> s.substring(0,1), Collectors.toSet()));
static<T,K> Collector<T,?,Map<K,List<T>>> groupingBy(Function<? super T,? extends K> classifier, Collector<? super T,A,D> downstream, Collector<? super T,A,D> downstream)
//TreeMapを使用してキーの昇順による順序付けを維持したい場合に使用する
//Map<String, String> map3 = stream1.collect(Collectors.groupingBy(s -> s.substring(0, 1), TreeMap::new, Collectors.joining()));
static<T> COllector<T,?,Map<Boolean,List<T>>> partitioningBy(Predicate<? supert T> predicate)
//キーがtrueまたはfalseのmapを生成する
//Stream<Integer> stream1 = Stream.of(3, 5, 7, 9);
//Map<Boolean, List<Integer>> map1 = stream1.collect(Collectors.partitioningBy(s -> s > 5));
static<T> COllector<T,?,Map<Boolean,List<T>>> partitioningBy(Predicate<? supert T> predicate, Collector<? super T,A,D> downstream)
//グループ化したリストに対して行いたい処理がある場合に使用する
//Map<Boolean, Integer> map3 = stream2.collect(Collectors.partitioningBy(s -> s > 5, Collectors.summingInt(n -> n)));
static<T,U,A,R> Collector<T,?,R> mapping(Function<? super T,? extends U> mapper, Collector<? super U,A,R> downstream)
//mapメソッド同様にストリームの各要素に対して行いたい処理を指定する。第2引数にはマップ後に行いたい処理を指定
static<T> COllector<T,?,Optional<T>> maxBy(Comparator<? super T> comparator)
static<T> Collector<T,?,Optional<T>> minBy(Comparator<? super T> comparator)
```

**ジェネリクス**

<? super T>
ラムダ式の左辺の値がTまたはTのスーパークラスであることを保証する。
`Consumer<? super Integer>` であれば、`Integer` のスーパークラス（例えば `Number` や `Object`）を受け取る `Consumer` を代入できます。
`Consumer<? super Integer> target = num -> System.out.println("Number: " + num.doubleValue());`

# モジュール

パッケージの上位に位置づけられ、パッケージをさらにグループ化する。

```java
//module-info.java
module foo { //モジュール名は.で区切った名前を使用することができる
  exports xlib; //どのパッケージを公開するかを定義
  requires hoge; //必要としているモジュール名を定義. fooモジュールからhogeモジュールにアクセス可能
  requires java.base; //すべてのモジュールに暗黙的に含まれるため省略可能
  requires transitive bar; //間接エクスポート. fooをrequiresしたモジュールはbarモジュールもrequiresしたことになる.
  provides xlib.MyInter with xlib.XTest, xlib.YTest; //「サービス」に対する「サービスプロバイダ」を指定.サービスはサービスインターフェースのクラス名.サービスプロバイダは実装クラス名を記述.
  uses xlib.MyInter;
}
```

```powershell
#java javacコマンド
java --module-path #(-p) モジュールが格納されているパスを指定.
java --module #(-m) モジュール名とエントリ・ポイントとなるクラス(mainクラス)を完全修飾名で指定.
java -module-path #jarファイルを使用した実行では、-module-pathでjarファイルが保存されているディレクトリを指定.
javac -d <クラスファイルの生成場所> <コンパイル対象のソースファイル>
javac -cp <classpath> #クラスパス指定オプション 
#javaコマンド
java --list-module #参照可能なモジュールを出力
java --describe-module (-d) #モジュール記述子の情報を出力
java --show-module-resolution #モジュールの解決の様子を出力
#jarコマンド
jar cvf [ファイル名].jar [対象ファイルやディレクトリ]
#jdepsコマンド
jdeps -summary (-s) #依存関係のサマリーを出力
jdeps -jdkinternals #JDKの内部APIでクラス・レベルの依存関係を検索
jdeps -dotoutput #DOTファイルの出力先ディレクトリを指定.
#jlinkコマンド
jlink --add-modules #イメージに追加数モジュールを固定
jlink --module-path (-p) #モジュール・パスを指定
jlink --compress (-c) #リソースの圧縮を有効化 0:圧縮なし, 1:定数文字列の共有, 2:ZIP
jlink --launcher command=module (--launcher command=module/main) #モジュールのランチャー・コマンド名、またはモジュールおよびメイン・クラスのコマンド名
jlink --output #出力ディレクトリ
```

### 無名モジュール

クラスパス上に存在し、モジュール名をもたない(module-info.classがない)モジュール

- すべてのパッケージをexportsする
- モジュールパス上のすべてのモジュールをrequiresする
- 名前付きモジュールから無名モジュールを参照することはできない

### 自動モジュール

モジュールパス上に存在し、モジュール名をもたない(module-info.classがない)モジュール
例)自身が作成したアプリケーションはモジュール化されているが、その中で使用しているライブラリがモジュール化していないなど

- すべてのパッケージをexportsする
- モジュールパス上のすべてのモジュールをrequiresする
- 名前付きモジュールから自動モジュールの参照が可能

