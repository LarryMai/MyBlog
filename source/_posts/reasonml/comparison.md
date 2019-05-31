title: ECMAScript、Ramda 與 ReasonML 簡單比較
tags:
  - Ramda
  - ReasonML
  - ECMAScript
feature: images/feature/reasonml.png
date: 2019-05-31 11:56:00
---
ReasonML 與 Ramda 都試圖以 FP 改造 ECMAScript，前者是以 Static Type Language 編譯成 JS，後者則以 Library 掛在 JS 之上，本文以簡單範例初步比較三者差異。

<!-- more -->

## Version

VS Code 1.34.0
Quokka 1.0.219
Ramda 0.26.1
ReasonML 3.0.4
BuckleScript 3.1.0

## ECMAScript

```javascript
let data = [
  { title: "FP in JavaScript", price: 100 },
  { title: "RxJS in Action", price: 200 },
  { title: "Speaking JavaScript", price: 300 }
];

// fn :: Number -> [a] -> [a]
let fn = price => arr => arr
  .filter(x => x.price === price)
  .map(x => x.title)

fn(300)(data); //?
```

* ECMAScript 若要支援 currrying，要以兩個 `=>` 完成

* ECMAScript 若要將 `filter()` 與 `map()` 串起來，要以 OOP 的 method chaining
* ECMAScript 為 dynamic type language，因此只能自己加上 type signature

![cross001](/images/reasonml/comparison/cross001.png)

## Ramda

```javascript
import { pipe, filter, map, prop, propEq } from 'ramda';

let data = [
  { title: "FP in JavaScript", price: 100 },
  { title: "RxJS in Action", price: 200 },
  { title: "Speaking JavaScript", price: 300 }
];

// fn :: Number -> [a] -> [a]
let fn = price => pipe(
  filter(propEq('price', price)),
  map(prop('title'))
);

fn(300)(data); //?
```

* Ramda 以 `pipe()` 將 `filter()` 與 `map()` 串起來，且順便 point-free 把 `arr` parameter 省下來，也同時支援 currying
* `filter()` 與 `map()` 的 callback 可用 `propEq()` 與 `prop()` 產生 arrow function，連 callback 也 point-free
* 需要將 Ramda 所使用的 function 一一 import 進來
* Ramda 只能自己加上 type signature

![cross000](/images/reasonml/comparison/cross000.png)

## ReasonML

```ocaml
type book = {
  title: string,
  price: int
};

let data = [
  { title: "Fp in JavaScript", price: 100 },
  { title: "RxJS in Action", price: 200 },
  { title: "Speaking JavaScript", price: 300 }
];

let fn = (price, arr) => arr
  |> List.filter(x => x.price == price) 
  |> List.map(x => x.title) 
  |> Array.of_list

fn(300, data) |> Js.log
```

* ReaonML 為 static type language，因此要使用 `type` 宣告 record，可以想成是強型別的 object，因此 `data` 型別為 `list(book)`
* ReasonML 支援 auto currying，使用單一 `=>` 即可
* ReasonML 支援 `|>` operator，不需使用 `pipe()` function

* ReasonML 對於 module 不是使用 `import` 而是直接使用，因此為 `List.filter()` 與 `List.map()`
* `Array.of_list()` 不是 camelCase，因為是 OCaml 的 function
* `Js.log()` 為 BuckleScript 所提供，相當於 JS 的 `console.log()`
* ReasonML 支援 type inference，可自行推導出 type signature

![cross002](/images/reasonml/comparison/cross002.png)

## Conclusion

* ECMAScript 僅支援有限 FP，但藉由 method chaining 實現 pipe，語法也還算直覺
* Ramda 以 library 層次支援 FP，尤其連 callback 也能 point-free，但 `pipe()` function 還是沒 `|>` operator 漂亮
* ReasonML 以 language 層次支援 FP，而且是 static type，唯有些 function 還必須借助 OCaml，因此不是 camelCase，看起來很礙眼
* 僅 ReasonML 支援 type inference，ECMAScript 與 Ramda 都必須手動加上 type signature

* ECMAScript、Ramda 與 ReasonML 三種寫法，你喜歡哪種風格呢 ?