# 引っ張って更新処理を実装する


## はじめに

これまで作ってきたようなユーザインタフェースには**引っ張って更新**という処理を入れてあげたほうがユーザさんにとって使い勝手の良いアプリケーションというイメージに繋がると思います。

引っ張って更新の処理は、ゼロベースで作るのは色々大変なのですが、Titanium Mobile SDK 3.x になってからは、[Titanium.UI.RefreshControl](http://docs.appcelerator.com/titanium/3.0/#!/api/Titanium.UI.RefreshControl)というAPIが提供されており、これを利用することで簡単に実現できます。

なおこのAPIは**iOSのみ利用可能なため今回はiOSに絞って説明します**


## Titanium.UI.RefreshControlの使い方の解説

## QiitaのWebAPIからいつでも最新情報を取得できるように変更する


### qiita.jsのソースコード

これは従来から変更はありませんが、念のためソースコードを以下記載します

```javascript
exports.getLocalJSON = function(){
  var sample,file,body;
  sample = Ti.Filesystem.getFile(Ti.Filesystem.resourcesDirectory, "sample.json");
  file = sample.read().toString();
  body = JSON.parse(file);
  return body;
};
exports.getItems = function(callback){
	var xhr,qiitaURL,method;
	qiitaURL = "https://qiita.com/api/v1/items";
	method = "GET";
	xhr = Ti.Network.createHTTPClient();
	xhr.open(method,qiitaURL);
	xhr.onload = function(){
		var body;
		if (this.status === 200) {
			body = JSON.parse(this.responseText);
			Ti.API.info("number is :" + body.length);
			callback('ok',body);
		} else {
			Ti.API.info("error:status code is " + this.status);
		}
	};
	xhr.onerror = function(e) {
		var error;
		error = JSON.parse(this.responseText);
		Ti.API.info(error.error);
	};
	xhr.timeout = 5000;
	xhr.send();
};
```

### style.jsのソースコード

```javascript
exports.mainTable = {
	width: Ti.UI.FULL,
	height: Ti.UI.FULL,
	backgroundColor: "#fff",
	separatorColor: "#ccc",
	efreshControlEnabled: true,
	refreshControlTintColor: '#66cc99',
	refreshControlBackgroundColor: '#ccc', // optional
	left: 0,
	top: 0,
	zIndex:0
	
};
exports.row = {
	width: Ti.UI.FULL,
	height:60,
	borderWidth: 0,
	className:"entry"
};

exports.textLabel = {
	width:250,
	height:50,
	top:5,
	left:60,
	color:"#222",
	font:{
		"fontSize":16,
		"fontWeight":"bold"
	}
};

// これ以降追加する箇所

// これまでは単にタイトルのみ表示してたが、投稿者のアイコンを表示するように修正
exports.iconImage = {
	width:40,
	height:40,
	top:5,
	left:5,
	defaultImage:"logo.png"
};

// 引っ張って更新処理中の色を指定
exports.refreshControl = {
	tintColor:'red'	
};

// QiitaのWebAPIから情報を読み込んでいる状態を示すために ActivityIndicatorを配置しためので
// その設定値
exports.actInd = {
	top:"20%",
	left:"30%",
	height:Ti.UI.SIZE,
	width:Ti.UI.SIZE,
	zIndex:0,
	color: "#f9f9f9",
	backgroundColor:"#444",
	font: {
		fontFamily:'Helvetica Neue',
		fontSize:16,
		fontWeight:'bold'
	},
	message: 'Loading...',
	style:Ti.UI.iPhone.ActivityIndicatorStyle.DARK
};
```


### mainWindow.jsのソースコード



```javascript
exports.createWindow = function(){
	var win = Ti.UI.createWindow({
		title:"QiitaViewer"
	});
	
	// 起動時にWebAPIからデータを取得して投稿情報が取得できたらmainTableにセット

	getQiitaItems(function(){
		Ti.API.info("起動時にWebAPIからデータを取得しました");
	});
	win.add(mainTable);
	win.add(actInd);
	return win;
};


// これより下の箇所はこのmainWindow.js内では参照可能な変数だが、外部からは利用できない。
var style = require("style"),
    refreshControl,
    mainTable = Ti.UI.createTableView(style.mainTable),
    actInd = Ti.UI.createActivityIndicator(style.actInd),
    qiita = require("qiita");

refreshControl = Ti.UI.createRefreshControl(style.refreshControl);
mainTable.refreshControl = refreshControl;

refreshControl.addEventListener('refreshstart', function(){
	getQiitaItems(function(){
		refreshControl.endRefreshing();
		Ti.API.info("引っ張って更新処理が完了");
	});
});

var getQiitaItems = function(callback){
	var rows;
	if (Ti.Network.online === false){
		alert("利用されてるスマートフォンからインターネットに接続できないため情報が取得できません");
	} else {
		actInd.show();
		qiita.getItems(function(status,items){
			rows = createRows(items);
			mainTable.setData(rows);
			actInd.hide();
			return callback();
		});
	}
	
};
var createRows = function(items){
	var rows = [],_i,_len,style = require("style");
	for (_i = 0, _len = items.length; _i < _len; _i++) {
		row = Ti.UI.createTableViewRow(style.row);
		textLabel = Ti.UI.createLabel(style.textLabel);
		iconImage = Ti.UI.createImageView(style.iconImage);
		iconImage.image = items[_i].user.profile_image_url;
		textLabel.text = items[_i].title;
		row.add(textLabel);
		row.add(iconImage);
		rows.push(row);
	}
	return rows;
};
```


### app.jsのソースコード

mainWindow.jsの構造を見なおしたことで、app.jsはこれまでよりも更にシンプルになりました。

```javascript
var mainWindow,win;
mainWindow = require("mainWindow");
win = mainWindow.createWindow();	
win.open();		
```
