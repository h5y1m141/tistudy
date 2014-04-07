# TiStudyについて

## はじめに

自分のような非エンジニアな人が、Titanium Mobileに興味を持って取り組み始めた時の参考書となるような情報として活用いただくことを想定してまとめてます。

## 想定する読者について

- ひとまずTitanium Mobileに興味を持ってチャレンジしはじめた方
- プログラミング経験はそこまで問わないですが「こんなスマートフォンのアプリが欲しい」と具体的なアイデアを持っていて、かつ、HTML/CSSのコーディング程度は可能なレベルの方を想定してます。

なお、Macの環境を想定してまとめてます。

## JavaScript基礎
- 関数編
- 変数について
- 制御文（for）
- if文
- ファイル分割のためのCommonJSのお話

## Titanium Classic環境

- モジュール化
      - CommonJSについて
      - Titanium Classic環境でも手軽に使えて開発効率があがるJavaScriptライブラリの紹介
        - moment.js:SNS系アプリのタイムラインにある「xx時間前に投稿」を手軽に実現する
        - underscore.js: JavaScriptの配列/オブジェクト操作の便利ユーティリティ
        - Jasmine：TDD/BDDのため
    - TableView/ListViewの活用
      - TableView/ListViewの基礎
      - TableView使ってデータ一覧表示
      - ListView使ってデータ一覧表示
      - TableView/ListViewの使い分け
    - Facebook/Twitterのようなソーシャルアカウント連携処理
      - TiPlatformConnectを使ったOAuth認証
      - YahooアカウントのようなTiPlatformConnect非対応のサービスを対応させる  
    - 体感速度をあげるための工夫
      - データソースとして利用する頻度が高いJSONオブジェクトを効果的に使いまわす
        - JSONオブジェクトのsortや該当データのみ抽出する処理など頻度高い処理を効率的に行うためのunderscore.jsの活用
      - 取得済のデータのローカルへのキャッシュ
        - 簡易な方法としてTi.App.propertiesの利用
        - SQliteの活用
      - 画像表示について
        - 非同期で画像を読み込む
        - 読み込みが遅く感じられた場合の最終手段（Titanium側のソースを編集）
