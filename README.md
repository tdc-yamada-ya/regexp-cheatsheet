# テキストを検索・置換するための正規表現チートシート

複雑な条件でテキストを検索・置換えするために使用する正規表現の使い方をまとめました。

## 正規表現を使う理由

正規表現を使うことでテキストを検索・置換する処理が格段に効率的になります。
例えば、ログを検索する際に「4時と8時にERRORと出力されている行」を一回で検索したり、
行から必要な部分を抽出してタブ区切りテキストに変換したり、
といったことが可能になります。

## 正規表現を活用するには

正規表現を活用するには普段から練習しておく必要があります。
練習の方法は単純で正規表現に対応したテキストエディタを使用してログファイルや公開されているソースコードを対象に検索を行うだけです。

## Notepad++

[Notepad++](https://notepad-plus-plus.org/)

* 正規表現を使用で
* 無量
* 日本語にも対応
* **ログファイルのような巨大ファイルをスムーズに開くことができる**

正規表現による検索・置換を行うには Ctrl+F(検索) あるいは Ctrl+H(置換) を押して、
検索モードから正規表現を選択してください。

## メタキャラクタ

Notepad++の[正規表現の説明ページ](http://docs.notepad-plus-plus.org/index.php/Regular_Expressions)から使う可能性が比較的高いものをピックアップしました。
他のテキストエディタでは同様の動作にならない可能性があります。

|メタキャラクタ|説明|
-|-
|`.`|任意の一文字|
|`\c`|任意の一文字|
|`\X`|結合文字とそれに続く任意長の非結合文字|
|`\xnn`|`nn`で指定した8ビット16進数の文字コード|
|`\xnnnn`|`nnnn`で指定した16ビット16進数の文字コード|
|`\n`|改行(form feed)|
|`\r`|改行(line feed)|
|`\R`|全ての改行文字|
|`\t`|タブ|
|`[abc]`|a, b あるいは c のいずれか|
|`[a-z]`|a から z までのいずれか|
|`[^A-Za-z]`|A から Z、a から z のいずれでもない|
|`\d`|数字(全角を含む)|
|`\D`|数字以外|
|`\l`|小文字のアルファベット(全角を含む)|
|`\L`|小文字のアルファベット以外|
|`\u`|大文字のアルファベット(全角を含む)|
|`\U`|大文字のアルファベット以外|
|`\w`|数字(全角を含む)、アルファベット（全角を含む）、アンダースコアのいずれか|
|`\W`|数字(全角を含む)、アルファベット（全角を含む）、アンダースコアのいずれでもない|
|`\s`|空白(全角を含む)、タブのいずれか|
|`\S`|空白、タブのいずれでもない|
|`+`|直前の文字の1文字以上の繰り返し(最長一致)|
|`+?`|直前の文字の1文字以上の繰り返し(最短一致)|
|`*`|直前の文字の0文字以上の繰り返し(最長一致)|
|`*?`|直前の文字の0文字以上の繰り返し(最短一致)|
|`{n}`|直前の文字のn回の繰り返し|
|`{n,}`|直前の文字のn回以上の繰り返し(最長一致)|
|`{n,}?`|直前の文字のn回以上の繰り返し(最短一致)|
|`{m,n}`|直前の文字のm回以上n回以下の繰り返し(最長一致)|
|`{m,n}?`|直前の文字のm回以上n回以下の繰り返し(最短一致)|
|`^`|行の始まり|
|`$`|行の終わり|
|`(...)`|グループ化して、マッチ箇所の部分文字列を置き換えることができる|
|`(:...)`|単に読みやすさを向上するためにグループ化し、部分文字列としてはカウントされない|

* 正規表現による検索であっても小文字、大文字のアルファベットを区別するには、検索オプションの`大/小文字を区別`にチェックを入れる必要があります

## Apacheアクセスログ(標準フォーマット)の検索・置換例

```apache
127.0.0.1 - frank [10/Oct/2000:13:55:36 -0700] "GET /apache_pb.gif HTTP/1.0" 200 2326 "http://www.example.com/start.html" "Mozilla/4.08 [en] (Win98; I ;Nav)"
```

|用途|検索|置換|
-|-|-
|IPアドレスの検索|`^127\.0\.0\.1`|なし|
|IPアドレスを抽出|`^(\S+).*$`|`$1`|
|ファイル名を抽出|`^.*"[A-Z]+ (.+?) .+?".*$`|`$1`|
