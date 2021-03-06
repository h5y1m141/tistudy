# イベント駆動型プログラミングの基礎

## はじめに

Titanium Mobileでのアプリケーション開発をする上で、人間の操作をきっかけとしてプログラムが実行される手法が多用されます。

そのような手法をイベント駆動型プログラミングと呼ばれ、スマートフォン向けのアプリケーション含めてGUIアプリケーションを開発する上で必要となる考え方になるので、以下順番に解説をしていきます。

## 逐次駆動型とイベント駆動型

関数編で以下のようなサンプルコードを記載しました。

```javascript
var tabGroup,tab1,tab2;
Titanium.UI.setBackgroundColor('#000');
tabGroup = Titanium.UI.createTabGroup();
tab1 = createTabElement('1!!!');
tab2 = createTabElement('2!!!');
tabGroup.addTab(tab1); 
tabGroup.addTab(tab2); 

tabGroup.open();

function createTabElement(titleNumber){
    var win,label;
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

    var tab = Titanium.UI.createTab({  
        icon:'KS_nav_views.png',
        title:"Tab" + titleNumber,
        window:win
    });
    return tab;
};
```
このように上から順番に逐次実行されるので、このようなものを逐次駆動型のプログラムと呼びます。

一方で、スマートフォン向けのアプリケーションの場合で例えば、labelをクリックした場合に画面上に何かの文字を表示するといようなプログラムを考えた場合に、以下のようなソースコードになります。

```javascript
var tabGroup,tab1,tab2;
Titanium.UI.setBackgroundColor('#000');
tabGroup = Titanium.UI.createTabGroup();
tab1 = createTabElement('1!!!');
tab2 = createTabElement('2!!!');
tabGroup.addTab(tab1); 
tabGroup.addTab(tab2); 

tabGroup.open();

function createTabElement(titleNumber){
    var win,label;
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
    label.addEventListener('click',function(e){
      alert("this is a label");
    });


    win.add(label);

    var tab = Titanium.UI.createTab({  
        icon:'KS_nav_views.png',
        title:"Tab" + titleNumber,
        window:win
    });
    return tab;
};
```

上記のソースコードは基本的には上から順番に解釈されますが

```javascript
label.addEventListener('click',function(e){
  alert("this is a label");
});
```

という部分だけは、人間の操作、この場合には、labelがタッチされるまでずっと待機状態にあります。そして、**人間の操作というものをイベントとして捉えて**そのイベントをきっかけとしてプログラムが実行されます。


## イベント駆動型のプログラミングはどんな所で利用されているのか？

最初に触れていますが、スマートフォンアプリに限らず、GUIアプリケーションを開発する上でイベント駆動型のプログラミングは比較的利用されており一番馴染みがある所ではブラウザ上で動作してるGoogleMapsがイメージしやすいかと思います。


## イベントとしてどのようなものがあるのか？

- click：ある要素をクリックした時のイベント
- swipe：画面を左（もしくは右）にスワイプした時のイベント

などがありますが、それぞれのユーザインタフェース毎に登録できるイベントの内容は異なってきます。

詳しいことは 開発元のAppceleratorの公式ドキュメントを参照して下さい。


