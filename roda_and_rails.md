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
* bugs.ruby-lang.org にも良く出現している

---

# 何故Rodaか

---

# Rails is slow?

---

# Rails is slow?

* (そもそも速度ってWebサービス毎に求められるものは違うしフレームワークの処理だけじゃなくネットワークやDB等の諸々考慮すべきだよね、みたいな話は一旦置いておいて)
* Railsが遅い、という話は割と見る
* 実際どうなんでしょうね

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
  * その辺りも考慮して見てね

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
  * (これは筆者が勝手に呼んでいるだけで公式で名乗っている訳ではない)
* Rodaのcoreは本当に最低限の機能しか提供しておらず自分で使う機能を選ぶ必要がある
  * デフォルトだとテンプレートのレンダリングも出来ない

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

# Rails vs Roda

* デフォルトで全部入りのフレームワーク(Rails)とデフォルトで何も入っていないフレームワーク(Roda)を比較すればそれは勿論何も入ってない方が速い
* Railsはomakaseだけど、メニューは変えれる
* 上手いことRailsでRodaを使えないか?

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
* それならRodaを使えないか?

---

# 上手いことRodaの良い所をRailsに取り込めないか試してみよう

---

# Roda on Rails

* Rodaは当然Rackベース
* Rackベースのアプリケーションなので`mount`メソッドを使えばRailsのroutingで使える
  * doc参照 https://edgeapi.rubyonrails.org/classes/ActionDispatch/Routing/Mapper/Base.html#method-i-mount

---

# Roda Application

```ruby {style="font-size: 12p"}
class RodaRoutes < Roda
  # Rodaのrouting処理
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

# 性能検証

* とりあえず動く状態になったので性能検証見てみよう
* 今回はApache Benchを使用
* concurrency: 10、requests: 20000
* serverはproduction相当の設定で起動
* CPU: Intel® Core™ M-5Y71 Processor (4M Cache, up to 2.90 GHz) 、メモリ: 8Gのマシンで検証

---

# 性能検証(スクリプト)

```ruby {style="font-size: 16p"}
auth_token = JWT.encode(payload, private_key, "RS256")

File.write("ab_post", "query { repositories { id } }".to_json)
script = <<EOS
  ab -p ab_post -H 'Authorization: Bearer #{auth_token}' -c 10 -n 20000 http://localhost:3000/graphql
EOS
system(script)
```

---

# 性能検証

**Rails result**

``` {style="font-size: 14p"}
Requests per second:    349.34 [#/sec] (mean)
Time per request:       28.625 [ms] (mean)
Time per request:       2.863 [ms] (mean, across all concurrent requests)
Transfer rate:          98.59 [Kbytes/sec] received
                        330.24 kb/s sent
                        428.83 kb/s total
```

{.column}

**Roda result**

``` {style="font-size: 14p"}
Requests per second:    467.87 [#/sec] (mean)
Time per request:       21.373 [ms] (mean)
Time per request:       2.137 [ms] (mean, across all concurrent requests)
Transfer rate:          134.33 [Kbytes/sec] received
                        444.57 kb/s sent
                        578.90 kb/s total
```

---

# 性能検証

* RailsにそのままRodaをのっけただけでも多少速くなる
  * Controllerの処理を通らなくなるため、ログが出なくなる等挙動の違いはある

---

# 他のパターンも試してみよう

先のパターンではRailsのrouterはそのまま使用したが、そこを変えたらどうなるか

---

# Rails application

* Rails applicationとしては、HTTP requestがくるとRack Middlewareで処理を実施、最後にRouterで処理を行う

---

# Rails application

![](http://drive.google.com/uc?export=view&id=1kOC6gbyTG9VLJRGpS5ohpNuVFymDv7an)

---

# Rails application

* このRouter(実際のクラスは`ActionDispatch::Routing::RouteSet`)は実は差し替え可能
  * 内部的には"endpoint"という表現を使用している

---

# Rails application

```ruby {style="font-size: 12p"}
# Returns the underlying Rack application for this engine.
def app
  @app || @app_build_lock.synchronize {
    @app ||= begin
      stack = default_middleware_stack
      config.middleware = build_middleware.merge_into(stack)
      config.middleware.build(endpoint)
    end
  }
end

# Returns the endpoint for this engine. If none is registered,
# defaults to an ActionDispatch::Routing::RouteSet.
def endpoint
  self.class.endpoint || routes
end
```

https://github.com/rails/rails/blob/870377915af301c98a54f7f588e077610b2190aa/railties/lib/rails/engine.rb#L503-L518

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

* このアプローチではあまり速くならなかった
  * `mount`を使った場合と同じ程度の速度

---

# 最後にもうひとつ別のアプローチ

---

# Rack Middlewares

---

# Rack Middlewares

* Railsアプリケーションは様々なRack Middlewaresを使用している
* 使用しているmiddlewareの一覧は`rails middleware`で確認出来る

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
* Rodaで必要なRack middlewareだけを個別に指定すれば、そもそもRequestの処理にRails applicationは不要なのでは?
  * 勿論Railsで使う事を前提としているmiddlewareはそのままでは使えない

---

# config.ru

```diff
# This file is used by Rack-based servers to start the application.

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
  use Rails::Rack::Logger, Rails.application.config.log_tags
end
```

---

# 性能検証

**Rails result**

``` {style="font-size: 14p"}
Requests per second:    349.34 [#/sec] (mean)
Time per request:       28.625 [ms] (mean)
Time per request:       2.863 [ms] (mean, across all concurrent requests)
```

{.column}

**Roda result(今回の結果)**

``` {style="font-size: 14p"}
Requests per second:    595.30 [#/sec] (mean)
Time per request:       16.798 [ms] (mean)
Time per request:       1.680 [ms] (mean, across all concurrent requests)
```

---

# まとめ

* Rails is omakase
* とはいえメニューは変えられる
* 自分達のアプリケーションに合わせて、適切にメニューを選べるようになると、それはそれで便利で良いんじゃないでしょうか
