# duct-crud-compojure

Webフレームワーク「Duct」入門サンプルコードはいくつかウェブ上にあるけれど、<br>
なぜか判で押したように ataraxy と PostgreSQL の組み合わせ。あとは、不必要に docker と絡めていたり。<br>
…えーと、僕はローカル環境で「Duct」を試したいだけなんすけどぉ？<br>

動作に必要な「最小限度」ってどんなんだろう、Compojure と MySQL じゃダメなの？<br>
…ということで、Compojure と MySQL で動くよう試行錯誤した「Duct」のサンプルコードです。<br>

あと、「個人の感想ですが」と断りつつも。<br>
なんだか ataraxy 版だと、設定ファイル（.edn）が EJB の .xml じみてるな、と。<br>

・ルーティングごとの関数名に、いちいち長々クラスパスがついている。<br>
・その関数の処理が DB アクセスをともなう場合、{:db #ig/ref :duct.database/sql} といちいち書くのぉ！？<br>
・で、その関数実体は別ファイルに移動して確認しないといけない、と。<br>

```edn
;; [ duct-crud-practice/resources/duct-crud-practice/config.edn ]

{:duct.profile/base
 {:duct.core/project-ns duct-crud-practice

  :duct.router/ataraxy
  {:routes { [:get "/example"] [:duct-crud-practice.handler/example]
             ["/users"] { [:get] [:duct-crud-practice.handler/user-index]
                          [:get "/new"] [:duct-crud-practice.handler/user-new]
                          [:post "/" {body :params}] [:duct-crud-practice.handler/user-create body]

                          [:get "/" id] [:duct-crud-practice.handler/user-show id]
                          [:get "/" id "/edit"] [:duct-crud-practice.handler/user-edit id]

                          [:post "/" id "/update" {body :params}] [:duct-crud-practice.handler/user-update id body]
                          [:post "/" id "/delete"] [:duct-crud-practice.handler/user-del id] }
             }
   }

  :duct-crud-practice.handler/example {}

  :duct-crud-practice.handler/user-index
  {:db #ig/ref :duct.database/sql}

  :duct-crud-practice.handler/user-new {}

  :duct-crud-practice.handler/user-create
  {:db #ig/ref :duct.database/sql}

  :duct-crud-practice.handler/user-show
  {:db #ig/ref :duct.database/sql}

  :duct-crud-practice.handler/user-edit
  {:db #ig/ref :duct.database/sql}

  :duct-crud-practice.handler/user-update
  {:db #ig/ref :duct.database/sql}

  :duct-crud-practice.handler/user-del
  {:db #ig/ref :duct.database/sql}

  }

 :duct.profile/dev   #duct/include "dev"
 :duct.profile/local #duct/include "local"
 :duct.profile/prod  {}

 :duct.module/logging {}

  ;; add
 :duct.module/web {}
 :duct.module/sql
 {:database-url "jdbc:mysql://localhost:3306/test?user=root&password=password"}
}
```
「ボイラープレート」ってゆーんじゃなかったっけ？<br>
全然 Clojure らしくないな、と（ソースコードのように、関数、マクロで重複部分を抽象化することもできない）。<br>
…なので、ataraxy 版の印象良くないっす。<br><br>

このサイトを参考にしました。<br>

「<a href="https://asukiaaa.blogspot.com/2017/12/clojureductcrud.html" target="_blank" rel="noopener noreferrer">試行錯誤な日々</a>」 [ `https://asukiaaa.blogspot.com/2017/12/clojureductcrud.html` ]<br>

## Developing

### Setup

When you first clone this repository, run:

```sh
lein duct setup
```

This will create files for local configuration, and prep your system
for the project.

### Environment

To begin developing, start with a REPL.

```sh
lein repl
```

Then load the development environment.

```clojure
user=> (dev)
:loaded
```

Run `go` to prep and initiate the system.

```clojure
dev=> (go)
:duct.server.http.jetty/starting-server {:port 3000}
:initiated
```

By default this creates a web server at <http://localhost:3000>.

When you make changes to your source files, use `reset` to reload any
modified files and reset the server.

```clojure
dev=> (reset)
:reloading (...)
:resumed
```
<!---
### Testing

Testing is fastest through the REPL, as you avoid environment startup
time.

```clojure
dev=> (test)
...
```

But you can also run tests through Leiningen.

```sh
lein test
```
-->

## Legal

Copyright © 2021 FIXME
