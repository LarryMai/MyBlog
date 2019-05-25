title: 如何讀取 Cookie ?
tags:
  - Ramda
  - Crocks
  - Maybe
  - Ramda/pipe
  - Ramda/compose
  - Ramda/split
  - Ramda/converge
  - Ramda/nth
  - Ramda/objOf
  - Ramda/map
  - Ramda/find
  - Ramda/prop
  - Ramda/isNil
  - Ramda/complement
  - Crocks/prop
  - Maybe/option
feature: images/feature/ramda.png
date: 2019-05-25 15:05:26
---
當我們使用 `document.cookie()` 讀取 Cookie 時，回傳為 String，我們希望提供 Key 讀取其 Value，這常見的需求該如何實現呢 ? 本文分別使用 Imperative、Functional 與 Maybe 三種方式實現。

<!-- more -->

## Version

VS Code 1.34.0
Quokka 1.0.216
Ramda 0.26.1
Croks 0.11.1

## Cookie

```javascript
let readCookie = () => {
  let data = document.cookie.split('; ').slice(0, -2);
  console.log(getCookie('firstName')(data));
};
```

實務上要使用 `document.cookie` 去讀取 cookie，但讀出來為以下格式：

```shell
firstName=Sam; lastName=Xiao; _ga=GA1.1.320322304.1546312556; Hm_lvt_47acec2d282c3986f1b600abdc11c7ab=155525178
```

倒數第二個 `;` 之後的資訊並不是我們要的，所以先 `split()` 成 array 後，用 `slice()` 取得不含倒數兩個 element 的新 array，最後再傳入 `getCookie()` ，根據 key 取得 value。

## Imperative

```javascript
let data = [
  'firstName=Sam',
  'lastName=Xiao'
];

// getCookie :: String -> [String] ->  String
let getCookie = key => arr => {
  for(let i = 0; i < arr.length; i++) {
    let tuple = arr[i].split('=');
    if (tuple[0] === key) return tuple[1];
  }

  return '';
};

console.log(getCookie('firstName')(data));
console.log(getCookie('lastName')(data));
console.log(getCookie('age')(data));
```

`data` 為根據 `document.cookie()` 所整理過的 string。

我們希望當傳入 key 時， `getCookie()` 回傳 value。

第 8 行

```javascript
for(let i = 0; i < arr.length; i++) {
```

Imperative 會使用 `for` loop 一個一個找。

第 9 行

```javascript
let tuple = s.split('=');
```

Array 中的 string 形式為 `firstName=Sam`，可再使用 `split()` 拆成 array，因為很類似 tuple，變數姑且命名為 `tuple`。

> `tuple` 當然是很糟糕的命名，應該取一個有意義的變數名稱

第 10 行

```javascript
if (tuple[0] === key) return tuple[1];
```

`tuple[0]` 即為 key，而 `tuple[1]` 為 value，因此可直接使用 `if` 判斷。

> Imperative 會充斥著 `中繼變數`，然後就開始為 `變數命名` 傷腦筋，要如何使變數可讀性高又有意義，但畢竟我們在乎的是結果，這些中繼變數真的需要嗎 ? 是值得深思的問題

13 行

```javascript
return '';
```

若傳入的 key 不存在，則回傳 empty string，避免產生 `undefined`。

![cookie000](/images/ramda/getcookie/cookie000.png)

## Functional

```javascript
import { pipe, compose, split, converge, nth, objOf, map, find, propOr, prop, isNil, complement } from 'ramda';

let data = [
  'firstName=Sam',
  'lastName=Xiao'
];

// strToObj :: String -> Object
let strToObj = pipe(
  split('='),
  converge(objOf, [nth(0), nth(1)])
);

// isNotNil :: * -> Boolean
let isNotNil = complement(isNil);

// getCookie :: String -> [String] -> String
let getCookie = key => pipe(
  map(strToObj),
  find(compose(isNotNil, prop(key))),
  propOr('', key)
);

console.log(getCookie('firstName')(data));
console.log(getCookie('lastName')(data));
console.log(getCookie('age')(data));
```

17 行

```javascript
// getCookie :: String -> [String] -> String
let getCookie = key => pipe(
  map(strToObj),
  find(compose(isNotNil, prop(key))),
  propOr('', key)
);
```

FP 不會使用 `for` loop 處理，可由 `pipe()` 清楚看出演算法流程：

1. 先使用 `map()` 將 array 內的 string 轉成 object
2. 再使用 `find()` 搜尋 array 中每個 object，找到就傳回 object，否則傳回 `undefined`
3. 最後使用 `propOr()` 根據 key 取得 value

其中將 string 轉成 object，是想借助 Ramda 對 object 支援豐富 function。

> 可以看出 FP 解決問題方式是將問題最小化切割，然後各個擊破，與 Imperative整體思考方式不同

第 8 行

```javascript
// strToObj :: String -> Object
let strToObj = pipe(
  split('='),
  converge(objOf, [nth(0), nth(1)])
);
```

1. 先使用 `split()` 將字串轉為 array

2. 再使用 `objOf()` 將 array 轉成 object

20 行

```javascript
find(compose(isNotNil, prop(key)))
```

使用 `find()` 根據 key 找尋 value，其 predicate 為 `(a -> Boolean)`，而 `prop()` 回傳為 `a | Undefined`，因此組合 `isNotNil()` 使其轉成 boolean。

