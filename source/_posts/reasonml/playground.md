title: 如何在本機打造 ReasonML Playground ?
tags:
  - ReasonML
  - Nodemon
feature: images/feature/reasonml.png
date: 2019-05-26 18:32:23
---
ReasonML 官方雖然提供了 Bsb、rtop 與 Sketch.sh 三種測試平台，但或許你還是比較喜歡 VS Code + Quokka 的方式，只可惜目前 Quokka 尚未支援 ReasonML，讓我們自己打造類似 Quokka 的測試環境。

<!-- more -->

## Version

macOS Mojave 10.14.5
VS Code 1.34.0
ReasonML 3.0.4
Bsb 5.0.4
Nodemon 1.18.0

## 安裝 Node

(略)

## 安裝 Yarn

(略)

## 安裝 Bsb

```shell
$ yarn global add bs-platform
```

`Bsb` 為 ReasonML 的建置系統，負責將 ReasonML 編譯成 ECMAScript。

![reason000](/images/reasonml/playground/reason000.png)

## 建立 Node 專案

```shell
$ bsb -init reasonml-playground -theme basic-reason
```

使用 `bsb` 建立 Node 專案。

* `-init`：建立 sample project
* `-theme`：sample project 主題，使用 `basic-reason`

> 目前 Bsb 支援兩個主題：
>
> - `basic-reason`：建立 Node 專案
> - `react`：建立 ReasonReact 專案

![reason001](/images/reasonml/playground/reason001.png)

##  安裝 Nodemon

```shell
$ yarn global add nodemon
```

將使用 nodemon 監視 `*.js` 的變動，自動重新執行 Node。

![reason002](/images/reasonml/playground/reason002.png)

## 開啟 VS Code

**package.json**

```json
"nodemon": "nodemon src/Demo.bs.js"
```

使用 VS Code 開啟 `package.json`，在 `scripts` 新增 `nodemon`。

![reason003](/images/reasonml/playground/reason003.png)

## Yarn start

```shell
$ yarn start
```

當 `*.re` 按下存檔時，Bsb 會自動將 ReasonML 編譯成 ECMAScript。

![reason004](/images/reasonml/playground/reason004.png)

## Yarn nodemon

```shell
$ yarn nodemon
```

當 `*.js` 改變時，Node 會自動重新執行。

![reason005](/images/reasonml/playground/reason005.png)

改變檔案後，由於 VS Code 會自動存檔，結果會立刻改變。

![reason006](/images/reasonml/playground/reason006.png)

## Conclusion

* Sketch.sh 雖然好用，但無法測試學習 BuckleScript 部分
* 透過 nodemon，我們也可以在熟悉的 VS Code 測試學習 ReasonML + BuckleScript，並且立即顯示結果

## Reference

[Dr. Axel Rauschmayer](https://twitter.com/rauschma), [Running code snippets via Node.js and nodemon](http://2ality.com/2018/08/nodemon-code-snippets.html)
[Remy Sharp](https://github.com/remy), [nodemon](https://github.com/remy/nodemon)

