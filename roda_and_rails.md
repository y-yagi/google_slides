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
* Railsをお使い(であろう)皆さんへのRodaのすゝめ

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
* Rails的にはErubi(ERB Handler) author

---

# Jeremy Evans

![](http://drive.google.com/uc?export=view&id=1ZW030sbr-LC1riwFSqAVM3ejjs1b-IAw)

https://rubykaigi.org/2019/speakers

---

# Jeremy Evans

![](http://drive.google.com/uc?export=view&id=1NYtjgWwgdPNawDhL0xBlZxEyDHZkFTQa)

https://rubykaigi.org/2019/presentations/jeremyevans0.html#apr20

---

# 何故Rodaか

---

# Roda

* Rodaは特徴の1つに速度がある

---

# Roda is fast

![](http://drive.google.com/uc?export=view&id=13KekYhxZ82d9S1bQNVVx7Lgy7RiqAP__)

http://roda.jeremyevans.net/

---

# Roda is fast

* (そもそも速度ってWebサービス毎に求められるものは違うしフレームワークの処理だけじゃなくネットワークやDB等の諸々考慮すべきだよね、みたいな話は一旦置いておいて)
* 上のベンチマークは[luislavena/bench-micro](https://github.com/luislavena/bench-micro)の結果
  * "Hello World!"を返すだけのアプリ
* これだけみるとRails遅い
* 他のベンチマークも見てみたい

---

# Web Framework Benchmarks

* TechEmpower社がおこなっているWeb Framework Benchmarks(https://www.techempower.com/benchmarks/) というベンチマークがある
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

* ベンチマークを取るのに使用したソースも公開されているので、興味がある方はそちらもどうぞ
  * https://github.com/TechEmpower/FrameworkBenchmarks

---

# Web Framework Benchmarks

* JSON serializationの結果を見てみると、Railsは290 / 328
  * Rubyの中で最下位
  * https://www.techempower.com/benchmarks/#section=data-r17&hw=cl&test=json&l=zijxtr-1
* Roda(+ Sequel)は上位

---

# Roda is fast?

* そもそも提供している機能が違うので単純に比較するべきではない
* もちろんMicro frameworksだと速くなる、というわけではないが
  * DjangoとflaskだとDjangoの方が上にいたりする

---

# Roda is fast?

* Roda is à la carte
  * これは私が勝手に呼んでいるだけで公式で名乗っている訳ではない
* Rodaのcoreは本当に最低限の機能しか提供していない
  * デフォルトだとテンプレートのレンダリングも出来ない
* 代わりに各種機能をpluginとして提供しており、使用する側で必要なpluginを選択する必要がある

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

* デフォルトで全部入りのフレームワーク(Rails)とデフォルト最小構成のフレームワーク(Roda)を比較すればそれは勿論最小構成の方が速い
* 勿論それ以外にもRodaには速度向上のための対応が色々と行われている
  * この辺りの話はRubyKaigiで聞けそうな気がするんでみんな楽しみにしよう

---

# Roda or Rails

* Rodaは速いが、あくまでルーティングライブラリなので、実際これを使ってアプリを作ろうと思うと、Railsと比べると当然大変
  * Railsのコマンドやrakeタスク、autoloadやreloadの仕組み、設定ファイル等に関する処理は提供されてないので自分で準備する必要がある
  * 当ディレクトリ構成等も考える必要がある
* Railsの便利な部分と、Rodaの速い部分を上手いこと組み合わせられないか?

---

# Rails Router{.big}

Rodaは"Routing Tree Web Framework Toolkit"なので、Routingについて考えてみる

---

# RailsのRouter

* URLとcontrollerのactionとのマッピング
* pathとURLのhelper
* Viewの為のhelper(e.g. `url_for`)
* scope(namespace)
* constraints
* journey

---

# Rails Router

* Rails Routerは多機能
* 複雑なウェブアプリケーションを作成するためにはこれらの機能は必要だった(多分)
* Rodaの標準のpluginでは、完全に同じ機能を提供する事は出来ない

---

# 今はどうか

* 元々の役割は変わらずあるが、それだけではなくなった
* SPAアプリの場合サーバ側はAPIだけで良い事もある
  * その場合そこまで複雑なRoutingが必要ではない
  * helperメソッドも無くても良い
* 例えばGraphQLの場合、POSTのエンドポイントが1つあれば良い

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
* 多機能なRouterは不要そう
* それならRodaを使えないか?

---

# Roda on Rails

* Rodaは当然Rackベース
* Rackベースのアプリケーションは`mount`メソッドを使えばRailsのroutingで使える
  * https://edgeapi.rubyonrails.org/classes/ActionDispatch/Routing/Mapper/Base.html#method-i-mount

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

```ruby {style="font-size: 12p"}
class RodaRoutes < Roda
  # ...
end
```

```ruby {style="font-size: 12p"}
# routes.rb
Rails.application.routes.draw do
  post "/graphql", to: "graphql#execute"
  mount RodaRoutes.freeze.app => "/roda"
end
```

---

# 性能検証

* とりあえず動く状態になったので性能検証してみよう
* 各エンドポイントに対して結果が1件だけ取得できるqueryをPOST
* 先に提示した以外のコードは完全に一緒
* ベンチマークツールは wrk(https://github.com/wg/wrk)を使用
* 10 threads / 100 connectionsで30s

---
# 性能検証

* Rails: 6.0.0.beta3, Roda: 3.18.0
* CPU: Intel® Core™ M-5Y71 Processor (4M Cache, up to 2.90 GHz) 、メモリ: 8Gのマシンで検証

---

# 性能検証

**Rails**

``` {style="font-size: 14p"}
  10 threads and 100 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    26.20ms    7.52ms  73.32ms   73.87%
    Req/Sec   191.41     16.16   222.00     73.75%
  5744 requests in 30.09s, 2.93MB read
Requests/sec:    190.89
Transfer/sec:     99.55KB
```

{.column}

**Roda**

``` {style="font-size: 14p"}
  10 threads and 100 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    21.76ms    5.81ms  60.93ms   76.23%
    Req/Sec   230.66     16.43   260.00     79.07%
  6916 requests in 30.10s, 1.89MB read
Requests/sec:    229.78
Transfer/sec:     64.18KB
```

---

# 性能検証

* RailsにそのままRodaのっけただけでも多少速くなる

---

# 他のパターンも試してみよう

先のパターンではRailsのrouterはそのまま使用したがそこを変えたらどうなるか

---

# Railsアプリケーション

* RailsアプリケーションもまたRackアプリケーション
* RailsアプリケーションはHTTP requestがくるとRack Middlewareの処理を実施し、最後にRouterで処理を行う

---

# Rails application

![](http://drive.google.com/uc?export=view&id=1kOC6gbyTG9VLJRGpS5ohpNuVFymDv7an)

---

# Rails application

* Router(実際のクラスは`ActionDispatch::Routing::RouteSet`)は差し替え可能
  * 内部的には"endpoint"という表現を使用している
* https://edgeapi.rubyonrails.org/classes/Rails/Engine.html のdocに説明がある

---

# endpoint

```
# config/application.rb
class Application < Rails::Application
  endpoint RodaRoutes.freeze.app
  # ...
end
```
---

# endpoint

* 結論からいうとこのアプローチではあまり速くならなかった
  * `mount`を使った場合と同じ程度の速度
* 次行ってみよう

---

# Rack Middlewares

* Railsアプリケーションは様々なRack Middlewaresを使用している
* 使用しているmiddlewareの一覧は`rails middleware`コマンドで確認出来る

---

# Rack Middlewares(API-only)

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

# Rack Middlewares

* 色々ある
* しかし要らないmiddlewareもあるのでは
  * 例えば`Rack::Sendfile`はX-Sendfile header設定する為のmiddlewareだが、ファイルアップロード処理が無いアプリケーションなら要らないよね

---

# Rack Middlewares

使わないMiddlewareは個別に削除出来る

```
# config/application.rb
class Application < Rails::Application
  config.middleware.delete Rack::Sendfile
  # ...
end
```

---

# Rails application

* RodaにもRack middlewareを指定する機能はある
* Rodaで必要なRack middlewareだけを個別に指定すれば、そもそもRequestの処理にRailsアプリケーションは不要なのでは?

---

# config.ru

```diff
require_relative 'config/environment'

- run Rails.application
+ run RodaRoutes.freeze.app
```

---

# Roda

```
class RodaRoutes < Roda
  use Rack::JWT::Auth
  use ActionDispatch::RequestId

  # ...
end
```

---

# 性能検証

**Rails result**

``` {style="font-size: 14p"}
  10 threads and 100 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    26.20ms    7.52ms  73.32ms   73.87%
    Req/Sec   191.41     16.16   222.00     73.75%
  5744 requests in 30.09s, 2.93MB read
Requests/sec:    190.89
```

{.column}

**Roda result(今回の結果)**

``` {style="font-size: 14p"}
  10 threads and 100 connections
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency    17.85ms    4.89ms  65.38ms   75.27%
    Req/Sec   140.38     30.76   202.00     75.00%
  8398 requests in 30.05s, 0.94MB read
Requests/sec:    279.50
Transfer/sec:     31.93KB
```

---

# まとめ

* Rails is omakase
* とはいえメニューは変えられる
* 自分達のアプリケーションに合わせて、適切にメニューを選べるようになると、それはそれで便利で良いんじゃないでしょうか
* Rodaでアプリケーションを作る方法について知りたい方は、"First step of Roda"という同人誌を去年書いたのでそれを見て下さい
