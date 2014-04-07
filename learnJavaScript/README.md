## はじめに

自分のような非エンジニアな人が、Titanium Mobileに興味を持って取り組み始めてつまづく要因が色々あると思うのですが、JavaScriptの基礎的な部分の理解がちょっと足りないことによるものというのは結構大きいかなと思ってます。

JavaScriptの基礎的な部分というのは、人によって様々あるかと思いますが、非エンジニアな人にとっては

- 関数
- 制御文

などが挙げられると思ってます。

裏を返せば、この部分の理解をある程度深めておくことで自分が作りたいと思ったものを、調べながらなんとか作れるようになれるんじゃないかなとふと思いつき、非エンジニアな人におくるJavaScriptの基礎みたいなチュートリアルを考えてみました。


## とりあげるJavaScriptの知識

上記でも記載しましたが、以下についてとりあげます

- 関数
- 制御文
    - for ループ
    - if



## ここではとりあげないJavaScriptの知識

以下については、とりあげません。

- 無名関数（特に function(){..}()みたいな記号だらけのやつ
- JavaScriptでのクラスを実現する方法
- JavaScriptのthisのスコープ
- JavaScriptのCommonJSについて

ただ、自分の体験上、画面遷移を伴い、かつ、複数ウィンドウを管理するようなアプリを開発する場合には上記に上げた内容を理解してないと、開発がはかどらない可能性があるかと思ってるのでいつかとりあげてみたいとは思ってます。(*1）

## 想定対象者

Titanium Mobileに興味を持って実際にいじりはじめた非エンジニアな人を想定しています。

## チュートリアル始める前の準備

新規プロジェクト作成後のひな形のアプリをベースにしたサンプル

ある程度ベースとなるアプリを基盤に、少しづつ機能を追加してくようなアプローチの方が理解が進みやすいと考え、当初は、オリジナルなサンプルアプリを考えたのですが、考えるのが大変だったので、Titaniumで新規プロジェクト作成後に出来上がるひな形のアプリをベースにすることにしました。

ひな形のアプリなら、環境構築した人にとってもすぐに利用できる題材で敷居も低いかなと思ったので、ひとまずこれを利用することにします。

### 念のためTitanium Studioでのproject設定の流れ

TitaniumStudioを起動した後、File→New→Titanium Mobile Projectと進みます

![プロジェクト設定スタート画面](https://raw.github.com/h5y1m141/streetAcademy/master/image/1stStep-project-configuration001.png)

Project Template画面が標示されたら、Default Projectを選択します

![テンプレート選択](https://raw.github.com/h5y1m141/streetAcademy/master/image/1stStep-project-configuration002.png)

プロジェクト設定画面が表示されますので任意の名前、idを設定して下さい

![プロジェクト名とID入力画面](https://raw.github.com/h5y1m141/streetAcademy/master/image/1stStep-project-configuration003.png)

しばらくして設定が完了すると、以下のような画面が表示されればOKです

![設定完了後の画面](https://raw.github.com/h5y1m141/streetAcademy/master/image/1stStep-project-configuration004.png)
