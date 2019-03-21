---

# Roda and Rails

## y-yagi@Railsdm 2019

---

# 自己紹介

* @y-yagi
* Ginza.rbの主催の一人

---

# 今日お話すること

* Rodaというフレームワークについての紹介
* Railsをお使い(であろう)皆さんへのRodaの使いドコロ

---

# About Roda

---

# Roda

* "Routing Tree Web Framework Toolkit"
* Webアプリを作る為のフレームワーク
  * MVCにおけるView / Controller部分のみを提供
  * routing / controllerを定義するDSL的なもの
* RailsではなくSinatraの仲間
  * Sinatraと違い階層構造(木構造)でroutingを定義出来る

---

# Roda

```ruby {style="font-size: 12p"}
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
* Rails的にはErubi(ERB Handler) author

---

# Jeremy Evans

![](http://drive.google.com/uc?export=view&id=1ZW030sbr-LC1riwFSqAVM3ejjs1b-IAw)

[https://rubykaigi.org/2019/speakers](https://rubykaigi.org/2019/speakers)

---

# Jeremy Evans

![](http://drive.google.com/uc?export=view&id=1NYtjgWwgdPNawDhL0xBlZxEyDHZkFTQa)

[https://rubykaigi.org/2019/presentations/jeremyevans0.html#apr20](https://rubykaigi.org/2019/presentations/jeremyevans0.html#apr20)

---

# 何故Rodaか

---

# はやい

---

# Roda

![](http://drive.google.com/uc?export=view&id=13KekYhxZ82d9S1bQNVVx7Lgy7RiqAP__)

[http://roda.jeremyevans.net/](http://roda.jeremyevans.net/)

---

# Roda is fast

* ベンチマークは[luislavena/bench-micro](https://github.com/luislavena/bench-micro)の結果
  * "Hello World!"を返すだけのアプリ
* これだけみるとRails遅い
* 他のベンチマークもみたい

---

# Web Framework Benchmarks

* TechEmpower社がおこなっているWeb Framework Benchmarks([https://www.techempower.com/benchmarks/](https://www.techempower.com/benchmarks/)) というベンチマークがある
* ここはRubyに限らず様々なプログラミング言語のWebアプリケーションのフレームワークのパフォーマンス比較を行っている
  * ベンチマークは定期的に取得するようになっており最新(Round 17 / 2018-10-30)では、28のプログラミング言語 / 179のフレームワークを対象に実施

---

# Web Framework Benchmarks

* ベンチマークは複数のパターン(JSONを返す、データベースに対して1つSQLを実行する、等々)で実施されている
  * データベース(MySQL、PostgreSQL、MongoDB、等々)やアプリケーションサーバも複数パターン(Puma、unicorn、等々)で行っている
* Full-stack frameworksだけじゃなくMicro frameworksも対象になっている
  * そもそも素のRackもFrameworkに含まれている
  * その辺りも考慮して見てください

---

# Web Framework Benchmarks

* ベンチマークを取るのに使用したソースも公開されているので、興味がある方は合わせてどうぞ
  * [https://github.com/TechEmpower/FrameworkBenchmarks](https://github.com/TechEmpower/FrameworkBenchmarks)

---

# Web Framework Benchmarks

* 例えばJSON serializationの結果を見てみるとRoda(+ Sequel)は上位
  * [https://www.techempower.com/benchmarks/#section=data-r17&hw=cl&test=json&l=zijxtr-1](https://www.techempower.com/benchmarks/#section=data-r17&hw=cl&test=json&l=zijxtr-1)
* DBアクセスが絡む処理だとRubyではTOP

---

# Roda is fast?

* そもそも提供している機能が違うので単純に比較するべきではない
  * もちろん提供している機能が少ないと速くなる、というわけではない
  * DjangoとflaskだとDjangoの方が上にいたりする

---

# Roda is fast?

* Roda is à la carte
* Rodaのcoreは本当に最低限の機能しか提供していない
  * デフォルトだとテンプレートのレンダリングも出来ない
* 代わりに各種機能をpluginとして提供しており、使用する側でpluginを選択する必要がある
  * 90以上のpluginが提供されている(Roda 3.18.0時点)

---

# Plugin system

```ruby {style="font-size: 16p"}
class MyTodo < Roda
  plugin :csrf
  plugin :render, escape: true

  # ...
