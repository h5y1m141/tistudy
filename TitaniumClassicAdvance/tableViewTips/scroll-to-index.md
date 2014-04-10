## はじめに


RSSリーダーのようなアプリを作る時に

- メインウィンドウで一覧画面を生成
- 該当エントリをタッチして、詳細情報を表示するために別ウィンドウに画面遷移
- 再度メインウィンドウに戻る

というような機能を実装するケースがあると思います。

メインウィンドウに表示する情報量が多くなると一覧画面をスクロールしてから、別ウィンドウに画面遷移して、再度メインに戻る時に、元のスクロール箇所を表示するようにしたほうがユーザにとっては使いやすく感じると思うのでその実装方法について解説します

## 解説

元のスクロール箇所に戻るためにはTableViewのscrollToIndex()を利用することで実現できます。


今回のサンプルは一覧情報⇔詳細画面という２つのウィンドウ間を行き来するものを想定していますが、以下２つの部分についての実現方法について難しく感じたため、本題から少し外れますがその点について補足の解説をしておきます

- currentTab nullの問題
- メインウィンドウに戻る時のイベント発火


### currentTab nullの問題

### メインウィンドウに戻る時のイベント発火

メインウィンドウに戻る時に元のスクロール箇所を表示するには前述した通りTableViewのscrollToIndex()を利用すればOKなのですが、このメソッドを呼び出すためのEventListenerの割り当て方法に一工夫必要になりました。



## ソースコード全体

### app.js

{% highlight javascript %}
var tabGroup = Titanium.UI.createTabGroup({});
tabGroup.addEventListener('focus', function(e){
  tabGroup._activeTab = e.tab;
});
var tab1 = Titanium.UI.createTab();
var win1 = Titanium.UI.createWindow({
  title:'ブロガー一覧',
  backgroundColor:'#fff'
});


var bloggers = require('bloggers').data;
var rows=[];
for(var i=0;i<bloggers.length;i++){
  var row = Ti.UI.createTableViewRow({
    backgroundColor:'#eee',
    width:320,
    borderColor:'#999',
    height:60
  });

  var nameLabel = Ti.UI.createLabel({
    color:'#595959',
    font:{
      fontSize:16,
      fontWeight:'bold'
    },
    text:bloggers[i].name,
    top:10,
    left:20,
    width:100,
    height:20,
    zIndex:2
  });
  row.add(nameLabel);
  var blogTitleLabel = Ti.UI.createLabel({
    color:'#595959',
    font:{
      fontSize:12
    },
    text:bloggers[i].blogTitle,
    top:32,
    left:20,
    width:150,
    height:20,
    zIndex:2
  });
  row.add(blogTitleLabel);

  var line = Ti.UI.createLabel({
    bottom:0,
    width:320,
    left:0,
    height:1,
    text:'',
    backgroundColor:'#666'
  });
  row.add(line);
  var line1 = Ti.UI.createLabel({
    bottom:1,
    width:320,
    left:0,
    height:1,
    text:'',
    backgroundColor:'#eee'
  });
  row.add(line1);

  var halfView = Ti.UI.createView({
    backgroundGradient:{
      type: 'linear',
      startPoint: { x: '50%', y: '0%' },
      endPoint: { x: '50%', y: '100%' },
      colors: [
        { color: '#eee', offset: 0.0},
        { color: '#999', offset: 0.3},
        { color: '#444', offset: 1.0 }
      ]
    },
    width:320,
    left:0,
    height:30,
    bottom:0,
    opacity:0.3
  });
  row.add(halfView);
  rows.push(row);
}
var tableView = Ti.UI.createTableView({
  separatorColor: '#eee',
  maxRowHeight:60,
  data:rows
});



tableView.addEventListener('click',function(e){
  var index = e.index;
  var name = Ti.UI.createLabel({
    width:200,
    height:40,
    top:5,
    left:5,
    text:e.row.children[0].text,
    color:'#595959',
    font:{
      fontSize:16,
      fontWeight:'bold'
    }
  });
  var title = Ti.UI.createLabel({
    width:200,
    height:40,
    top:30,
    left:5,
    color:'#595959',
    font:{
      fontSize:12,
      fontWeight:'bold'
    },
    text:e.row.children[1].text
  });

  var newWindow = Ti.UI.createWindow({
    title:e.row.children[1].text,
    backgroundColor:'#fff'
  });
  newWindow.add(name);
  newWindow.add(title);
  /*
    遷移後のウィンドウからメイン画面に戻る時に、Backボタンを独自に準備
    してそのボタンに対してEventListenerを割り当てることで、
    tableView.scrollToIndex()を呼び出すことが出来る。
    以下情報を参考にしてます
    http://developer.appcelerator.com/question/21721/add-listener-on-winbackbutton
*/
  var b = Titanium.UI.createButton({title:'Back'});
  newWindow.leftNavButton = b;
  b.addEventListener('click', function(){
    Ti.API.info(index);
    tableView.scrollToIndex(index,{animated:true});
    newWindow.close();
  });

  tabGroup._activeTab.open(newWindow,{animated:true});


});
win1.add(tableView);
win1.hideTabBar();
tab1.window = win1;
tabGroup.addTab(tab1);
tabGroup.open();
{% endhighlight %}



### bloggers.js

{% highlight javascript %}
var bloggers = [
  {
    name:'横田　真俊',
    blogTitle:'毎日がアップデート',
  },
  {
    name:'大橋　悦夫',
    blogTitle:'SOHO考流記',
  },
  {
    name:'宮原　徹',
    blogTitle:'オープンでいこう',
  },
  {
    name:'太木　裕子',
    blogTitle:'幸せはどこにある？',
  },
  {
    name:'金山　めぐみ',
    blogTitle:'Megumilkめもりー',
  },
  {
    name:'金澤　創平',
    blogTitle:'20歳からのキャリア考',
  },
  {
    name:'上原　仁',
    blogTitle:'どこでもドアを創りたい',
  },
  {
    name:'大谷　弘喜',
    blogTitle:'踊るプログラマ物語',
  },
  {
    name:'美谷　広海',
    blogTitle:'世界を巡るFool on the web',
  },
  {
    name:'日比　知子',
    blogTitle:'家で働くママ日記',
  },
  {
    name:'佐々木　正吾',
    blogTitle:'作家の作業日報',
  },
  {
    name:'小山田　浩',
    blogTitle:'キャリア形成のお手伝い',
  },
  {
    name:'藤原　真由美',
    blogTitle:'人は見たものしか信じない',
  },
  {
    name:'堀川　貴満',
    blogTitle:'チェケラッ！',
  }

];

var exports = {
  data:bloggers
};
{% endhighlight %}


