title: 使用 uniqWith() 提供特殊比較方式取得不重複資料
tags:
  - Ramda
  - Ramda/uniqBy
  - Ramda/uniqWith
  - Ramda/eqBy
  - Ramda/anyPass
  - Ramda/eqProps
feature: images/feature/ramda.png
date: 2019-05-13 20:28:27
---
對於一般需求，`uniq()` 即可勝任，但若需自行提供特殊比較方式，則要使用 `uniqWith()`，自行傳入 Callback。

<!-- more -->

## Version

VS Code 1.33.1
Quokka 1.0.212
Ramda 0.26.1

## uniqBy()

```javascript
import { uniqBy } from 'ramda';

let data = [1, 2, 3, -1];

// fn :: [a] -> [a]
let fn = uniqBy(x => Math.abs(x));

console.log(fn(data));
```

`data` 中包含 `1` 與 `-1`，若`1` 與  `-1` 均視為相同而不想重複顯示，也就是我們希望結果只顯示 `[1, 2, 3]`。

> **uniqBy()**
> `(a -> b) -> [a] -> [a]`
> 將 array 經過 supply function 轉換，回傳 element 不重複 array

![uniqwith001](/images/ramda/uniqwith/uniqwith001.png)

## uniqWith()

```javascript
import { uniqWith, eqBy } from 'ramda';

let data = [1, 2, 3, -1]

// fn :: [a] -> [a]
let fn = uniqWith((x, y) => Math.abs(x) === Math.abs(y));

console.log(fn(data));
```

我們也可以使用 `uniqWith()` 完成。

> **uniqWith()**
> `((a, a) → Boolean) → [a] → [a]`
> 自行提供 predicate 決定比較方式，回傳 element 不重複 array

`((a, a) → Boolean)`：predicate function，自行決定比較方式

`[a]`：data 為 array

`[a]`：回傳 element 不重複 array

> 由 Ramda source code 得知，`uniqWith()` 其實是兩層 `for` loop，第一層為第一個 argument，第二層為第二個 argument，也就是第一個 argument 代表 array 中每個 element，第二個 argument 代表所比較 array 中每個 element

![uniqwith002](/images/ramda/uniqwith/uniqwith002.png)

## Point-free

```javascript
import { uniqWith, eqBy } from 'ramda';

let data = [1, 2, 3, -1]

// fn :: [a] -> [a]
let fn = uniqWith(eqBy(Math.abs));

console.log(fn(data));
```

`(x, y) => Math.abs(x) === y` 可進一步由 `eqBy(Math.abs)` 化簡成 point-free。

> **eqBy()**
> `(a → b) → a → a → Boolean`
> 若兩 value 經過 function 轉換後結果相同則回傳 `true`，否則 `false`

![uniqwith000](/images/ramda/uniqwith/uniqwith000.png)

## Object

```javascript
import { uniqWith } from 'ramda';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 },
  { title: 'FP in JavaScript', price: 400 },
  { title: 'Exploring ReasonML', price: 200 },
];

// fn :: [a] -> [a]
let fn = uniqWith((x, y) => x.title === y.title || x.price === y.price);

console.dir(fn(data));
```

若 element 為 object，只要 `title` 或 `price` 相等就視為相同，也就是第四筆與第一筆相同，第五筆與第二筆相同，我們希望結果只顯示不重複的前三筆。

此時就不能再使用 `uniqBy()`，因為條件太特殊，只能使用 `uniqWith()`。

![uniqwith003](/images/ramda/uniqwith/uniqwith003.png)

## Point-free

```javascript
import { uniqWith, anyPass, eqProps } from 'ramda';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 },
  { title: 'FP in JavaScript', price: 400 },
  { title: 'Exploring ReasonML', price: 200 },
];

// fn :: [a] -> [a]
let fn = uniqWith(anyPass([eqProps('title'), eqProps('price')]));

console.dir(fn(data));
```

`(x, y) => x.title === y.title || x.price === y.price` 也可由 `anyPass()` 與 `eqProps()` 產生，使其 point-free。

> **anyPass()**
> `[(*… → Boolean)] → (*… → Boolean)`
> 將眾多 predicate 組合成單一 predicate，只要任何 predicate 成立即可

![uniqwith004](/images/ramda/uniqwith/uniqwith004.png)

## Conclusion

* Unique series 三兄弟：`uniq()`、`uniqBy()` 與 `uniqWith()`，其中 `uniqWith()` 算客製化程度最高的 function，可自行決定比較方式
* Ramda 若很難從官網說明明白其意義，直接從其 source code 了解也是不錯方式，`uniqWith()` 就是一例

## Reference

[Ramda](https://ramdajs.com), [uniqBy()](https://ramdajs.com/docs/#uniqBy)
[Ramda](https://ramdajs.com), [uniqWith()](https://ramdajs.com/docs/#uniqWith)
[Ramda](https://ramdajs.com), [eqBy()](https://ramdajs.com/docs/#eqBy)
[Ramda](https://ramdajs.com), [anyPass()](https://ramdajs.com/docs/#anyPass)
[Ramda](https://ramdajs.com), [eqProps()](https://ramdajs.com/docs/#eqProps)