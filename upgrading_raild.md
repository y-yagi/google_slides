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
* 逆にいうと、Railsが正式リリースされても対応のPRが無い・PRがあっても放置されているようなgemは、メンテされてない・使われていない可能性が高いので、今後の利用を少し考えた方が良い

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
  * public APIについては、必ず一度deprecationにしてから挙動を変えるようにしてる
  * 一部例外あり。どうしても挙動が変わってしまうAPIについては、[A Guide for Upgrading Ruby on Rails](http://guides.rubyonrails.org/upgrading_ruby_on_rails.html) に載る(はず)
* 予期せず挙動が変わっていたらそれはバグなので、issue報告すればOK

---

# public API / private API

* private APIを多用している場合のアップグレードは大変
  * そもそもメソッドが無くなっている場合もある
* Railsの機能を拡張するgemでprivate APIを使うのは仕方が無いが、普通のRailsアプリではprivate APIを使用しなくてもどうにかなるはず

---

# TODO

* rails app:update の駄目なところをかく
  * 代わりにどうしているかかく
* deprecateメッセージの対応方法書く
* 新しい設定はすぐにはonにしない話を書く

---
