---

# Thinking about Rails upgrading

## @y-yagi

---

# About

* Rails upgradingを行う際にやっていること・考えている事をまとめた資料です
* あくまで個人の感想です

---

# Upgrading Flow

1. Bundle update Rails
1. Run test
1. Update config
1. Run test again
1. Fix deprecate message
1. Finally run the test again

---

# Upgrading Flow

1. **Bundle update Rails**
1. Run test
1. Update config
1. Run test again
1. Fix deprecate message
1. Finally run the test again

---

# Bundle update Rails

* 依存しているgemでrailsのバージョンロックが入っている事が多く、まずここで時間がかかる
* 片っ端から更新のPRを送る
  * の前に、そもそもそのgem本当に必要か再度考える

---

# Bundle update Rails

* そのgem本当に必要ですか?
* インストールした際は必要だったとしても、その後要件が変わったりして実は不要になったgemが残ってるかもしれない
* 便利かと思っていれてみたが、実はそんなに便利ではなかった(要件が合わなかった)gemが残ってるかもしれない
* Rails本体で同じ機能が提供された事により、不要になった可能性もある(e.g. `quiet_assets`)

---

# Let's clean the Gemfile

---

# Bundle update Rails

* Railsのアップグレードの際にgemの掃除も行う
* Railsのアップグレードで辛い事の多くはgemの更新(個人の感想です)なので、今後の事も考えて負担を減らしていく
* 見直しをした結果、本当に必要なgemにだけ更新のPRを送る
  * それでも多いかもしれないが、そこは頑張る

---

# Bundle update Rails

* よく使われているgemであれば、既に誰かがPRを送っている可能性が高い
* 逆にいうと、Railsが正式リリースされても対応のPRが無い・PRがあっても放置されているようなgemは、メンテされてない・使われていない可能性が高いので、今後の利用を考えた方が良い

---

# Upgrading Flow

1. Bundle update Rails
1. **Run test**
1. Update config
1. Run test again
1. Fix deprecate message
1. Finally run the test again

---

# Run test

* 最初のテスト実行
* ここで落ちる場合、Railsのバグを踏んだ、private APIを使用していてそれが壊れた、gemが壊れた、等々色々な要因がありえる
* ここは頑張って一つずつ対応していくしか無い
* そもそもテストが無いプロジェクトは、色々頑張っていきましょう

---

# public API / private API

* Railsはpublic API(ユーザが使用して良いAPI)とprivate API(ユーザが使用しては**いけない**API)が明確にわかれている
* public APIは[Rails API doc](http://api.rubyonrails.org/) にのっているAPI、それ以外は基本private API
* 例えばActive Recordでは`where`はpublic APIだが、`where!`はprivate API
  * なので`where!`は使っては駄目ですよ
  * Arelも同様

---

# public API / private API

* public APIは一つのバージョンアップ(major, minor問わず)で挙動が変わる事は無い
  * public APIについては、必ず既存の挙動をdeprecationにしてから変えるようにしてる
  * 一部例外あり。どうしても挙動が変わってしまうAPIについては、[A Guide for Upgrading Ruby on Rails](http://guides.rubyonrails.org/upgrading_ruby_on_rails.html) に載る(はず)
* public APIの挙動が変わっていたらそれはバグなので、issue報告して下さい

---

# public API / private API

* private APIを多用している場合のアップグレードは大変
  * そもそもメソッドが無くなっている場合もあったりで、気付くのが大変
* Railsの機能を拡張するgemでprivate APIを使うのは仕方無いが、普通のRailsアプリではprivate APIを使用しなくてもどうにかなるはず

---

# public API / private API

* どうしてもprivate APIが必要な場合は、その部分だけ切り出してgemにする、他のファイルと混ぜない(モンキーパッチである事が明確にわかる管理にする)ようにきをつける
* または、private APIをpublic APIにするようRails側にPRを送る
  * 一旦public APIに出来ればこっちのもん
* 最大限private APIがアプリ内で使われるのを避けるようにする

---

# Upgrading Flow

1. Bundle update Rails
1. Run test
1. **Update config**
1. Run test again
1. Fix deprecate message
1. Finally run the test again

---

# Update config

* テストが通ったら、新しいconfigを適用していく
* 新しいconfigについては [RailsDiff](http://railsdiff.org/) を見る、手元で`rails new`してアプリを作ってみて既存のアプリとのdiffをとる、等をして確認すると良いと思います

---

# Update config

* `app:update`は使ってない
* 色々と問題がある(`rails new`で指定したオプションを考慮しない、configのappendが出来ない等)
  * 直したい気持ちはある(https://github.com/rails/rails/pull/29645)ので、多少は良くなっているかも

---

# Upgrading Flow

1. Bundle update Rails
1. Run test
1. Update config
1. **Run test again**
1. Fix deprecate message
1. Finally run the test again

---

# Run test again

* 再度テスト実行
* ここエラーになるケースはそんなに多くない印象

---

# Upgrading Flow

1. Bundle update Rails
1. Run test
1. Update config
1. Run test again
1. **Fix deprecate message**
1. Finally run the test again

---

# Fix deprecate message

* deprecateメッセージを片っ端から潰していく
* deprecateメッセージに代わりに使うべきメソッド・configが含まれている筈なので、基本的にはそれに従う
  * "without replacement"なケースもあるが、それで本当に困る事は滅多に無い筈

---

# Fix deprecate message

* Railsのバージョンがrcの場合は少し様子を見る事もある
  * rcリリース後にdeprecateだったのがdeprecateじゃなくなるケースもちょいちょいある
  * e.g. [Remove implicit coercion deprecation of durations](https://github.com/rails/rails/commit/a91ea1d51048342d13fc73f9b09ce4cfd086bb34)
  * DHHぱわー(真っ当意見が多い)

---

# Upgrading Flow

1. Bundle update Rails
1. Run test
1. Update config
1. Run test again
1. Fix deprecate message
1. **Finally run the test again**

---

# Finally run the test again

* 最後のテスト実行
* ここエラーになるケースはそんなに多くない筈

---

# Finish

後は震えながら本番にリリースするだけ


---

# Conclusion

* Rails upgradingを行う際にやっていること・考えている事についてお話しました
* 最近のRailsは、一つのバージョンアップで挙動が変わらないよう気をつけています
* しかしそれでも、(予想外に)壊れる事は往々にしてあります
* 早めにアップグレードを試してみて、みんなでバグ踏んでいこうな

---
