---

# Rails コントリビューション

## @y-yagi

---

# 自己紹介

* @y-yagi
* Ginza.rbの主催の一人

---

# 今日お話すること {.big}

過去のRailsへのコントリビュートの経験を元に、RailsにPRを送る際にどのような事に気をつければ良いか、どうすればPRを送れるようになるかをご紹介出来ればと思います。

---

# 自己紹介

* Rails Contributors(http://contributors.rubyonrails.org)では24位(598コミット)
* 最初のコミットが2014-07-15なのでコントリビュートしはじめて3年4ヶ月

---

# Rails Contributors

* Rails開発のcore memberの一人であるXavier Noria(@fxn)が作成したサイト
* OSS( https://github.com/fxn/rails-contributors )
* マージコミットやバックポートのコミットもカウントされる
* また、コミットメッセージに特定のフォーマットで名前を記載した場合、その記載されている名前全てコントリビューターとしてカウントされる
* なので、GitHub上でのコミット数とは大分事なる

---

# 何故コントリビュートするようになったのか

---

# コミットログを読んでいる

---

# コミットログ

* Railsのコミットログを読んでブログにメモを記載するという事をやっている
* 2014-04-07から

---

# コミットログ

* 当時、なんとなくRailsを使ってコードは書けるが、ちゃんとした書き方がわからないなあと思うことが多かった
* ちゃんとRailsについて勉強したいなと思い、色々考えた結果コミットログを読み始めた
   * あと当時暇だった

---

# 読み始めた結果どうだったか

---

# コミットログ

* わからない事だらけ
* コンポーネントの構成がどうなっているとか、クラスの継承関係等がさっぱりわからなかったので
* 何でこれで動くの…? という事が多かった

---

# コミットログ

* わからなかったら動かして確認した
  * Railsは動かすのが簡単
* 割とテストがちゃんと書かれるようになっていた頃だったのでテストをよく見ればなんとなく意図がわかった

---

# コミットログ

* 2,3ヶ月もするとなんとなくRailsのコードに慣れた
* 慣れると色々とミスに気付くようになる
  * docに記載されいてる説明と実際の挙動が違う、そもそも動かない、等々
* 問題がある状態をそのままにしておくのは良くない
* この頃からコントリビュートをはじめれるようになって今に至る

---

# 本題

---

# Rails コントリビューション

---

# どういう時にコントリビュートするのか

* Railsが期待通りに動かないとき
* Railsに機能追加したいとき

---

# 期待通りに動かない

* まあまあある
  * バグなのか仕様なのか難しい問題もある
* Railsは良く使う道具なので期待通り動いて欲しい
  * よく使う道具が期待通りに動かないとストレスになる
* SNSで文句を言ってても直らない

---

# 期待通りに動かない

* Issueをつくる or PRをつくる
  * Issueをつくるのも大事なコントリビュート
* 英語が苦手なので、私はPRを投げてしまう事の方が多い

---

# 機能追加したいとき

* 新しく追加された機能は大体機能が足りない
* Railsは良く使う道具なので機能が足りてて欲しい

---

# Rails コントリビューション

* 具体的なアプローチについて
* なお、ここでの説明は https://github.com/rails/rails のみを対象として話します
  * github.com/rails 配下の他のリポジトリだと方針が多少違う事もある

---

# Rails コントリビューション

* コントリビュートの方法について記載したRails guide(http://edgeguides.rubyonrails.org/contributing_to_ruby_on_rails.html) もあるので、こちらも合わせて参照してください
  * ちょっと内容古い部分もあるが……

---

# Issue

---

# Issue

* rails/railsのIssueはバグの管理にのみ使われいる
* 質問や新機能の提案などには使われておらず、それらのissueを作成しても、無視、又は即closeされる
  * https://github.com/rails/rails/issues/30762#issuecomment-333337344 のようになる

---

# Issue

* 新機能についての議論をしたい場合は rails-core mailing list(https://groups.google.com/forum/?fromgroups#!forum/rubyonrails-core)へ
* PR投げちゃってPR上で議論をするでも大丈夫
  * どちらかというとこっちの方が多い印象

---

# Issue

* バグ報告用にIssueテンプレートが用意されているので出来ればそのフォーマットに従う
  * https://github.com/rails/rails/blob/master/.github/issue_template.md
* バグの報告をする際は(可能な限り)再現手順を記載する

---

# Issue

* 再現手順作成用のテンプレートファイルが用意されているので使えるならそれらを使用する
  * https://github.com/rails/rails/tree/master/guides/bug_report_templates
  * 上記テンプレートを使用すると、ファイル単体でActie RecordやActive Jobを使用したスクリプトが準備出来る
* 上記を使って手順を再現するのが難しい場合、GitHub上に再現手順をまとめたRailsアプリをアップロードしてくれたりすると大変助かる
  * 例：https://github.com/saneef/reproduce-couldnt-find-template-for-digesting

---

# Issue

* masterブランチでも問題が再現するか確認する
* サポート対象外の古いRailsでのIssueを報告しても即close or 無視されるので、古いRailsを使っている場合はまず頑張ってRailsのバージョンを上げる
  * メンテナンスポリシーについては http://guides.rubyonrails.org/maintenance_policy.html 参照

---

# Pull request

---

# Pull requsetを出すその前に

* 似たようなPRがもう無いかGitHubで検索してみる
  * OpenされているPRだけではなくClosedになっているPRも検索する
* 幸いRailsは早い時期にGitHubに移行したため、GitHubを検索すれば大体事足りる

---

# Pull requsetを出すその前に

* 同じようなPRがあってそれがMergeされる事無くCloseされているなら何故Closeされてしまっているかを確認する
* 理由を確認した上でそれでもPRを投げたいならPRのdescriptionにそのあたりの理由もちゃんと記載する

---

# Pull requsetを出すその前に

* 同じようなPRがやりとりが止まってしまっている事もある
* そのPRの作者にpingしてまだやる気ある? というのを確認してみてみる
  * やる気なさそうなら引き継ぎでやってしまう
  * 一言断り入れてからやるのが紳士的

---

# Pull requsetを出すその前に

* その機能本当にRails本体にいる? かを考える
* gemじゃ駄目なのか考えてみる
  * リリースとかテストとかgemの方が楽な事も多い
* foreignerやmigration\_commentsのように、gemから機能を本体にインポート、みたいなケースもある

---

# Pull requset

実際にPRを出す際に気をつけていること

---

# Pull request

* こちらもフォーマットが用意されているので出来ればそのフォーマットに従う
  * https://github.com/rails/rails/blob/master/.github/pull_request_template.md
* descriptionちゃんと書くの大事
* 既にIssueがあるBugの修正の場合コミットログにそのIssueの番号を入れる
  * 例: https://github.com/rails/rails/commit/52422f2af60c0830da6e5749700f911c6c0b22ea

---

# Pull request

* コードの修正や機能追加の場合、テストの実行・追加を忘れずに
* テストの実行方法はちょっとややこしいので割愛
* 大体は`bundle exec rake test`で動く
* 予想外の所が壊れる事があるので、CIの結果も確認する
  * CIちょっと不安定なので全然関係無いところがエラーになってしまう事もある……

---

# Pull request

* docやコード内のコメントの修正の場合CIが実行されないようコミットログに"\[ci skip\]"をいれる
  * 例: https://github.com/rails/rails/commit/88dc74b78468546748fdfdc4133e153efcc1f1c9
* "\[skip ci\]" でも大丈夫
  * https://docs.travis-ci.com/user/customizing-the-build/#Skipping-a-build
* RailsのCIは割と時間がかかるので、不要な場合はCIを実行しないようにする必要がある

---

# Pull request

* 性能改善のPRの場合、 性能チェックに使ったスクリプト及び結果をそのままコミットログに入れてしまう
  * 例：https://github.com/rails/rails/commit/338127869a4b62ddda5c75647ac1fb928361db70
* コミットログに入れておくと、後からコミットを見た人が直ぐ結果の再取得が出来てちょうべんり
* 性能チェック用のスクリプトファイルのフォーマットも提供されている
  * https://github.com/rails/rails/blob/master/guides/bug_report_templates/benchmark.rb

---

# Pull request

* コミットログに説明が必要な事は全て入れてしまう
  * PRのdescriptionに記載が必要な内容はコミットログに全部入れるようにしている
* 後から何か確認したい時にコミットログを見れば済むので大変楽
  例：https://github.com/rails/rails/commit/ffc4710c2bff273b82ddb76675701f986d82ef4f

---

# Pull request

* その対応が何故必要なのか、何故妥当なのかの説明もコミットログに入れている
* 証拠や参考になるコミットやPRがあればそれへのリンクも入れる
  * 例:  https://github.com/rails/rails/commit/0e47e58a96c2cd6d7f655d0ccf35737ac5fbf80c

---

# Pull request

* レビューする側の負担を減らせるよう必要そうな情報をすべて渡する
  * レビューする側がRailsすべての機能に対して詳しいとは限らない　
  * そのPRを作ったあなたの方が詳しい、という事も普通にある

---

# Pull request

* public APIの挙動を変えない
  * Railsにおけるpublic APIとは API doc(http://api.rubyonrails.org/)にのっているAPI
* public APIの挙動を変えたい場合はまずDeprecateから
  * https://github.com/rails/rails/commit/58f10a31b37e9bb6e975a71aa63744f318ee043d
  * https://github.com/rails/rails/commit/fbcc4bfe9a211e219da5d0bb01d894fcdaef0a0e

---

# Pull request

* 適切な単位にcommitをsquashする
  * rails/railsではPRは一つのcommitになっている事が望まれる
  * Reviewの指摘対応のcommitもsquashして一つにする

---

# コントリビュートをしてみたい、と思っているが何からはじめたら良いかわからない方に

---

# コントリビューションのはじめかた

* 公式のdocを見る
* 新しいバージョンをさわる

---

# doc

* Railsの公式のドキュメントは二つ
* http://api.rubyonrails.org/ と http://guides.rubyonrails.org/
* 上記二つはリリース済み(gemとして公開されている)のRailsに対応
* edge(GitHubのmasterブランチ)版は http://edgeapi.rubyonrails.org/ と http://edgeguides.rubyonrails.org/

---

# doc

* 普段から公式のドキュメントを見る癖をつける
* ドキュメントは今は全てrails/railsリポジトリで管理されているので、内容が間違えてる・フォーマットが崩れている等があればPRチャンス
  * 割とよくある

---

# edgeをさわる

* 最新バージョンをちゃんと触る
  * rcをまたずbeta1が出たら直ぐ触る
* 新しい機能はだいたいバグがある
* 既存のコードも割と壊れる事がある

---

# Rubyの新しいバージョンでさわる

* Rubyの機能追加・変更に伴いRails側で対応が必要な事がある
  * Ruby 2.5の場合`Hash#transform_keys`が追加されたことによる対応等
* テストが壊れる事もある

---

# コントリビューションの壁?

* 英語がつらい?
* 私もつらい
* コミットログを参考にする

---

# コミットログを参考にする

* コミットログを適切そうな単語で検索してみる
  * rails/railsには大量のコミットログがある
  * rails/railsには大量のissue / PRもある
* 同じ事をしているコミットやPRを見つけて参考にする
* git log + pecoでlogを検索出来るようにしてる

---

# コントリビューションの壁?

* 何か怖い
  * たまにきく
* 普段やらない事や慣れていない事を不安に感じるのはしょうがない

---

# コントリビューションの壁

* これについては慣れるしか無い　
  * 誰も怒らないので、とりあえず気になる事があったらやってみると良いと思います
* 普段OSSにコントリビュートしている知り合いに協力してもらう等が出来ると良さそう
* そういう知り合いがいない場合、OSS Gate(https://oss-gate.github.io) に参加してみるのも良いと思います
* Rubyのコミュニティに行って相談してみる、とかも良いと思います

---

# まとめ

* 私が普段コントリビュートしている事について説明しました
* ただ、こういう事は慣れてきたら気にする、位で良いと思います
* Issueを報告する際はテンプレートに従う事と再現手順をつけること
* PRを投げる際はdescriptionをちゃんと書くこととテストが通ること
* 位が出来れば大丈夫

---

# Appendinx(時間があまったときよう)

Rails 5.2つまみぐい

---

# Active Storage

* ファイルアップロード処理用ライブラリ
  * [carrierwave](https://github.com/carrierwaveuploader/carrierwave) や [shrine](https://github.com/janko-m/shrine) の仲間
* クラウドサービスに簡単にファイルをアップロード、及び、Active Recordから参照ができるようになっている
* https://speakerdeck.com/willnet/file-upload-2017 に良くまとまっているので詳細は左記参照

---

# Early Hints for HTTP/2

* https://github.com/rails/rails/pull/30744
* HTTP/2のEarly Hints対応
* 今のところはPumaじゃないと動かない

---

# credentials.yml

* https://github.com/rails/rails/pull/30067
* 秘密情報を保持する為の新しい仕組み
  * アプローチはEncrypted Secretsと同じ
* `config/secrets.yml`、`config/secrets.yml.enc`、`SECRET_BASE_KEY`はお役御免

---

# recyclable cache keys

* https://github.com/rails/rails/pull/29092
* fragments cachingのcache keyのフォーマットが再利用可能なフォーマットに変わる
* 元々はcache keyにassocationのversion(timestamp)を含んでいたが、これは含まなくなる
  * cache keyとcache versionが別に管理される
  * version情報はcacheの中に入る
