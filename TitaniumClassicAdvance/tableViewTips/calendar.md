## はじめに

TitaniumのMarketPlaceでカレンダーモジュールがいくつか存在してます。
AGCalendar Moduleというのを見つけてこれは良さそうと思ったが、標準のカレンダー的なUIではなくリスト形式のカレンダー表示が欲しかったので一旦保留

1ヶ月をリスト形式で表示するようなカレンダーモジュールがなかったので作ってみました。

## app.jsについて
読み込む側のapp.jsはこのようなものを準備します
<script src="https://gist.github.com/2939329.js?file=app.js"></script>

## カレンダーの元となる日付データの生成

<script src="https://gist.github.com/2939339.js?file=listCalendar.js"></script>

## 上記で生成されたデータを下にして、TableView利用してUIを生成

<script src="https://gist.github.com/2939341.js?file=weeklyCalendar.js"></script>
