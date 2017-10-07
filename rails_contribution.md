---

# Rails contribution

## @y-yagi

---

# おしながき

* Railsへのcontributeの仕方

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

# Rails contribution

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
* Issueをつくるのも重要ななcontribute

---

# 機能追加したいとき

* たまにある
* アプローチの仕方はケースバイケース
  * 具体的なユースケースを提示する等々

---

# Rails contribution

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
* RailsのCIは割と時間がかかるので、不要な場合はCIを実行しないようにするのが大変好まれる

---

# Pull request

* 性能改善のPRの場合、 性能チェックに使ったスクリプト、及び結果をそのままコミットログに入れてしまう
  * 例：https://github.com/rails/rails/commit/338127869a4b62ddda5c75647ac1fb928361db70
* こうしておくと後からこのコミットを見た人が直ぐ結果の再取得が出来てちょうべんり

---

# Rails contribution

* contributeの方法について記載したRails guide(http://edgeguides.rubyonrails.org/contributing_to_ruby_on_rails.html) もあるので、合わせてご参照下さい
---