end
```
---

# Roda is fast?

* デフォルトで全部入りのライブラリ(Rails)とデフォルト最小構成のライブラリ(Roda)を比較すればそれは勿論最小構成の方が速い
* 勿論それ以外にもRodaには速度向上のための対応が色々と行われている
  * この辺りの話はRubyKaigiで聞けそうな気がするんでみんな楽しみにしよう

---

# Roda or Rails

* Rodaは速そう
* が、あくまでルーティングライブラリなので、Rodaを使ってWebアプリケーションを作るとRailsと比べると当然大変
  * Railsのコマンドやrakeタスク、autoloadやreloadの仕組み、設定ファイル等々の処理を自分で準備する必要がある
  * ディレクトリ構成や、テストについても考える必要がある
* Railsの便利な部分と、Rodaの速い部分を上手いこと組み合わせられないか?

---

# Rodaは"Routing Tree Web Framework Toolkit"なのでRouterについて考えてみる

---

# Rails Router

* URLとcontrollerのactionとのマッピング
* pathとURLのhelper
* scope(namespace)、constraints
* journey
* etc

---

# Rails Router

* RailsのRouterは多機能
* 複雑なウェブアプリケーションを作成するためにはこれらの機能は必要だった(多分)
  * 因みにRodaの標準のpluginでは、完全に同じ機能を提供する事は出来ない

---

# Rails Router

* しかし今はどうか
* 元々の役割は変わらずあるが、それだけではなくなった
* SPAアプリの場合サーバ側はAPIだけ
  * その場合そこまで複雑なRoutingが必要ではない
  * helperメソッドも無くても良い
* 例えばGraphQLの場合、POSTのエンドポイントが1つあれば良い

---

# GraphQL

graphql gemをつかう場合

```ruby
Rails.application.routes.draw do
  post "/graphql", to: "graphql#execute"
end
```

---

# GraphQL

* URLとcontrollerのaction(というか、何らかの処理)と紐付けだけ出来れば良い
* 多機能なRouterは不要そう
* それならRodaを使えないか?

---

# Roda on Rails

---

# Roda on Rails

* Rodaは当然Rackベース
* Rackベースのアプリケーションは`mount`メソッドを使えばRailsのroutingで使える
  * [https://edgeapi.rubyonrails.org/classes/ActionDispatch/Routing/Mapper/Base.html#method-i-mount](https://edgeapi.rubyonrails.org/classes/ActionDispatch/Routing/Mapper/Base.html#method-i-mount)

---

# GraphQL endpoint(Rails)

```ruby {style="font-size: 10p"}
class GraphqlController < ApplicationController
  def execute
    variables = ensure_hash(params[:variables])
    query = params[:query]
    operation_name = params[:operationName]
    context = { current_user: current_user }
    result = BeholderSchema.execute(query, variables: variables, context: context, operation_name: operation_name)
    render json: result
  rescue => e
    # ...
  end
end
```

---

# GraphQL endpoint(Roda)

```ruby {style="font-size: 10p"}
class RodaRoutes < Roda
  plugin :json
  plugin :json_parser
  route do |r|
    r.post "graphql" do
      variables = ensure_hash(request.params["variables"])
      query = request.params["query"]
      operation_name = request.params["operationName"]
      context = { current_user: current_user }
      BeholderSchema.execute(query, variables: variables, context: context, operation_name: operation_name).to_h
    rescue => e
      # ...
    end
  end
end
```

---

# Roda Application

```ruby {style="font-size: 14p"}
# routes.rb
Rails.application.routes.draw do
  post "/graphql", to: "graphql#execute"
  mount RodaRoutes.freeze.app => "/roda"
end
```

---

# 性能検証

* 動く状態になったので性能検証してみよう
* Rails、Roda、それぞれのGraphQLの用のURLに対してqueryをPOST
  * 先に提示した以外のコードは完全に一緒
* ベンチマークツールは wrk(https://github.com/wg/wrk)を使用
* 10 threads / 100 connectionsを30s

---

# 性能検証

* Rails: 6.0.0.beta3, Roda: 3.18.0
* CPU: Intel® Core™ M-5Y71 Processor (4M Cache, up to 2.90 GHz) 、メモリ: 8Gのマシンで検証

---

# 性能検証

||Requests/sec|Latency(Avg)|
------------ | -------------| -------------
Rails | 190.89|26.20ms|
mount| 229.78|21.76ms|

---

# 性能検証

RailsにそのままRodaのっけただけでも多少速くなる

---

# 他のパターンも試してみよう

先のパターンではRailsのrouterはそのまま使用したがそこを変えたらどうなるか

---

# Railsアプリケーション

* RailsアプリケーションもまたRackアプリケーション
* Railsアプリケーションはリクエストがくると、Rackミドルウェアの処理を実施し、最後にRouterの処理を行う

---

# Railsアプリケーション

![](http://drive.google.com/uc?export=view&id=1kOC6gbyTG9VLJRGpS5ohpNuVFymDv7an)

---

# Railsアプリケーション

* Router(実際のクラスは`ActionDispatch::Routing::RouteSet`)はpublic APIで差し替え可能
  * 内部的には"endpoint"という表現を使用している
* https://edgeapi.rubyonrails.org/classes/Rails/Engine.html のdocに説明がある

---

# Railsアプリケーション

```ruby {style="font-size: 10p"}
def app
  @app || @app_build_lock.synchronize {
    @app ||= begin
      stack = default_middleware_stack
      config.middleware = build_middleware.merge_into(stack)
      config.middleware.build(endpoint)
    end
  }
