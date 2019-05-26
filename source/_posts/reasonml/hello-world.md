title: ReasonML 初體驗
tags:
  - ReasonML
feature: images/feature/reasonml.png
date: 2019-05-26 18:18:13
---
雖然也可以直接建立 ReasonReact 專案練習 ReasonML，但若只想學習 ReasonML，是否有更簡單的方式呢 ? 本文整理出 Bsb、Reason CLI 與 Sketch.sh 三種學習 ReasonML 方式。

<!-- more -->

## Version

macOS Mojave 10.14.5
Node 10.15.3
Yarn 1.15.2
ReasonML 3.0.4
Bsb 5.0.4

## 安裝 Node

(略)

## 安裝 Yarn

(略)

## Bsb

```shell
$ yarn global add bs-platform
```

`Bsb` 為 ReasonML 的建置系統，負責將 ReasonML 編譯成 ECMAScript。

![reason000](/images/reasonml/hello-world/reason000.png)

```shell
$ bsb -init reasonml-hello-world -theme basic-reason
```

使用 `bsb` 建立 Node 專案。

* `-init`：建立  sample project
* `-theme`：sample project 主題，使用 `basic-reason`

> 目前共有兩個主題：
>
> * `basic-reason`：建立 Node 專案
> * `react`：建立 ReasonReact 專案

![reason001](/images/reasonml/hello-world/reason001.png)

```shell
$ cd reasonml-hello-world
$ yarn build
$ node src/Demo.bs.js
```

進入專案目錄，輸入 `yarn build` 將 ReasonML 編譯成 ECMAScript。

ReasonML 的 source code 都會放在 `src` 目錄下，`yarn build` 會將 `*.re` 編譯成 `*.bs.js`。

使用 Node 執行 `*.js`。

![reason002](/images/reasonml/hello-world/reason002.png)

## Reason CLI

```shell
$ yarn global add reason-cli@latest-macos
```

Reason CLI 包含以下工具：

* `ocamlmerlin`：為 editor 提供 OCaml 語法提示
* `ocamlerlin-reason`：為 editor 提供 ReasonML 語法提示
* `refmt`：ReasonML 的 code formatter
* `rtop`：ReasonML 的 REPL

> Reason CLI 有點大，需要一點時間安裝

![reason003](/images/reasonml/hello-world/reason003.png)

```shell
$ rtop
```

輸入 `rtop` 進入 ReasonML 的 REPL。

![reason004](/images/reasonml/hello-world/reason004.png)

1. 輸入 `rtop`
2. 輸入 `let msg = "Hello World"` 將 `Hello World` 字串 binding 到 `msg`
3. 使用 `print_string()` 將 `msg` 印出
4. 當輸入時，下方會顯示相關 keyword 或 function

> 輸入 `#quit;` 或熱鍵 `⌃ + D` 離開 `rtop`

## Sketch.sh

![reason006](/images/reasonml/hello-world/reason006.png)

若你只想嚐試 ReasonML，而不想安裝 Bsb 或 Reason CLI，也可以到 [Sketch.sh](https://sketch.sh) 測試 ReasonML，可將 [Sketch.sh](https://sketch.sh) 視為線上版的 rtop。

除此之外，Sketch.sh 還有個好處，當你要將 code 分享給他人時，可以將其 `Save`，將會產生專屬 url，類似 gist。

## Conclusion

* 以學習 ReasonML 角度，最方便的是 [Sketch.sh](https://sketch.sh)，包含語法顏色，也包含 formatter，相當於 VSCode 的 Quokka

## Reference

[Dr.Axel Rauschmayer](https://twitter.com/rauschma), [Exploring ReasonML and functional programming](http://reasonmlhub.com/exploring-reasonml/)
[Reasonml](https://reasonml.github.io/en/), [Installation](https://reasonml.github.io/docs/en/installation)
[Reasonml](https://github.com/reasonml), [reason-cli](https://github.com/reasonml/reason-cli)
[Sketch.sh](https://sketch.sh)

