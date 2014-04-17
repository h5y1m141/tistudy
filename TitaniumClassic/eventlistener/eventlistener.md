# イベントリスナーを設定してみましょう

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


