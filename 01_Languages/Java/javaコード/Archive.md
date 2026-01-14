---

---

> [!note]+ TreeMap
> [link](https://java-code.jp/230)
> 
> HashMapではキーの順番を保証しないのに対して、TreeMapではキーを自動的にソートし、順序を保証します

> [!note]+ [連想配列](https://eng-entrance.com/java-hashmap#i)/hashmap
> 言葉同士でキーと値が繋がった配列を連想配列という
> 
> ```java
> Map<String, String> map = new HashMap<>(); 
> ```
> 
> ```java
> HashMap<String, String> hashmap = new HashMap<String, String>(); //整数はInteger
> hashmap.put("apple", "りんご");          //keyに"apple" 取り出したい値が"りんご"
> System.out.println(hashmap.get("apple"))//"りんご"と出力される
> ```

> [!note]+ hashSet
> [順番を持たない要素の集合](https://java-code.jp/228)
> 
> ```java
> Set<String> set = new HashSet<>();
> ```