---

---
# 実行コマンド
**環境構築が未実施の場合は、環境構築を先に行う**
tailwind実行中にhtmlで定義された必要なcssのみ、-iオプションで指定したファイルに出力される。
よって、tailwindを使用してhtmlを編集する際は、tailwind実行コマンドを実行する必要がある。
```bash
# 以下を実行してtailwindを実行。
npx @tailwindcss/cli -i ./wwwroot/css/app.css -o ./wwwroot/css/output.css --watch
```
### Package.jsonにscriptを登録

ショートカットコマンドを登録

登録したら、`npm run dev` でコマンド実行が可能になる

```json
{
    "dependencies": {
        "@tailwindcss/cli": "^4.1.18",
        "tailwindcss": "^4.1.18"
    },
    "scripts": {
        "dev": "npx @tailwindcss/cli -i ./wwwroot/css/app.css -o ./wwwroot/css/output.css --watch"
    }
}

```
# 環境構築

[Tailwind CLI - Installation](https://tailwindcss.com/docs/installation/tailwind-cli)

### npmのインストール
```bash
# nodeインストール (未インストール時)
winget install OpenJS.NodeJS.LTS
# 実行環境を整えます。
npm install tailwindcss @tailwindcss/postcss postcss
```

### **Install Tailwind CSS**

```bash
npm install tailwindcss @tailwindcss/cli
```

### **Import Tailwind in your CSS**

Add the `**@import "tailwindcss";**` import to your main CSS file.

```bash
@import "tailwindcss";
```

### **Start the Tailwind CLI build process**

tailwindビルドプロセスが起動。
閉じたらtailwindが反映されなくなるので、閉じたらまた起動する

input.cssは先の手順で`@import`を書き込んだcssファイルを指定。
output.cssは適当なパス

```bash
npx @tailwindcss/cli -i ./src/input.css -o ./src/output.css --watch
```

### **Start using Tailwind in your HTML**

```bash
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link href="./output.css" rel="stylesheet"> <!-- 使用するhtmlこの行追加 --!>
</head>
<body>
  <h1 class="text-3xl font-bold underline">
    Hello world!
  </h1>
</body>
</html>
```

***
# チートシート
[Tailwind CSS Cheat Sheet](https://nerdcave.com/tailwind-cheat-sheet)
.~は、classの表現
## flex-box
```tailwind

```