---
title: TableViewの区切の線を無くす方法
layout: post
pict: no-separator.png
category : ui
desc: Path風のUIを実現したい場合などに区切り線を非表示にしたいケースが出てくるかと思います。separatorStyleというプロパティ活用することで実現できます
---
{% include JB/setup %}

![リスト形式の画像イメージ](/img/no-separator.png)

例えば、Path風のUIを実現したい場合などにこの区切り線を非表示にしたいケースが出てくるかと思います。


## 実現方法
区切り線を無くす方法はとても簡単でseparatorStyleというプロパティに定数のNONEを設定します

{% highlight javascript %}
var tableView = Ti.UI.createTableView({
  separatorStyle:'NONE' //数値の0でも同様のことが実現できます
});
{% endhighlight %}

こうすることで区切り線を非表示にすることができます
