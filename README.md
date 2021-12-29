# duct-crud-compojure

Webフレームワーク「Duct」入門サンプルコードはいくつかウェブ上にあるけれど、<br>
なぜか、どれも判で押したように ataraxy と PostgreSQL の組み合わせ。あとは、不必要に docker と絡めていたり。<br>
…えーと、僕はローカル環境で「Duct」を試したいだけなんすけどぉ？<br>

動作に必要な「最小限度」ってどんなんだろう、Compojure と MySQL じゃダメなの？<br>
…ということで、Compojure と MySQL で動くよう試行錯誤した「Duct」のサンプルコードです。<br>

ataraxy 版だと、設定ファイル（.edn）がどんどん膨れあがる（EJB の .xml じみてる）。<br>
全然 Clojure らしくないな、と（ソースコードのように、関数、マクロで重複部分を抽象化することもできない）。<br>

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