end

def endpoint
  self.class.endpoint || routes
end

```

[https://github.com/rails/rails/blob/870377915af301c98a54f7f588e077610b2190aa/railties/lib/rails/engine.rb#L503-L518](https://github.com/rails/rails/blob/870377915af301c98a54f7f588e077610b2190aa/railties/lib/rails/engine.rb#L503-L518)

---

# endpoint

```ruby
# config/application.rb
class Application < Rails::Application
  endpoint RodaRoutes.freeze.app
  # ...
end
```
---

# 性能検証

|| Requests/sec|Latency(Avg)|
------------ | -------------| -------------
Rails | 190.89|26.20ms|
mount| 229.78|21.76ms|
endpoint|227.25|22.00ms|

---

# Rackミドルウェア

* Railsアプリケーションは様々なRackミドルウェアを使用している
* 使用しているミドルウェアの一覧は`rails middleware`コマンドで確認出来る

---

# Rackミドルウェア(API-only)

``` {style="font-size: 10p"}
$ RAILS_ENV=production  ./bin/rails middleware
use ActionDispatch::HostAuthorization
use Rack::Sendfile
use ActionDispatch::Executor
use ActiveSupport::Cache::Strategy::LocalCache::Middleware
use Rack::Runtime
use ActionDispatch::RequestId
use ActionDispatch::RemoteIp
use Rails::Rack::Logger
use ActionDispatch::ShowExceptions
use ActionDispatch::DebugExceptions
use ActionDispatch::Callbacks
use Rack::Head
use Rack::ConditionalGet
use Rack::ETag
```

---

# Rackミドルウェア

* デフォルトは全部入り
* しかし要らないミドルウェアもある
  * 例えば`Rack::Sendfile`はX-Sendfile header設定する為のミドルウェアだが、ファイルアップロード処理が無いアプリケーションなら要らない

---

# Rackミドルウェア

使わないミドルウェアは個別に削除出来る

```ruby
# config/application.rb
class Application < Rails::Application
  config.middleware.delete Rack::Sendfile
  # ...
end
```

---

# Rackミドルウェア

* まとめて削除機能は(public APIには)無い
* Rodaにも当然Rackミドルウェアを使用する機能はある
* Rodaで必要なRackミドルウェア だけを個別に指定すれば、そもそもHTTPリクエストの処理にRailsアプリケーションを使う必要はないのでは?

---

# Roda

```ruby
class RodaRoutes < Roda
  use Rack::JWT::Auth
  use ActionDispatch::RequestId

  # ...
end
```

---

# config.ru

```diff
require_relative 'config/environment'

- run Rails.application
+ run RodaRoutes.freeze.app
```

---

# 性能検証

|| Requests/sec|Latency(Avg)|
------------ | -------------| -------------
Rails | 190.89|26.20ms|
mount| 229.78|21.76ms|
endpoint|227.25|22.00ms|
Roda|281.94|17.74ms|

---

# Roda

* 割と速くなった
* ただこれだと全てのリクエストがRodaにいくので、RailsのRouterを使用していた場合、既に定義済みのルーティングを全てRodaに移植する必要がある
  * 割と大変
* もうちょっと楽にはじめたい

---

# Rackアプリケーション

* Rackサーバ起動時に指定出来るRackアプリケーションは1つ
* なら、ルーティングに応じてRackアプリケーションを切り替えるRackアプリケーションがあれば良いのでは

---

# railroad-switch

* 探したところそういうライブラリは無かった
* 無いなら作れば良いよね
  * https://github.com/y-yagi/railroad-switch

---

# config.ru

```ruby {style="font-size: 12p"}
Railroad::Switch.register(path: "/roda/graphql", app: RodaRoutes.freeze.app)
Railroad::Switch.fallback_to = Rails.application
run Railroad::Switch.app
```

---

# 性能検証

|| Requests/sec|Latency(Avg)|
------------ | -------------| -------------
Rails | 190.89|26.20ms|
mount| 229.78|21.76ms|
endpoint|227.25|22.00ms|
Roda|281.94|17.74ms|
Rails(SW)|188.27|26.52ms|
Roda(SW)|281.11|17.76ms|

---

# まとめ

* omakase is benri
* omakaseが自分たちのアプリケーションの期待するものと完全に一致するとは限らない
* アプリケーションに合わせて、適切なメニューを選べるようになるとそれはそれで便利で良いんじゃないでしょうか
* そのメニューの1つとして、Rodaを検討してみるのは如何でしょうか

---

# おまけ

因みに、Rodaでアプリケーションを作る方法についてもうちょっと知りたい方は、"First step of Roda"という同人誌を去年書いたのでそれを見て頂ければと

https://github.com/y-yagi/roda-first/blob/master/book.pdf

