# Dojo Day24
今日のテーマ  
**「使う側が使いたいように使う」**

## お品書き
- 委譲を使った Adopter パターン
- ~~Facade パターン~~

## タイムスケジュール
- 10:30 - 10:35 本日の進め方
- 10:35 - 10:45 解説(Adapterのみ。Facadeは次回)
- 10:45 - 10:55 課題
- 10:55 - 11:00 質疑応答

## 委譲を使ったAdopterパターン
Day21では、インターフェースの継承を使ってAdopterパターンを実装しました。  
今回は、Seito クラスが外部ライブラリにあって変更できないケースを想定して、  
委譲(Delegation)を使ってAdopterパターンを実装します。

### 課題
`day24.employee.EmployeeMessageService` でSyainクラスも扱えるようにしてください。　　


## 参考
### 継承より委譲（先週のAdapterのときに出てきた雑談の話）
ただ単にコードを共通化したいだけであれば継承を使うべきではありません。  

継承にすると、2つのクラスの関係は単純に利用する関係よりも依存度が高くなります。  
そのクラス関係に汎用と特化の関係ができるからです。  
継承を使う場合は、親クラスが子クラスの汎用になっているか(きれいなis-a関係になっているか)
が重要なポイントです。  
前回話題になった、「abstractクラスを使って継承するな」という話は、is-a関係をきちんと設計できるようになるまではむやみに継承を使わないでねというふうに理解していただければ良いかなと思います。

では、コードを共通化したい場合はどうするのか？  
共通処理を別クラスに追い出して、そのクラスから処理を呼び出す「委譲」を使ってください。

そもそも、関係の薄いクラスに共通処理が出てくるということは、その共通処理は別のクラスが責任を持つべきです。  

責任を外部に追い出してあげて、各クラスは責任を持つクラスから処理を呼び出してあげる。
これにより、同じ処理を何度も書くことなく、かといって、関係の薄いクラス同士が依存することを防いでくれます。

### 結合度と凝集度の話
きっと他のだれかがやってくれるか、ググってください。


## Facadeパターン
Facadeを簡単に一言でいうと、「窓口」です。
(※直訳は別の訳になりますが)

一つのことを実現するために様々な処理を決まった手順で呼び出す必要があったりする場合は、
Facadeパターンで窓口を用意して上げることで、窓口を使う側が多くのことを意識せずに実現できるようになります。

### 例題
独自のバージョン管理ツールAPIのFacadeを作って、`FileEditService` から利用する

利用する側にMyVersionの branch -> checkout -> fix -> add -> commit -> push
を意識させずに使ってもらうFacadeを作ります。

### 課題
Gitの機能を使って、リモートリポジトリに変更のリクエストを送る機能を実装する。

使う側にGitのbranch -> checkout -> fix -> add -> commit -> push
を意識させずに使ってもらうFacadeを作ります。

### (参考)イメージ
Google検索はまさにFacadeです。
検索というのは裏で非常に複雑な処理をしています。
- 事前準備
  - Webページを閲覧して回ってWebページリストを作っておく
  - 雑多なリストを意味のある順番に並べ替えておく
  - インデックスを貼る(例：辞書のように、「あ」→「あ」から始まる言葉にすぐアクセスできるようにする)
  - Webページリスト同士の関係を調べて繋がりをつくておく
  - etc...
- 検索
  - 検索文字を意味のある単位に分割する
  - 検索文字と関連する言葉を用意する
  - 文字列をもとにインデックスを探す
  - インデックスから該当ページを探し、一覧にする
  - 一覧を事前に決まったアルゴリズムで並べる

上記のような複雑な手順が必要な処理を、使う側は「検索ワードを入力する」だけで実現できます。  
これはGoogleがこの窓口ページから内部で何をやっているのかを意識させずに「検索する」という機能のみを公開しているためです。

### 応用問題(未完成)
Gitを使ったファイル編集アプリ「GEdit」を保守していましたが、オリジナルバージョン管理ツールを使ってファイルを管理をしているシステム「SVNet」と統合するような要件が発生しました。
ただし、このシステムのソースコードは失われ、JavaDocとjarのみが残されておりAPIはかなり複雑です。

AdopterパターンとFacadeパターンを使って利用者側に大きな変更を加えることなく管理対象を増やしてください。