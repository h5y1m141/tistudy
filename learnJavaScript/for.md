# JavaScript基礎:制御文（for）

## はじめに


過去私がが作ってきたアプリを振り返るとWebAPIから取得したJSON形式のデータを**１つづつ取り出して**処理したり、アプリ側のデータベースの機能を利用してキャッシュした情報から**データを１つづつ取り出して**TableViewRowに追加していくような処理をすることが比較的多くありました。

作るアプリによっては、他の制御文を利用するケースというのももちろんありますが、Titanium Mobileが得意とするWeb APIと連携するスマフォアプリ開発に限ればまずは

- ループ処理のための for
- if

の２点をおさえておくのが良いかと思っております。

そこで、まずはサンプルコードを示しながら、for ループの使い方について解説をしていきます。

## for ループを使って複数のTabを生成するサンプル

前回は、関数に渡す引数を以下のようにすることでI am Window1!!! や、I am Window2!!!というラベルが表示されるタブを２つ生成しました

```javascript
var tab1 = createTabElement('1!!!');
var tab2 = createTabElement('2!!!');
```

この処理を応用させて、forループを使ってI am Window1からI am Window5というラベルがそれぞれ表示される５つのタブを生成するようにしてみます。

```javascript
var tabGroup,i,tab;
Titanium.UI.setBackgroundColor('#000');
tabGroup = Titanium.UI.createTabGroup();
for(i = 1; i < 6; i++){  //(1)
	tab = createTabElement(i + '!!!'); // (2)
	tabGroup.addTab(tab);  	
}
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

1. for ループの処理を開始します。今回はI am Window1からI am Window５まで表示したいので、forループのカウンターとして利用してる変数のiの初期値として、1を設定しています。また5までは表示したいので、ループのカウンターとして利用してる変数のiの値が6より小さくなるまで繰り返します。
2. createTabElement()に渡す引数の箇所に、変数iと文字列を組み合わせて渡すことで、変数iには１から５までの数値が順番に入っていきます

上記実行すると、このようにタブが５つ生成されます。

<p><span itemscope itemtype="http://schema.org/Photograph"><img src="http://cdn-ak.f.st-hatena.com/images/fotolife/h/h5y1m141/20140324/20140324064516.png" alt="f:id:h5y1m141:20140324064516p:plain" title="f:id:h5y1m141:20140324064516p:plain" class="hatena-fotolife" itemprop="image"></span></p>
