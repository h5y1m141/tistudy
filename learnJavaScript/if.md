# JavaScript基礎:制御文（if）

## はじめに

過去私が作ってきたアプリを振り返った場合に、forループで処理をしつつ、**特定の項目の値がtrueだった場合** にTableViewにLabelを追加する処理を実装したり、 **Facebookのようなユーザインタフェースのアプリで、メインウィンドウがスライドした状態にあるかか判定** して、処理を実行するようなケースでif文を利用してました。

if文自体は使いこなすのはそれほど難しい点はないかもしれませんが、JavaScriptでif文使う上での注意点があるため、簡単なコードを紹介しながらそのその点の解説をしたいと思います。

## 同じ値かどうか判定しながら処理をする

以下の様なサンプルコードがあった場合に(1)と(2)とでそれぞれどういう結果になるのかわかりますか？

```javascript
var list1 = 1.0;
var list2 = "1.0";

// (1)値の確認が同じかどうか確認する時に == を利用した場合
if(list1 == list2){
  Ti.API.info("list1 and list2 is same");
} else {
  Ti.API.info("list1 and list2 is not same");
}


// (2)値の確認が同じかどうか確認する時に === を利用した場合
if(list1 === list2){
  Ti.API.info("list1 and list2 is same");
} else {
  Ti.API.info("list1 and list2 is not same");
}
```

両方共同じ結果になりそうですが、結果はというと

```sh
[INFO] list1 and list2 is same
[INFO] list1 and list2 is not same
```
という形になります。

「あれ何でだろう？」

と素朴な疑問を持つかもしれませんし、私も昔この違いよく理解せず、ひとまず、全部 == としてましたが、ここを理解してないと思いがけないエラーを引き起こす可能性あるため、順を追って説明します。

### まず list1とlist2に格納されてる「型」を理解する

list1は**数字**として扱うことを意識してクォーテーション無しで値を代入し、一方、list2は**文字**として扱うことを意識してクォーテーションで囲いました。

（JavaScript含めて）プログラミング言語の変数には**型**というものがあります。

プログラミング言語によっては、変数を宣言する時に、その変数がどのような型なのかを同時に指定しなければいけないものがありますが、JavaScriptは変数名だけを宣言すればOKなのですが、あくまで宣言する時の話であって、実際には、型というものが存在してます。

**typeof**という関数を以下のように利用することでその変数の型を調べることが出来ます。

```javascript
Ti.API.info("list1 type: " + typeof list1);
Ti.API.info("list2 type: " + typeof list2);
```

上記の実行結果はこのようになります

```sh
[INFO] list1 type: number
[INFO] list2 type: string
```

list1は数値、list2は文字列として扱われているため、頭のなかで数字同士の足し算をしてるつもりになって

list1 + list2

とやっても、このようなケースの場合には、list1が数値でもlist2が文字列のため、前者のlist1が文字列として扱われてしまうため

11.0

という結果になります。

ちょっとしたサンプルアプリ程度なら**型の違い**を深く理解してなくてもエラーになる確率は少ないかもしれませんが、ある程度の規模のアプリケーションの開発をする際には型についてしっかりと抑えておく必要があります。

型についてもう少し踏み込んで勉強したい方は

- JavaScriptの型にはどのようものがあるのか？
- 上記のような型が異なる場合の処理を行うための型変換

あたりを書籍などで調べて見ると良いかと思います。

<div class="amazlet-box" style="margin-bottom:0px;"><div class="amazlet-image" style="float:left;margin:0px 12px 1px 0px;"><a href="http://www.amazon.co.jp/exec/obidos/ASIN/487311425X/hoyamada-22/ref=nosim/" name="amazletlink" target="_blank"><img src="http://ecx.images-amazon.com/images/I/513cHHi%2BkCL._SL160_.jpg" alt="初めてのJavaScript 第2版" style="border: none;" /></a></div><div class="amazlet-info" style="line-height:120%; margin-bottom: 10px"><div class="amazlet-name" style="margin-bottom:10px;line-height:120%"><a href="http://www.amazon.co.jp/exec/obidos/ASIN/487311425X/hoyamada-22/ref=nosim/" name="amazletlink" target="_blank">初めてのJavaScript 第2版</a><div class="amazlet-powered-date" style="font-size:80%;margin-top:5px;line-height:120%">posted with <a href="http://www.amazlet.com/" title="amazlet" target="_blank">amazlet</a> at 14.03.31</div></div><div class="amazlet-detail">Shelley Powers <br />オライリージャパン <br />売り上げランキング: 478,390<br /></div><div class="amazlet-sub-info" style="float: left;"><div class="amazlet-link" style="margin-top: 5px"><a href="http://www.amazon.co.jp/exec/obidos/ASIN/487311425X/hoyamada-22/ref=nosim/" name="amazletlink" target="_blank">Amazon.co.jpで詳細を見る</a></div></div></div><div class="amazlet-footer" style="clear: left"></div></div>

