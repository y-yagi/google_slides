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
1. Run tes again

---

# Upgrading Flow

1. **Bundle update Rails**
1. Run test
1. Update config
1. Run tes again

---

# Bundle update Rails

* 依存しているgemでrailsのバージョンロックが入っている事が多く、まずここで時間がかかる
* 片っ端から更新のPRを送る
  * ...の前に、そもそもそのgem本当に必要か再度考える

---

# Bundle update Rails

* そのgem本当に必要ですか?
* インストールした際は必要だったとしても、その後要件が変わったりして実は不要になったgemが残ってるかもしれない
* 便利かと思っていれてみたが、実はそんなに便利ではなかった(要件が合わなかった)gemが残ってるかもしれない
* Rails本体で同じ機能が提供された事により、不要になった可能性もある(e.g. `quiet_assets`)

---

# Let's clean the gems

---

# Bundle update Rails

* Railsのアップグレードの際にgemの掃除も行う
* 見直しをした結果、本当に必要なgemにだけ更新のPRを送る
  * それでも多いかもしれないが、そこは頑張る
* 変なgemを使ってなければ、既に誰かがPRを送っている可能性が高い

---
