---
title: 微妙な明度差のグラデーションを活用
layout: post
category: ui
pict: tableViewGradientSample.png
desc: 背景色に微妙な明度差のグラデーションにすることで見た目の印象がかなり変わります。backgroundGradientプロパティを活用することがそれが実現できます
tags : [view]
---
{% include JB/setup %}

## はじめに

少し凝ったUIデザインにしたい場合に、Photoshop/Illustratorなどの画像処理ソフトを活用して、素材となる画像を準備した上で、TableViewの背景画像として指定することがあるかと思います。

TableViewの背景画像使わなくても、backgroundGradientプロパティをうまく使うことで、以下の様な仕上がりに出来るのでその方法について解説したいと思います

![画像イメージ](/img/tableViewGradientSample.png)


## backgroundGradientとは？

背景に対してグラデーション適用できるプロパティです。

TableViewだけではなく、View、Buttonなどでも利用できます。詳細はAPI Documentで、backgroundGradient をキーワードに検索をしてみて下さい

## backgroundGradientの使い方


### typeについて

ここで指定できる値は、’linear’か’radial’のどちらかになります。今回のように直線でグラデーションという場合にはlinearを指定しますが、radial使うと複雑なグラデーションを指定出来るようです

### startPointとかendPoint

例えば、

{% highlight coffeescript %}
backgroundGradient:
  type: 'linear'
  startPoint:
    x:'0%'
    y:'0%'
  endPoint:
    x:'0%'
    y:'100%'
{% endhighlight %}

のようにstartPoint、endPointのxの値は同じに設定して、yの値を0→100%のようにすることで以下のように縦方向のグラデーションが設定されます

![画像イメージ](/img/tableViewGradientVertical.png)

また

{% highlight coffeescript %}
backgroundGradient:
  type: 'linear'
  startPoint:
    x:'0%'
    y:'0%'
  endPoint:
    x:'100%'
    y:'0%'
{% endhighlight %}

のようにstartPoint、endPointのxの値を0→100%のにして、yの値は同じものに設定することで以下のように横方向のグラデーションが設定されます

![画像イメージ](/img/tableViewGradientHorizontal.png)


### colorsプロパティのcolorとpositionについて

グラデーションの配色具合を調整しますが、実際のコードをサンプルにしながら順番に解説します。


{% highlight coffeescript %}
backgroundGradient:
  type: 'linear'
  startPoint:
    x:'0%'
    y:'0%'
  endPoint:
    x:'0%'
    y:'100%'
  colors: [
    color: '#222'
    position: 0.0
  ,      
    color: '#363636'
    position: 0.3
  ,      
    color: '#dfed33'
    position: 1.0
  ]
{% endhighlight %}

typeをlinearにしてstartPointのx,yを同じ値にして、yの値を0→100%にしているので、直線で縦方向にグラデーションがかかります。

さて、メインとなるcolorsの所ですが

{% highlight coffeescript %}
  colors: [
    color: '#222'
    position: 0.0
  ,      
    color: '#363636'
    position: 0.3
  ,      
    color: '#dfed33'
    position: 1.0
  ]
{% endhighlight %}

このように、colorと positionの２つのプロパティを持つオブジェクトを1つ以上含んだ配列を指定します

上記の例の場合

- 塗りつぶしの開始位置から30%まで:#222→#363636のグラデーション
- 30%から塗りつぶしの終了位置:#363636→#dfed33のグラデーション

となり、画面上ではこのような表示になります

![黄色から黒にグラデーションされている画面キャプチャ](/img/tableViewGradientVerticalYellow.png)

---



## ソースコード全体

### app.coffee

{% highlight coffeescript %}
bloggers = require('bloggers').data
gradientTable = require("ui/gradientTable")
tableView = new gradientTable(bloggers)


win = Ti.UI.createWindow
  backgroundColor:'#fff'

win.add tableView
win.open()
{% endhighlight %}

### ui/gradientTable.coffee
{% highlight coffeescript %}
class gradientTable
  constructor:(bloggers) ->
    tableView = Ti.UI.createTableView
      left:0
      top:0
      width:320
      backgroundColor:'#3c3c3c'
      separatorStyle:"NONE"
      
    rows = []
    for blogger in bloggers
        
      row = Ti.UI.createTableViewRow(
        backgroundGradient:
          type: 'linear'
          startPoint:
            x:'0%'
            y:'0%'
          endPoint:
            x:'0%'
            y:'100%'
          colors: [
            color: '#393939'
            position: 0.0
          ,      
            color: '#363636'
            position: 0.5
          ,      
            color: '#333'
            position: 1.0
          ]
        width:320
        height:60
      )

      nameLabel = Ti.UI.createLabel(
        color:'#bfbfbf'
        font:
          fontSize:16
          fontWeight:'bold'

        text:blogger.name
        top:10
        left:20
        width:100
        height:20
        zIndex:2
      )
      row.add nameLabel
      
      blogTitleLabel = Ti.UI.createLabel(
        color:'#696969'
        font:
          fontSize:12
        text:blogger.blogTitle
        top:32
        left:30
        width:150
        height:20
        zIndex:2
      )
      row.add blogTitleLabel

      rows.push row

    tableView.setData rows
    tableView.addEventListener('swipe',(e) ->
      Ti.API.debug e.direction
    )
    return tableView

module.exports = gradientTable        
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

