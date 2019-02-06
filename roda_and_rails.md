---

# Roda and Rails

## y-yagi@Railsdm

---

# 自己紹介

* @y-yagi
* Ginza.rbの主催の一人

---

# 今日お話すること

* Rodaというフレームワークについての紹介
* RailsでRodaを使う

---

# Roda

* Rodaの紹介
* 多分3枚くらい

---

# 何故Rodaをすすめるか

---

# Rails is slow?

* そやな

---

# Rails is slow?

* ここからWeb Framework Benchmarks(@<href>{https://www.techempower.com/benchmarks/})のはなし

---

# Roda is fast?

* Roda  is à la carte
  * 自分で使う機能を選ぶ必要がある

---

# Roda is fast?

* Rails is omakase
  * 全部入り

---

# Roda on Rails

* Railsはおまかせだけど、メニューは変更出来る
* ならば、RailsのroutingにRodaを使ってみる、という事も出来る

---

# Rails Router{.big}

RailsのRouterが提供している機能について考えてみる

---

# Rails Router

* URLとcontrollerのactionとのマッピング
* pathとURLのhelper
* scope(namespace)

---

# Rails Routerは多機能

* 複雑なWebアプリケーションを作成するためには、これらの機能は必要だった(多分)

---

# 今Webアプリケーションに求められる役割

* 元々の役割は変わらずある
  * ただ、それだけではないよね
* SPAアプリの場合、サーバ側はAPIだけで良い事もある
  * その場合、そこまで複雑なRoutingが必要ではない

---

# GraphQL

* POSTのエンドポイント一個あれば良い

---

# GraphQL

```ruby
Rails.application.routes.draw do
  post "/graphql", to: "graphql#execute"
end
```
---

# GraphQL

* URLとcontrollerのaction(というか、何らかの処理)と紐付けだけ出来れば良い
* 多機能なRouterは不要なはず

---

# RouterにRodaを使ってみよう

---

# Roda on Rails

* controllerの場合の実装

---

# Roda on Rails

* rodaでの実装

---

# Roda on Rails

* mountメソッドについての紹介

---

# Roda on Rails

* 性能検証

---

# 性能検証

* benchmark-ipsのスクリプトはる

---

# 性能検証

* benchmark-ipsの結果はる

---

# 性能検証

* Apache Benchのスクリプトはる

---

# 性能検証

* Apache Benchの実行結果はる

---

# endpointについての話

---

# Railsを使わず

---

# Conclusion

