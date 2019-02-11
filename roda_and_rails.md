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
* Railsをお使い(であろう)皆さんへのRodaのすゝめ
* 何でRoda?

---

# About Roda

---

# Roda

* "Routing Tree Web Framework Toolkit"
* Webアプリを作る為のフレームワーク
  * MVCにおけるView / Controller部分のみを提供
  * routing / controllerを定義するDSL的なもの
* Sinatraの仲間
  * Sinatraと違い階層構造(木構造)でroutingを定義出来る

---

# Roda

```ruby {style="font-size: 16p"}
route do |r|
  r.is 'artist/:id' do |artist_id|
    r.get do
      view :artist
    end

    r.post do
      @artist.update(r['artist'])
      r.redirect
    end
  end
end
```
---

# Roda

* 作者は Jeremy Evans(@jeremyevans) https://github.com/jeremyevans
* Sequel(DBアクセス用ライブラリ) author
* OpenBSD Ruby ports maintainer
* 他にも色々
* bugs.ruby-lang.org にも割と良く出現している

---

# 何故Rodaか

---

# Rails is slow(?)

---

# Rails is slow?

* ここからWeb Framework Benchmarks(https://www.techempower.com/benchmarks/)のはなし

---

# Roda is fast?

* Roda is à la carte
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
* 等々

---

# Rails Router

* Rails Routerは多機能
* 複雑なウェブアプリケーションを作成するためにはこれらの機能は必要だった(多分)

---

# 今はどうだろうね

* 元々の役割は変わらずある
  * ただ、それだけではないよね
* SPAアプリの場合、サーバ側はAPIだけで良い事もある
  * その場合、そこまで複雑なRoutingが必要ではない

---

# GraphQL

* POSTのエンドポイント一個あれば良い

---

# GraphQL

GraphQL Rubyをつかう場合

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

# そこでRoda

---

# RouterにRodaを使ってみよう

---

# Roda on Rails

---

# Roda on Rails

* mountメソッドについての紹介

---

# Roda on Rails

* controllerの場合の実装

---

# Roda on Rails

* Rodaでの実装

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

# 性能検証

* ここまでのまとめ
* Pros / Cons書く

---

# endpoint

---

# Rack Middlewares

* Rails.applicationはRack application

---

# Rack Middlewares(API-only)

```
use ActionDispatch::HostAuthorization
use Rack::Sendfile
use ActionDispatch::Static
use ActionDispatch::Executor
use ActiveSupport::Cache::Strategy::LocalCache::Middleware
use Rack::Runtime
use ActionDispatch::RequestId
use ActionDispatch::RemoteIp
use Rails::Rack::Logger
use ActionDispatch::ShowExceptions
use ActionDispatch::DebugExceptions
use ActionDispatch::Reloader
use ActionDispatch::Callbacks
use ActiveRecord::Migration::CheckPending
use Rack::Head
use Rack::ConditionalGet
use Rack::ETag
```

---

# config.ru

---

# Roda on Rails


---

# Rails way?

* これは「Rails way」から外れている?
* そもそも何をやったらRails wayから外れる事になるんでしょうね
* 色々定義はあると思いますが、私の中では「RailsのPublic APIのみを使っているかどうか」がポイント
* 今回の話だと`mount`も`endpoint`もPublic APIなので、問題無い(と私は思っている)

---

# Conclusion

* Rails is omakase
