# CommonJSについて

## はじめに

ソースコードの保守性をあげる工夫という章でファイルの分割方法について解説しましたが、もう少し本格的な分割方法もあるのでそちらについてもご紹介したいと思います。

例えば、Ti.UI.LableのようなUIを複数利用する場合、それぞれに、幅、高さ、色、位置という値を適宜設定する必要があるため、それなりの量のソースコードになってきます。


```javascript
var win, label1, label2, label3 win2;
win = Ti.UI.createWindow({
  title:"Window",
  backgroundColor:'#fff'
});
label1 =Ti.UI.createLabel({
  color:'#999',
  text:'I am Label 1',
  font:{fontSize:20,fontFamily:'Helvetica Neue'},
  textAlign:'center',
  top:0,
  left:5,
  width:Ti.UI.FULL
});
label2 =Ti.UI.createLabel({
  color:'#666',
  text:'I am Label 2',
  font:{fontSize:20,fontFamily:'Helvetica Neue'},
  textAlign:'center',
  top:50,
  left:5,
  width:Ti.UI.FULL
});
label3 =Ti.UI.createLabel({
  color:'#333',
  text:'I am Label 3',
  font:{fontSize:20,fontFamily:'Helvetica Neue'},
  textAlign:'center',
  top:100,
  left:5,  
  width:Ti.UI.FULL
});
win.add(label1);
win.add(label2);
win.add(label3);
win2 = Ti.UI.createWindow({
  //省略
});

function createSomething(){
  //省略
}
function createSomething1(){
  //省略
}
function getSomething(){
  //省略
}
```

app.js にソースコードすべてを記述していくと、どこで、何の処理をしてるのかが頭のなかで把握しづらくなってきてしまいます。

Titanium MobileでのJavaScriptでは、開発しやすい形にファイル分割して開発するモジュール化の仕組みが標準的に備わっており、CommonJSという仕様に従った書き方をすることで、開発しやすい形にファイル分割して開発することができます。

※ CommonJSに準拠した書き方は、Titanium Mobile固有というわけではなく、サーバサイドのJavaScript開発のNode.jsでも採用されています。



## CommonJSスタイルの書き方の解説

具体的なサンプルを取り上げながらCommonJSスタイルの書き方について解説していきます。

### プロジェクトの構成

Resources直下に、ui.jsというファイルを新規に作成します。作成後は以下の様なフォルダ構成になるかと思います。
```sh
├── CHANGELOG.txt
├── LICENSE
├── LICENSE.txt
├── README
├── Resources
│   ├── KS_nav_ui.png
│   ├── KS_nav_views.png
│   ├── app.js
│   ├── iphone
│   └── ui.js
├── build
│   └── iphone
├── manifest
└── tiapp.xml
```

### ui.js の中身

ui.js を以下のように記述することで、UI要素を生成する箇所だけをapp.jsから切り離すことが出来ます。

```javascript
exports.createTabElement = function(titleNumber){
  var win, label, tab;
  win = Titanium.UI.createWindow({ 
      title:"Tab" + titleNumber,
      backgroundColor:'#fff'
  });

  label = Titanium.UI.createLabel({
      color:'#999',
      text:'I am Window' + titleNumber,
      font:{fontSize:20,fontFamily:'Helvetica Neue'},
      textAlign:'center',
      width:'auto'
  });

  win.add(label);

  tab = Titanium.UI.createTab({ 
      icon:'KS_nav_views.png',
      title:"Tab" + titleNumber,
      window:win
  });
  return tab;
};
```

### app.js を編集する

UI要素を生成する部分はui.jsにて行うようにしたことでapp.jsの方を編集する必要が出てきます。

具体的には

- app.js内で処理していたUI要素を生成する部分のコードは削除
- ui.jsを読み込みこちらの関数を利用してUI要素を生成する

という流れになります。

編集した結果以下の様なコードになります。

```javascript
var tabGroup, tab1, tab2, ui;
Titanium.UI.setBackgroundColor('#000');
ui = require('ui');
tabGroup = Titanium.UI.createTabGroup();
tab1 = ui.createTabElement('1!!!');
tab2 = ui.createTabElement('2!!!');
tabGroup.addTab(tab1); 
tabGroup.addTab(tab2); 
tabGroup.open();
```

### ソースコードの解説

ui.jsとapp.jsの対応関係を絵にしてみました

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/h/h5y1m141/20140402/20140402065718.png" alt="f:id:h5y1m141:20140402065718p:plain" title="f:id:h5y1m141:20140402065718p:plain" class="hatena-fotolife" itemprop="image"></span></p>

1. ui.jsの exportsというオブジェクトに、app.jsから呼び出したい関数の名前（createTabElement ）をつけてあげます。このようにすることでapp.js内では、xx.createTabElement('1!!!')という形でui.jsで定義した機能を利用することが出来ます。
2. app.jsからui.jsを読み込む時には、require()という関数を利用して任意の変数に格納します。（この場合にはuiという変数に格納してます）。なおui.jsの拡張子のjsを取って require('**ui**')とします。


