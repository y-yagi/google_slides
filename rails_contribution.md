---

# Rails コントリビューション

## @y-yagi

---

# おしながき

* Railsへのコントリビュートの仕方

---

# 自己紹介

* @y-yagi
* Ginza.rbの主催の一人

---

# Rails Contributorとして

* Rails Contributors(http://contributors.rubyonrails.org)では26位(580コミット)
* 最初のコミットが2014-07-15なので、contributeしはじめて約3年3ヶ月
---

# Rails Contributors

* Rails開発のcore memberの一人であるXavier Noria(@fxn)が作成したサイト
* OSS( https://github.com/fxn/rails-contributors )
* マージコミットやバックポートのコミットもカウントされる
* また、コミットメッセージに特定のフォーマットで名前を記載した場合、その記載されている名前全てコミットしたとカウントされる
* なので、GitHub上でのコミット数とは大分事なる

---

# そもそも何故contributeするようになったのか

---

# コミットログを読んでいる話

---

# コミットログ

* Railsのコミットログを読んでブログにメモを記載する、という事をやっている
* 2014-04-07から現在まで毎日

---

# コミットログ

* 当時、なんとなくRailsを使ったコードは書けるが、ちゃんとした書き方がわからないなあと思うことが多かった
* ちゃんとRailsについて勉強したいなと思い、色々と考えた結果コミットログを読み始めた
   * 丁度暇だったという事もある

---

# 読み始めた結果どうだったか

---

# なるほどわからん

---

# コミットログ

* わからない事だらけ
* コンポーネントの構成がどうなっているとか、クラスの継承関係等がさっぱりわからなかったので、何でこれで動くの…? という事が多かった
* 最初の頃は、自分の中でどう進めたら良いか、というのも固まってなかったので、大分時間が掛かった

---

# コミットログ

* わからなかったら動かして確認した
* あと割とテストがちゃんと書かれるようになっていた頃だったので、テストをよく見ればなんとなく意図がわかる事もあった

---

# コミットログ

* 2,3ヶ月もすれば、なんとなく慣れた
* 慣れると、コードのミスに気付く
  * docに記載されいてる説明と実際の挙動が違う等々
* この頃からcontributeをはじめられるようになった

---

# という訳で本題

---

# Rails コントリビューション

---

# どういう時にcontributeするのか

* Railsが期待通りに動かないとき
* Railsに機能追加したいとき

---

# 期待通りに動かない

* まあまあある
  * バグなのか仕様なのか難しい問題もある
* SNSで文句を言ってても直らない
* Issueをつくる or PRをつくる
* Issueをつくるのも重要なcontribute

---

# 機能追加したいとき

* たまにある
* アプローチの仕方はケースバイケース
  * 具体的なユースケースを提示する等々

---
# 筆者の場合

*


---

# Rails コントリビューション

"contribute"には色々な方法は幾つかありますが、ここではGitHubでのissue / Pull requestについて取り扱います

---

# Issue

* rails/railsリポジトリでは、issueはバグの管理にのみ使われいる
* 質問や新機能の提案などには使われておらず、それらのissueを作成しても、無視、又は即closeされる
  * https://github.com/rails/rails/issues/30762#issuecomment-333337344 のようになる

---

# Isseue

* 新機能についての議論をしたい場合は rails-core mailing list(https://groups.google.com/forum/?fromgroups#!forum/rubyonrails-core)へ
* PR投げちゃってPR上で議論をする、でも大丈夫

---

# Issue

* Issueテンプレートは用意されているので出来ればそのフォーマットに従う
  * https://github.com/rails/rails/blob/master/.github/issue_template.md
* バグの報告をする際は、(可能な限り)再現手順を記載する

---

# Issue

* 再現手順作成用のテンプレートファイルが用意されているので、必要ならそれらを使用する
  * https://github.com/rails/rails/tree/master/guides/bug_report_templates
  * 上記テンプレートを使用すると、ファイル単体でActie RecordやActive Jobを使用出来る
* 上記を使って手順を再現するのが難しい場合、GitHub上に再現手順をまとめたRailsアプリをアップロードしてくれたりすると大変助かる
  * 例：https://github.com/saneef/reproduce-couldnt-find-template-for-digesting

---

# Pull request

---

# Pull requsetを出すその前に

* 似たようなPRがもう無いかGitHubで検索してみる
  * OpenされているPRだけではなく、ClosedになっているPRも検索する
* 同じようなPRがあって、それがMergeされる事無くCloseされているなら、何故Closeされてしまっているかちゃんと確認する
* 理由を確認した上で、それでもPRを投げたいなら、PRのdescriptionにそのあたりの理由もちゃんと記載する
* TODO: 例いれる

---

# Pull requset

色々あるので、まずは概ねみんなやった方が良いのでは、という点から

---

# Pull request

* こちらもフォーマットが用意されているので出来ればそのフォーマットに従う
  * https://github.com/rails/rails/blob/master/.github/pull_request_template.md
* 既にIssueがあるBugの修正の場合コミットログにそのIssueの番号を入れる(関連付けの為)
  * 例: https://github.com/rails/rails/commit/52422f2af60c0830da6e5749700f911c6c0b22ea

---

# Pull request

* docやコード内のコメントの修正の場合、CIが実行されないようコミットログに"\[ci skip\]"をいれる
  * 例: https://github.com/rails/rails/commit/88dc74b78468546748fdfdc4133e153efcc1f1c9
* "\[skip ci\]" でも大丈夫
  * https://docs.travis-ci.com/user/customizing-the-build/#Skipping-a-build
* RailsのCIは割と時間がかかるので、不要な場合はCIを実行しないようにする必要がある

---

# Pull request

* 性能改善のPRの場合、 性能チェックに使ったスクリプト、及び結果をそのままコミットログに入れてしまう
  * 例：https://github.com/rails/rails/commit/338127869a4b62ddda5c75647ac1fb928361db70
* コミットログに入れておくと、後からコミットを見た人が直ぐ結果の再取得が出来てちょうべんり

---

# Pull request

* 適切な単位にcommitをsquashする
  * rails/railsではPRは一つのcommitになっている事が望まれる
  * Reviewの指摘対応のcommitもsquashして一つにする

---

# Pull request

* コードを修正したらテストを実行するのを忘れずに
* 予期せぬ箇所を壊してしまう事もあるので、必ずTravis CIでの結果を確認する

---

# Pull request

ここからは個人的に気をつけていること

---

# Pull request

* コミットログに説明が必要な事は全て入れてしまう
  * PRのdescriptionに記載が必要な内容はコミットログに全部入れるようにしている
* 後から何か確認したい時にコミットログを見れば済むので大変楽

---

# Pull request

* 例 ffc4710c2bff273b82ddb76675701f986d82ef4f

---

# Pull request

see also
TODO: リンク

---

# Rails コントリビューション

* contributeの方法について記載したRails guide(http://edgeguides.rubyonrails.org/contributing_to_ruby_on_rails.html) もあるので、合わせてご参照下さい

---

# コントリビューションのはじめかた

*

---

# コントリビューションのはじめかた

* doc
* Railsの公式のドキュメントは二つ
* http://api.rubyonrails.org/ と http://guides.rubyonrails.org/

---

# コントリビューションのはじめかた

* 普段から公式のドキュメント及びソースを見る癖をつける
* ドキュメントは今は全て rails/rails で管理されているので、内容が間違えてる・フォーマットが崩れている等があればPRチャンス
  * 割とよくある

---

# コントリビューションのはじめかた

* 最新バージョンをちゃんと触る
* 新しい機能はだいたいバグがある

---

# コントリビューションの壁
---

* 英語がつらい?
* 私もつらい
* コミットログを参考にする

---

# コミットログを参考にする

* コミットログを適切そうな単語で検索してみる
  * rails/railsには大量のコミットログがある
  * rails/railsには大量のissue / PRもある
* git log + pecoでlogを検索出来るようにしてる

---

# コントリビューションの壁

* 何か怖い
  * たまにきく
* 普段やらない事や慣れていない事を不安に感じるのはしょうがない

---

# コントリビューションの壁

* これについては慣れるしか無い　
* 普段OSSにコントリビュートしている知り合いに協力してもらう等が出来ると良さそう
* そういう知り合いがいない場合、OSS Gate(https://oss-gate.github.io) に参加してみるのも良いと思います

---

# まとめ

---


# Appendinx(時間があまったときよう)

Rails 5.2つまみぐい

---

# Active Storage

* ファイルアップロード処理用ライブラリ
  * [carrierwave](https://github.com/carrierwaveuploader/carrierwave)
  * [shrine](https://github.com/janko-m/shrine)
* クラウドサービスに簡単にファイルをアップロード、及び、Active Recordから参照ができるようになっている。
* 詳細は TODO: リンク　をみてね

---

# Early Hints for HTTP/2

---

# credentials.yml

---

# recyclable cache keys
