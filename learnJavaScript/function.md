# JavaScript基礎:関数編

## はじめに

関数の説明をする前にTitanium Studioを使って新規プロジェクトの設定完了後に自動的に出来上がるapp.jsのソースコードを簡単に解説します。

```javascript
// this sets the background color of the master UIView (when there are no windows/tab groups on it)
Titanium.UI.setBackgroundColor('#000');
// create tab group
var tabGroup = Titanium.UI.createTabGroup(); // (1)
//
// create base UI tab and root window
//
var win1 = Titanium.UI.createWindow({   // (2)
    title:'Tab 1',
    backgroundColor:'#fff'
});
var tab1 = Titanium.UI.createTab({      // (3)
    icon:'KS_nav_views.png',
    title:'Tab 1',
    window:win1
});
var label1 = Titanium.UI.createLabel({  // (4)
    color:'#999',
    text:'I am Window 1',
    font:{fontSize:20,fontFamily:'Helvetica Neue'},
    textAlign:'center',
    width:'auto'
});
win1.add(label1);                       // (5)
// 以下省略
```

1. Tabを格納するためのTabGroupを生成tabGroupという名前を付けてます
2. タイトルが「Tab 1]、背景色が白（#fff）のWindowを生成win1という名前を付けてます
3. タイトルが「Tab 1]、アイコンを指定、対応するWindowとしてWin1を指定したTabを生成。tab1という名前を付けてます
4. 表示内容「I am Window 1」、表示色を灰色（#999）、フォント名＆サイズを指定したLabelを生成。label1という名前を付けてます
5. win1の内部にlabel1を配置してます

![UI配置イメージ](https://raw.github.com/h5y1m141/streetAcademy/master/image/1stStep-008.png)

これと似た処理が、省略された箇所で実施されてますが、こういう似たような処理をコピーしながら適宜修正して・・という作業をしててうっかり作業を間違える可能性もあるかと思います。

このようにほぼ同じよな処理を複数こなす場合には、プログラムの中で小さいプログラムを定義するようなことがどのようなプログラミング言語でも実現できこれを **関数**と言います。

こう書いてもすぐにイメージできないかもしれないので、具体的なコードを示しながら順番に解説していこうと思います。

## createTabElementというを関数を使って書き換える

Label、Window、Tabにそれぞれ表示する文字が違いますが、Ti.UI.Label、Ti.UI.Window、Ti.UI.Tabを生成するという意味においては、共通した処理になるため、このUI要素を生成する部分を関数として定義することにします。

### 関数の名前付けについて

関数の名前を考える際に

- 先頭文字には数字の0から9は使用できない
- JavaScriptのプログラミング言語ですでに定義された用語（ifとかvarとかforなど）は使用できない

というのを避ければOKです。

一応日本語も使えるようですが通常は英単語を組み合わせて、他の人にもその関数がどのような処理を行うのかおおまかに把握できる名前にするのが一般的なので、今回は**createTabElement** という名前にしました。

### app.jsを関数を利用した形に書き換える

既存のapp.jsを以下のように書き換えます

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

上記のソースコードの以下の部分が今回キモになるポイントです

```javascript
function createTabElement(titleNumber){
  // 実際の処理
}
```

JavaScriptの場合、関数を利用する場合には

```javascript
  function 関数名(引数){}
```

という形で記述します。

引数の所が少しわかりづらいかもしれないので、補足説明します

```javascript
var tab1 = createTabElement('1!!!');
function createTabElement(titleNumber){
	var win;
	win = Titanium.UI.createWindow({
		title:"Tab" + titleNumber, // (1)
		backgroundColor:'#fff'
	});
    // 以下省略
}
```

(1)の所に注目いただくとわかるかと思いますが、引数として設定されたtitleNumberという変数がここに代入されます。

createTabElement関数に、**'1!!!'** としてるため、Ti.Ui.TabのTitleには、Tab'1!!!'が設定されます。