14 行

```javascript
// isNotNil :: * -> Boolean
let isNotNil = complement(isNil);
```

Ramda 並沒有提供 `isNotNil()`，須自行組合，也可使用 `compose(not, isNil)`。

21 行

```javascript
propOr('', key)
```

由於 `find()` 可能回傳 `undefined`，所以特別使用了 `propOr()` 處理。

> 我們可發現 FP 完全 `沒有` 中繼變數，所以再也不必為了變數命名而傷透腦筋

![cookie001](/images/ramda/getcookie/cookie001.png)


## Maybe

```javascript
import { pipe, compose, split, converge, nth, objOf, map, find, prop, isNil, complement } from 'ramda';
import { prop as prop_ } from 'crocks';

let data = [
  'firstName=Sam',
  'lastName=Xiao'
];

// strToObj :: String -> Object
let strToObj = pipe(
  split('='),
  converge(objOf, [nth(0), nth(1)])
);

// isNotNil :: * -> Boolean
let isNotNil = complement(isNil);

// getCookie :: String -> [String] -> Maybe String
let getCookie = key => pipe(
  map(strToObj),
  find(compose(isNotNil, prop(key))),
  prop_(key),
);

console.log(getCookie('firstName')(data).option('N/A'));
console.log(getCookie('lastName')(data).option('N/A'));
console.log(getCookie('age')(data).option('N/A'));
```

Ramda 的 `find()` 唯一缺點就是回傳 `undefined`，因此我們必須小心翼翼地使用 `propOr()` 處理，但這有幾個缺點：

* `undefined` 並非邏輯的一部分，是為了 `find()` 而處理
* 若一不小心使用了 `prop()`，就可能回傳 `undefined`
* 目前我們使用 `propOr('', key)`，也就是 `undefined` 時回傳 empty string，但若使用端想自行決定 `undefined` 的 string 呢 ?

比較好的方式是回傳 `Maybe`，由使用端決定 `undefined` 該如何處理。

21 行

```javascript
find(compose(isNotNil, prop(key))),
prop_(key),
```

由於要回傳 `Maybe`，改用 Crocks 的 `prop()`，而不是 Ramd 的 `prop()`，因為 Crocks 很多 function 名稱與 Ramda 一樣，差異只在於回傳 `Maybe`，因此 Crocks 所提供的同名 function 一律以 `_` postfix 表示。

26 行

```javascript
console.log(getCookie('firstName')(data).option('N/A'));
console.log(getCookie('lastName')(data).option('N/A'));
console.log(getCookie('age')(data).option('N/A'));
```

由於 `getCookie()` 回傳 `Maybe`，必須透過 `option()` 將 `Maybe` 轉回 string。

若 `undefined` 時想顯示 `N/A`，可一併傳進 `option()`。

> 使用 `Maybe` 後，`undefined` 改由使用端處理，`getCookie()` 可專心處理正常邏輯，不必再為了 `undefined` 分心，程式碼邏輯也更清楚

![cookie002](/images/ramda/getcookie/cookie002.png)

## Conclusion

* Imperative 會使用很多中繼變數，常需為了變數命名傷透腦筋，也必須小心處理 `undefined`
* Functional 則不必使用中繼變數，可由 `pipe()` 清楚看出演算法思路，藉由將問題最小化分割，然後各個擊破
* Ramda 有些 function 會回傳 `undefined`，如 `find()`，可藉由 `Maybe` 讓使用端處理，讓主邏輯更為清楚，不用再為 `undefined` 分心
* Crocks 不少 function 與 Ramda 同名，差異在於回傳 algebraic data type，一律以 `_` postfix 表示，類似 Haskell 以 `fn' ` 的 apostrophe 命名方式

## Reference

[W3schools.com](https://www.w3schools.com), [JavaScript Cookie](https://www.w3schools.com/js/js_cookies.asp)
[Ramda](https://ramdajs.com), [pipe()](https://ramdajs.com/docs/#pipe)
[Ramda](https://ramdajs.com), [compose()](https://ramdajs.com/docs/#compose)
[Ramda](https://ramdajs.com), [split()](https://ramdajs.com/docs/#split)
[Ramda](https://ramdajs.com), [converge()](https://ramdajs.com/docs/#converge)
[Ramda](https://ramdajs.com), [nth()](https://ramdajs.com/docs/#nth)
[Ramda](https://ramdajs.com), [objOf()](https://ramdajs.com/docs/#objOf)
[Ramda](https://ramdajs.com), [map()](https://ramdajs.com/docs/#map)
[Ramda](https://ramdajs.com), [find()](https://ramdajs.com/docs/#find)
[Ramda](https://ramdajs.com), [prop()](https://ramdajs.com/docs/#prop)
[Ramda](https://ramdajs.com), [isNil()](https://ramdajs.com/docs/#isNil)
[Ramda](https://ramdajs.com), [complement()](https://ramdajs.com/docs/#complement)
[Crocks](https://evilsoft.github.io/crocks/), [Maybe](https://evilsoft.github.io/crocks/docs/crocks/Maybe.html)
[Crocks](https://evilsoft.github.io/crocks/), [prop()](https://evilsoft.github.io/crocks/docs/crocks/Maybe.html#prop)
[Crocks](https://evilsoft.github.io/crocks/), [option()](https://evilsoft.github.io/crocks/docs/crocks/Maybe.html#option)