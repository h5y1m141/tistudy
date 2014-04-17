# TiStudyについて

## はじめに

Titanium Mobileに興味を持って取り組み始めた時の参考書となるような書籍はいくつか出版されてますが、出版された当時のTitanium MobileのSDKのバージョンが一世代前のものだったこともあり、現在推奨されているような開発スタイルになってないケースがあったりします。

また最新バージョンに適応した情報はインターネット上に多数存在しているのですが情報としてまとまってないため、初めてTitanium Mobileでの開発にチャレンジする人にとって少し敷居が高い状況になってるかと思ってます。

このサイトでは、Titanium Mobileに興味を持って、開発に着手しはじめた方を想定読者として念頭に置き、私が作った[Qiita](https://qiita.com)のビューワーアプリの[TiQiitaのアプリソースコードの一部を題材](https://github.com/h5y1m141/TiQiita)に取り上げて、大きく以下３点について学べることを意識してまとめてます。

- JavaScriptの基礎を抑えた上でTitanium Classic環境で簡単なアプリケーションが開発できる基礎知識
- Titanium Classic環境でTiQiitaのようなアプリケーションを開発する上で抑えておくべき知識
- Appceleratorの公式のMVCフレームワークのAlloyについて学びつつ、Titanium Classic環境からAlloyに移行する方法

このサイトでは、Titanium Mobileの開発環境を整える方法については取り上げていませんが、Titanium Mobileユーザ会にて[インストールガイド](http://titanium-install-guide-ja.github.io/)をまとめているため環境構築はそちらをご覧ください。

## 想定する読者について

- ひとまずTitanium Mobileに興味を持ってチャレンジしはじめた方
- Titanium Mobileの環境構築を終えて、サンプルアプリのコードを少しいじったがそこから先に進めない方


## 開発環境などの情報

執筆時点の2014年4月15日時点では以下の環境にて開発、動作確認をしております

- 開発環境
    - Mac OS X 10.8.5
    - Titanium SDK 3.2.0.GA
- アプリ動作確認環境    
    - iPhone
        - iPhone Simulator Retina 4 inch（iOS 7.0.3）
        - 実機はiPhone5(au) iOS 7.1にて確認
    - Android 
        - GenyMotion上のNexus4 4.2.2 API17

## その他：Webサイトとしてまとめてる理由

当初は、AmazonのKindle Direct Publishing（通称KDP）使って電子書籍化しようと思ったのですが、コンテンツの不備があったり、追記して欲しい内容があった場合に気軽にコメント出来るような仕組みがあるものを採用したほうが、より良いものになるかと思いました。

このサイトのオリジナルの原稿はGitHub上で公開しているので、コンテンツの不備があったり、追記して欲しい内容があった場合にはそちらにコメントしていただくか、Pull Requestしていただければ適宜反映していこうと思ってます。

なおこのサイト自体は、上記のMarkdown形式で書かれてる原稿をGitBookというツールを使ってHTML/CSS/JavaScriptに変換しています。


