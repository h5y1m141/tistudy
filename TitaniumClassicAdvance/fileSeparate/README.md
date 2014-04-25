# ソースコードの保守性をあげる工夫

## はじめに

TableViewの上に、ImageViewやLabelを多数配置した以下の様なユーザインタフェースをもったアプリケーションを開発することが比較的多くあるかと思います。

![](../../image/TiQiita-01.png)

このようなインタフェースを実現するために以下のようにTitanium MobileのSDKのTi.UI系のAPIを利用して各オブジェクトの生成していきますが、幅、高さ、色などの指定をしていく都合上、ソースコードがが増えてややコードの見通しがしづらくなるかと思います。

```javascript
var i ,len ,row ,rows,textLabel,iconImage,imagePath;
rows = [];
for (i = 0, len = 10; i < len; i++) {
  row = Ti.UI.createTableViewRow({
    width: Ti.UI.FULL,
    height:60,
    borderWidth: 0,
	className:'entry',
    color:"#222"
  });
  textLabel = Ti.UI.createLabel({
    width:250,
    height:50,
    top:5,
    left:5,
    color:'#222',
    font:{
      fontSize:16,
      fontWeight:'bold'
    },
    text:body[i].title
  });
  imagePath = body[i].user.profile_image_url;
  iconImage = Ti.UI.createImageView({
    width:40,
    height:40,
    top:5,
    left:5,
    defaultImage:"logo.png",
    image: imagePath
  });

  row.add(textLabel);
  row.add(iconImage);
  rows.push(row);
}

```

保守性や拡張性を意識したソースコードにしておくことで、開発したアプリを継続的に改善してくためのモチベーション維持に繋がるのではないかと思っています。

Titanium MobileでのJavaScriptではモジュール化の仕組みが標準的に備わっており、CommonJSという仕様に従った書き方をすることで、開発しやすい形にファイル分割して開発することができます。

※ CommonJSに準拠した書き方は、Titanium Mobile固有というわけではなく、サーバサイドのJavaScript開発のNode.jsでも採用されています。

次の項でCommonJSスタイルの書き方について解説していきます。

