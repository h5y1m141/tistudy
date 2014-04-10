## 幅、高さ、色などの設定情報だけを別ファイルにしておく

Webサイト構築する際に、見栄えに関わる部分をHTMLから分離して、CSSに記述するというのは理解しやすい話しかと思います。このような考え方をTitanium Mobileでの開発で応用させることで、当初のソースコードから見通しの良いものになるかと思います。

具体的な実装方法としては

- Window、TableView、Label、Button のそれぞれのUI 要素の幅、高さ、色などの設定情報だけをまとめたファイルを作る（**仮にstyle.js**とします）

- UI 要素となるオブジェクトを生成する際に上記のstyle.jsに記述された該当の設定情報を反映させる

という形になります。

UI生成に関する処理と、それぞれのUI 要素の設定情報を以下のように分離させることで当初のソースコードよりも見通しがよくなるかと思います

## ソースコード解説

まず、app.jsを以下のようにします

```javascript
// app.js

var styleJS,file,style,xhr,qiitaURL,method,mainTable,win,view,colorSet;
styleJS = Ti.Filesystem.getFile(Ti.Filesystem.resourcesDirectory, "style.js");
file = styleJS.read().toString();
style = JSON.parse(file);
mainTable = Ti.UI.createTableView(style.mainTable);
win = Ti.UI.createWindow({
  title:'QiitaViewer'
});

qiitaURL = "https://qiita.com/api/v1/items";
method = "GET";

xhr = Ti.Network.createHTTPClient();
xhr.open(method,qiitaURL);
xhr.onload = function(){
  var body,_i ,_len ,row ,rows,textLabel,iconImage,imagePath;
  if (this.status === 200) {
    body = JSON.parse(this.responseText);
    rows = [];
    for (_i = 0, _len = body.length; _i < _len; _i++) {
      if(Ti.Platform.osname==="iphone"){
        row = Ti.UI.createTableViewRow(style.iPhoneRow);
        textLabel = Ti.UI.createLabel(style.textLabel);
	textLabel.text = body[_i].title;
        imagePath = body[_i].user.profile_image_url;
        iconImage = Ti.UI.createImageView(style.iconImage);
	iconImage.image = imagePath;
        row.add(textLabel);
        row.add(iconImage);

      }else{
        row = Ti.UI.createTableViewRow(style.androidRow);
        view = Ti.UI.createView(style.view);
	textLabel = Ti.UI.createLabel(style.textLabel);
	textLabel.text = body[_i].title;
        imagePath = body[_i].user.profile_image_url;
        iconImage = Ti.UI.createImageView(style.iconImage);
        iconImage.image = imagePath;
        view.add(textLabel);
        view.add(iconImage);
        row.add(view);

      }
      rows.push(row);
    }
    mainTable.setData(rows);
    win.add(mainTable);
    win.open();

  } else {
    Ti.API.info("error:status code is " + this.status);
  }
};
xhr.onerror = function(e) {
  var error;
  error = JSON.parse(this.responseText);
  Ti.API.info(error.error);
};

xhr.send();
```

つぎに、それぞれのUI 要素の幅、高さ、色などの設定情報だけをまとめたstyle.jsを作成して、以下のように記述します

```javascript
// style.js

{"style":{
    "mainTable":{
      width: 320,
      height:480,
      backgroundColor:"#fff",
      separatorColor:"#ccc",
      left: 0,
      top: 0
    },
    "row":{
      width: 'auto',
      height:60,
      borderWidth: 0,
	  className:'entry',
      backgroundGradient: {
        type: 'linear',
        startPoint: {
          x: '0%',
          y: '0%'
        },
        endPoint: {
          x: '0%',
          y: '100%'
        },
        colors: [{
	  color: '#f8f8f8',
	  position: 0.0
	},{
          color: '#f2f2f2',
          position: 0.7
	},{
          color: '#e8e8e8',
          position: 1.0
	}]
      }
    },
    "textLabel":{
      width:250,
      height:30,
      top:5,
      left:60,
      color:'#222',
      font:{
	fontSize:16,
	fontWeight:'bold'
      }
    },
    "iconImage":{
      width:40,
      height:40,
      top:5,
      left:5,
      borderColor:"#bbb",
      borderWidth:1,
      defaultImage:"logo.png"
    },
    "view":{
      width: 'auto',
      height:60,
      backgroundGradient: {
        type: 'linear',
        startPoint: {
          x: '0%',
          y: '0%'
        },
        endPoint: {
          x: '0%',
          y: '100%'
        },
        colors: [{
	  color: '#f8f8f8',
	  position: 0.0
	},{
          color: '#f2f2f2',
          position: 0.7
	},{
          color: '#e8e8e8',
          position: 1.0
	}]
      }
    }
}}
```

