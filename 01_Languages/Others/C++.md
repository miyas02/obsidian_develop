---

---
パフォーマンスを出しやすくするために低レベルなコードが書けるようになっている分、 不具合が混入しやすいというデメリットがあります。

### ポインタ

```c++
int x = 5; 
int* p = &x; //「int*」でポインタ型を定義, &xでxのメモリを指定
int y = *p; //*pでポインタ型の値を参照
```

### 参照

変数にalias(別名)をつけるイメージ

```c++
int x = 5; 
int& r = x; //alias
```

# 関数

### 関数の前方宣言

関数の宣言のみを呼び出し箇所よりも上の行に記述することで、関数の存在をコンパイラに教えることが出来ます。

```c++
#include <iostream>

void HelloWorld(int n);  // 前方宣言

int main() {
    HelloWorld(10);

    return 0;
}

void HelloWorld(int n) {
    for (int i = 0; i < n; ++i) {
        std::cout << "[" << i << "] " << "Hello World!" << std::endl;
    }
}
```

### コマンドライン引数

```c++
//コマンドライン引数を受け取る場合、main関数の引数は次のようにします。
int main(int argc, char* argv[]);
//int argc: コマンドライン引数の個数を表す
//char* argv[]: コマンドライン引数が格納される
```

# 複数ファイル

### ファイルの種類

ヘッダ (拡張子: `.h` , `.hpp` )

ソース (拡張子: `.cpp` , `.cc` , `.cxx` )

### ソースファイルをわける

```c++
main.cc
void DoSomething();   // プロトタイプ宣言

int main() {
    DoSomething();
    return 0;
}
```

```c++
a.cc
void DoSomething() { /* 実装省略 */ }
```

### ヘッダファイルを利用する

```c++
main.cc
int main() {
    DoSomething();
    return 0;
}
```

```c++
a.h
void DoSomething();   // プロトタイプ宣言
```

```c++
a.cc
void DoSomething() { /* 実装省略 */ }
```

### インクルードガード

ヘッダにはインクルードガードが必要です。

```c++
#ifndef A_H_
#define A_H_

void DoSomething();
void DoSomething2();

#endif  // A_H_
```
