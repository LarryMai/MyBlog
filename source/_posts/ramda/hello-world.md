title: Ramda 初體驗
tags:
  - Ramda
  - Ramda/pipe
  - Ramda/map
  - Ramda/filter
  - Ramda/prop
  - Ramda/propEq
feature: images/feature/ramda.png
date: 2018-05-16 13:52:39
---
一直很羨慕 F# 的 `List` module 提供了豐富的 Function，而 ECMAScript 的 `Array.prototype` 卻只提供有限的 Function 可用，因此無法完全發揮 FP 威力。但這一切終於得到解決，Ramda 擁有豐富的 Function，且很容易自行開發 Function 與 Ramda 整合使用。

<!-- more -->

## Version

VS Code 1.34.0
Quokka 1.0.216
Ramda 0.26.1

## Imperative

```javascript
let data = [
  { title: 'fp in JavaScript', price: 100 },
  { title: 'rxJS in Action', price: 200 },
  { title: 'speaking JavaScript', price: 300 }
];

// fn :: Number -> [a] -> [b]
let fn = price => arr => {
  let result = [];
  
  for(let i = 0; i < arr.length; i++) {
    if (data[i].price === price) {
      result.push(data[i].title);
    }
  }

  return result;
};

console.log(fn(300)(data));
```

很簡單的需求，`data` 有各書籍資料，包含 `title` 與 `price`，我們想得到 `price` 為 `300` 的資料，只要 `title` 即可。

若使用 Imperative 寫法，我們會使用 `for` loop，先建立要回傳的 `result` array，由 `if` 去判斷 `price === 300`，再將符合條件的 `title` 寫入 `result` array，最後再回傳。

![ramda000](/images/ramda/hello-world/ramda000.png)

## Array.Prototype

```javascript
let data = [
  { title: 'fp in JavaScript', price: 100 },
  { title: 'rxJS in Action', price: 200 },
  { title: 'speaking JavaScript', price: 300 }
];

// fn :: Number -> [a] -> [b]
let fn = price => arr => arr
  .filter(x => x.price === price)
  .map(x => x.title);

console.log(fn(300)(data));
```

熟悉 FP 的讀者會很敏感發現，這就是典型 `filter()` 與 `map()` 而已，我們可直接使用 ECMAScript 在 `Array.prototype` 內建的 `filter()` 與 `map()` 即可完成需求。

![ramda001](/images/ramda/hello-world/ramda001.png)

## Ramda

```javascript
import { pipe, filter, map } from 'ramda';

let data = [
  { title: 'fp in JavaScript', price: 100 },
  { title: 'rxJS in Action', price: 200 },
  { title: 'speaking JavaScript', price: 300 }
];

// fn :: Number -> [a] -> [b]
let fn = price => pipe(
  filter(x => x.price === price),
  map(x => x.title)
);

console.log(fn(300)(data));
```

Ramda 身為 Functional Library，內建 `filter()` 與 `map()` 自然不在話下。

我們可先用 `pipe()` 將 `filter()` 與 `map()`  `組合` 出新的 `fn()`，再將 `data` 傳入。

至於 `filter()` 與 `map()` 要傳入的 callback，也可使用 arrow function。

> 我們發現 `fn()` 的 `arr` parameter 不見了，此稱為 point-free，讓程式碼更為精簡

![ramda002](/images/ramda/hello-world/ramda002.png)

## Point-free

```javascript
import { pipe, filter, map, propEq, prop } from 'ramda';

let data = [
  { title: 'fp in JavaScript', price: 100 },
  { title: 'rxJS in Action', price: 200 },
  { title: 'speaking JavaScript', price: 300 }
];

// fn :: Number -> [a] -> [b]
let fn = price => pipe(
  filter(propEq('price', price)),
  map(prop('title'))
);

console.log(fn(300)(data));
```

既然 `fn()` 能 point-free，`filter()` 與 `map()` 的 callback 也能 point-free 嗎 ?

* 由 `proEq('price', price)` 產生 `x => x.price === price`
* 由 `prop('title')` 產生 `x => x.title`

如此 callback 也 point-free 了，不只更精簡，且可讀性更高。

![ramda003](/images/ramda/hello-world/ramda003.png)

## User Function

```javascript
import { pipe, filter, map, propEq, prop, concat, toUpper, slice, nth, converge } from 'ramda';

let data = [
  { title: 'fp in JavaScript', price: 100 },
  { title: 'rxJS in Action', price: 200 },
  { title: 'speaking JavaScript', price: 300 }
];

// capitalize :: String -> String
let capitalize = x => x[0].toUpperCase() + x.slice(1);

// fn :: Number -> [a] -> [b]
let fn = price => pipe(
  filter(propEq('price', price)),
  map(prop('title')),
  map(capitalize)
);

console.log(fn(300)(data));
```

眼尖的讀者會發現結果的 `functional Programming in JavaScript` 的 `f` 為小寫，原始的資料就是如此，但 `F` 為大寫較符合英文閱讀習慣。

因此我們可以自行寫一個 `capitalize()`，將第一個字母變成大寫。

在 Ramda 要成為能夠 `pipe()` 的條件很簡單，只要 function 是單一 argument，且是 pure function 即可。

![ramda004](/images/ramda/hello-world/ramda004.png)

## Conclusion

* Ramda 提供了 FP 該有的 function，不再侷限於 `Array.prototype` 有限的 function
* Ramda 可以很容易的擴充 function，不再擔心污染 `Array.prototype` 
* Ramda 使用 `pipe()`，觀念上更接近 FP 的 Function Composition
* Point-free 也是 Ramda 一大特色，讓程式碼更精簡，可讀性更高

## Reference

[Ramda](https://ramdajs.com), [filter()](https://ramdajs.com/docs/#filter)
[Ramda](https://ramdajs.com), [map()](https://ramdajs.com/docs/#map)
[Ramda](https://ramdajs.com), [pipe()](https://ramdajs.com/docs/#pipe)
[Ramda](https://ramdajs.com), [prop()](https://ramdajs.com/docs/#prop)
[Ramda](https://ramdajs.com), [propEq()](https://ramdajs.com/docs/#propEq)