## アプリケーション全体の設計

先ほどのやり方でもある程度ソースコードの見通しはよくなるかと思いますが、もう少し本格的な分割方法もあるのでそちらについてもご紹介したいと思います。

### 役割ごとにモジュールやクラスにまとめる

[フロントエンドJavaScriptにおける設計とテスト](http://hokaccha.github.io/slides/javascript_design_and_test/)というとてもわかりやすい資料がありそこで以下のようなことが書かれていました。

> JavaScriptによるアプリケーションの設計で重要なのはこの二つ
> 
> ModelとViewを明確に分ける
> 
> Viewを疎結合にする

Viewを疎結合にするための手段として UI コンポーネント単位でおおまかに分割するとわかりやすいかと思います。

UI コンポーネント単位といっても、Button やLabel のような細かい単位で分割するよりも、もう少し大きな単位で分けたほうが整理しやすくなるかと思います。

参考までに私が作ったTiQiitaのアプリでは、UI に関する部分については以下のように分割してます。

- mainTable.js
- menuTable.js
- alertView.js
- progressBar.js
- statusView.js
- webView.js
- window.js

上記にあげたファイルのうち、alertView.jsは、警告メッセージを表示することに特化しています。

具体的には

- alertViewの幅、高さ、背景色、文字の色などを設定する
- 警告メッセージを表示する
- ダイアログを表示する際には上から下にスライドするような動作をする

という処理を担ってます。

以下が
```javascript
var alertView;
var __bind = function(fn, me){ return function(){ return fn.apply(me, arguments); }; };
alertView = (function() {
  function alertView() {
    var warnImage;
    this.alertView = Ti.UI.createView({
      zIndex: 20,
      width: 320,
      height: 80,
      top: -80,
      left: 0,
      backgroundGradient: {
        type: "linear",
        startPoint: {
          x: "50%",
          y: "0%"
        },
        endPoint: {
          x: "50%",
          y: "100%"
        },
        colors: [
          {
            color: "#fe0700",
            offset: 1.0
          }, {
            color: "#f74504",
            offset: 0.5
          }, {
            color: "#e77208",
            offset: 0.0
          }
        ]
      }
    });
    warnImage = Ti.UI.createImageView({
      width: 50,
      height: 50,
      top: 10,
      left: 5,
      image: "ui/image/light_warn@2x.png"
    });
    this.message = Ti.UI.createLabel({
      text: "ネットワークが利用できないかQiitaのサーバがダウンしてるようです。",
      width: 200,
      height: 60,
      top: 10,
      left: 70,
      font: {
        fontSize: 14,
        fontWeight: 'bold'
      },
      color: "#fff"
    });
    this.alertView.add(warnImage);
    this.alertView.add(this.message);
    this.alertView.hide();
    return true;
  }
  alertView.prototype.editMessage = function(value) {
    return this.message.text = value;
  };
  alertView.prototype.getAlertView = function() {
    return this.alertView;
  };
  alertView.prototype.show = function() {
    return this.alertView.show();
  };
  alertView.prototype.animate = function() {
    this.alertView.show();
    statusView.top = -50;
    progressBar.hide();
    return this.alertView.animate({
      duration: 600,
      top: 0
    }, __bind(function() {
      return this.alertView.animate({
        duration: 2000,
        top: 1
      }, __bind(function() {
        return this.alertView.animate({
          duration: 600,
          top: -80
        }, __bind(function() {
          return this.alertView.hide();
        }, this));
      }, this));
    }, this));
  };
  return alertView;
})();
module.exports = alertView;
```