<div class="amazlet-box" style="margin-bottom:0px;"><div class="amazlet-image" style="float:left;margin:0px 12px 1px 0px;"><a href="http://www.amazon.co.jp/exec/obidos/ASIN/4873113911/hoyamada-22/ref=nosim/" name="amazletlink" target="_blank"><img src="http://ecx.images-amazon.com/images/I/41H0Dk-K3PL._SL160_.jpg" alt="JavaScript: The Good Parts ―「良いパーツ」によるベストプラクティス" style="border: none;" /></a></div><div class="amazlet-info" style="line-height:120%; margin-bottom: 10px"><div class="amazlet-name" style="margin-bottom:10px;line-height:120%"><a href="http://www.amazon.co.jp/exec/obidos/ASIN/4873113911/hoyamada-22/ref=nosim/" name="amazletlink" target="_blank">JavaScript: The Good Parts ―「良いパーツ」によるベストプラクティス</a><div class="amazlet-powered-date" style="font-size:80%;margin-top:5px;line-height:120%">posted with <a href="http://www.amazlet.com/" title="amazlet" target="_blank">amazlet</a> at 14.03.31</div></div><div class="amazlet-detail">Douglas Crockford <br />オライリージャパン <br />売り上げランキング: 4,516<br /></div><div class="amazlet-sub-info" style="float: left;"><div class="amazlet-link" style="margin-top: 5px"><a href="http://www.amazon.co.jp/exec/obidos/ASIN/4873113911/hoyamada-22/ref=nosim/" name="amazletlink" target="_blank">Amazon.co.jpで詳細を見る</a></div></div></div><div class="amazlet-footer" style="clear: left"></div></div>

---

## == と === の違い

Qiitaに[厳密等価演算子 javaプログラマのjavascript入門](http://qiita.com/lasaya/items/d7d7a98e089d7fb91b84)という記事がありここから一部文章を引用します

> ==を「等価演算子」
> ===を「厳密等価演算子」という。
> 「厳密等価演算子」は型が同じかどうかチェックするが、「等価演算子」はチェックしない。
> 「等価演算子」を使うと型が異なる場合下記のように変換して比較する。
> ・数値と文字列を比較するとき、文字列は数値に変換される。

== を使ったチェックの場合には、型の違いはチェックしないため、値だけのチェックになります。

そのためlist1、list2とも、値としては1.0なので、

```javascript
var list1 = 1.0;
var list2 = "1.0";

if(list1 == list2){
  Ti.API.info("list1 and list2 is same");
} else {
  Ti.API.info("list1 and list2 is not same");
}
```

とあった場合には、コンソール上では

```sh
Ti.API.info("list1 and list2 is same");
```
と表示されます。

一方で、=== を利用した場合には、値のチェックのみならず、型のチェックも行い、値は両方共1.0ですが、listは数値型、list2は文字列型で異なる型になります。

そのためif文でチェックした場合には、
```javascript
var list1 = 1.0;
var list2 = "1.0";

if(list1 === list2){
  Ti.API.info("list1 and list2 is same");
} else {
  Ti.API.info("list1 and list2 is not same");
}
```
というコードはelse句が評価されるため、コンソール上では

```sh
[INFO] list1 and list2 is not same
```
と表示されます。


## まとめ

この章の内容が長くなったので最後簡単にまとめておきます。

if文を使いこなすのは簡単そうに見えるのですが、以下の様なポイントおさえておかないと思いがけないエラーが生じる可能性あります。

- 変数には型があるのをまずは理解しておく必要ある。
- if文を使って値のチェックをする時に、== （等価演算子）と=== （厳密等価演算子）がある。
- list1 = 1.0 と list2 = "1.0" というような変数同士の演算を行う場合には上記2点をしっかり理解しておく必要ある。
