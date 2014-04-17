# WebAPIからデータのみ取得

## はじめに

HTTPCLientの機能を活用してWebAPIからのデータ取得方法について解説します。WebAPIには色々なものがありますが、WebAPIの構造がシンプルで、最低限の利用に限れば細かい手続きが不要な[Qiita](https://qiita.com)を活用することにします。


## QiitaのWebAPIの構造について

QiitaのWebAPIは

**https://qiita.com/api/v1/利用したいサービス別のディレクトリ**

という形になってます。

### Qiitaの開発者向けのドキュメントの読み方について

[Qiitaの開発者向けのドキュメント](http://qiita.com/docs)に利用したいサービス別のディレクトリ情報などがまとまってますが、ドキュメントの読み方について簡単に説明します。

特定ユーザの情報取得の項では

**GET /api/v1/users/:url_name**

と記載されています。例えば、私のQiita上のユーザ情報（アカウント名はh5y1m141@github)を取得したい場合には **:url_name** の所を該当ユーザに置き換えることで情報が取得出来ます。

具体的には、[https://qiita.com/api/v1/users/h5y1m141@github](https://qiita.com/api/v1/users/h5y1m141@github) にアクセスすることで私のQiita上のユーザ情報を以下のように取得することができます。


```javascript
{"id":10187,"url_name":"h5y1m141@github","profile_image_url":"https://secure.gravatar.com/avatar/4fdf95707fe9a33f3a1ba8c97315468c?d=https://a248.e.akamai.net/assets.github.com%2Fimages%2Fgravatars%2Fgravatar-user-420.png","url":"http://qiita.com/h5y1m141@github","description":"iPhone用のQiita Viewerアプリ作ってます\r\nhttps://github.com/h5y1m141/TiQiita","website_url":"http://h5y1m141.hatenablog.com/","organization":"","location":"","facebook":"","linkedin":"","twitter":null,"github":"h5y1m141","followers":7,"following_users":9,"items":1,"teams":[],"image_upload":{"limit":2097152,"used":0}}
```

## 実装する前にWebブラウザを使ってQiitaのWebAPIにアクセスする		

実装する前にWebブラウザを使ってQiitaのWebAPIにアクセスして、どのような結果が取得できるのかを確認してみましょう。

Webブラウザを起動して以下URLにアクセスします

[https://qiita.com/api/v1/items](https://qiita.com/api/v1/items)

以下はMac版のGoogle Chromeでアクセスした時の画面イメージになります。

![ブラウザでアクセスした時のキャプチャ](../../image/qiitaviewer-webapi-001.jpg)

QiitaのWebAPIの構造についておおまかに理解出来たかと思いますので、実際にQiitaの投稿情報を取得する処理を実装していきます。

## Qiitaの投稿情報を取得する処理を実装する

Qiitaの投稿情報を取得するためにTitanium Mobileの 通信機能を使って実装します。

### Titanium Mobileの 通信機能を使って実装する

1. プロジェクト作成時に自動的に生成された **app.jsの中身のソースコードを全て削除**します。
2. その後に以下を記述します

```javascript
var xhr,qiitaURL,method;
qiitaURL = "https://qiita.com/api/v1/items";
method = "GET";
xhr = Ti.Network.createHTTPClient(); // (1)
xhr.open(method,qiitaURL);  // (2)
xhr.onload = function(){
  var body;
  if (this.status === 200) { // (3)
    body = JSON.parse(this.responseText); // (4)
	Ti.API.info(body);
  } else {
    Ti.API.info("error:status code is " + this.status);
  }
}
xhr.onerror = function(e) { // (5)
  var error;
  error = JSON.parse(this.responseText);
  Ti.API.info(error.error);
}
xhr.timeout = 5000;
xhr.send();
```
### ソースコード解説

1. httpClientを利用するためのオブジェクトを生成します

2. open()メソッドを使ってQiitaのWebAPIにアクセスします。最初の引数にHTTPメソッドを指定しますが、Qiitaの投稿の取得をする場合には、GETメソッドを指定する必要があります（詳しくは[Qiitaのドキュメント](http://qiita.com/docs#13)を参照してください)次の引数で投稿情報を取得するQiitaのエンドポイントとなるURLを指定します。

3. QiitaのWebAPIにアクセスして、接続成功したかどうかを判定して、その後の処理を実施します。具体的にはthis.statusの値を確認して、値が200の場合には接続成功しているため該当する処理を実施します

4. this.responseTextの値を確認することで、サーバから取得できた値をテキスト形式で取得できます。this.responseTextは見た目はJSON形式になっていますが、そのまま変数に代入すると文字列としてその後処理されてしまうため、JSON.parse()を使って、JSON化した状態で変数に格納します

5. 例えば、QiitaのWebAPIにアクセスして、150リクエスト/1時間というAPIの利用制限に引っかかってしまう場合などはエラーになり、その時にはonerrorイベントが呼び出されます。

イメージとしては以下のような対応関係になります

![httpClientのonloadとonerrorの対応関係](../../image/qiitaviewer-httpClient-overview-001.jpg)

動作確認するために、buildした結果は以下のとおりです

#### iPhone起動時の画面キャプチャ

![選択メニュー](../../image/qiitaviewer-httpClient-iphone-001.jpg)

#### Android起動時の画面キャプチャ

![選択メニュー](../../image/qiitaviewer-httpClient-android-001.jpg)

この段階ではTitanium Mobileの通信機能を使ってデータ取得することを目的に実装しているため、シミュレーターの画面には何も標示されずコンソール上に複数の文字が表示されるかと思います。それが確認できればOKです
