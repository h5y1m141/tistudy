# UI生成処理は別ファイルにする方法

## はじめに

Webサイト構築する際に、見栄えに関わる部分をHTMLから分離して、CSSに記述するというのは理解しやすい話しかと思います。このような考え方をTitanium Mobileでの開発で応用させることで、当初のソースコードから見通しの良いものになるかと思います。

## 幅、高さ、色などの設定情報だけを別ファイルにする

UI生成に関する処理と、それぞれのUI 要素の設定情報を

- Window、TableView、Label、Button のそれぞれのUI 要素の幅、高さ、色などの設定情報だけをまとめたファイルを作る（**仮にstyle.js**とします）
- UI 要素となるオブジェクトを生成する際に上記のstyle.jsに記述された該当の設定情報を反映させる

という形にすることで、当初のソースコードよりも見通しがよくなるかと思います。

以下具体的な方法についてサンプルコードを紹介しながら解説します

## ソースコード解説

まずそれぞれのUI 要素の幅、高さ、色などの設定情報だけをまとめたstyle.jsを作成して、以下のように記述します

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

次にapp.jsを以下のようにします。

```javascript
// app.js

var styleJS,file,style,xhr,qiitaURL,method,mainTable,win,view,colorSet;
styleJS = Ti.Filesystem.getFile(Ti.Filesystem.resourcesDirectory, "style.js"); // (1)
file = styleJS.read().toString();
style = JSON.parse(file);  // (2)
mainTable = Ti.UI.createTableView(style.mainTable);  // (3)
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
      row = Ti.UI.createTableViewRow(style.iPhoneRow);  // (5)
      textLabel = Ti.UI.createLabel(style.textLabel);   // (6)
      textLabel.text = body[_i].title;
      imagePath = body[_i].user.profile_image_url;
      iconImage = Ti.UI.createImageView(style.iconImage); // (7)
      iconImage.image = imagePath;
      row.add(textLabel);
      row.add(iconImage);
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

### ソースコード解説
1. ffdfd
2. ffdfd
3. ffdfd
4. ffdfd
5. ffdfd
6. ffdfd
7. ffdfd
