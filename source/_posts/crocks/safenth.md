title: 使用 Maybe 自行組合 safeNth()
tags:
  - Ramda
  - Ramda/nth
  - Crocks
  - Maybe
feature: images/feature/crocks.png
date: 2019-05-18 21:13:08
---
Ramda 有些 Function 會回傳 `undefined`，使用端常常會忘記處理 `undefined` 而產生 Bug，比較好的方式是改回傳 `Maybe`，強迫使用端處理 `undefined`。

<!-- more -->

## Version

VS Code 1.34.0
Quokka 1.0.213
Ramda 0.26.1
Crocks 0.11.1

## nth()

```javascript
import { nth } from 'ramda';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 }
];

console.log(nth(1)(data).title);
console.log(nth(3)(data).title);
```

使用 Ramda 的 `nth()`，取出資料後為 object，取其 `title` property 顯示。

> **nth()**
> `Number -> [a] -> a | Undefined`
> 回傳 array 的 nth element

`Number`：`n`th element

`[a]`：data 為 array

`a | Undefined`：若找得到則傳回 element，找不到則傳回 `undefined`

![nth000](/images/crocks/safenth/nth000.png)

但別忘了 `nth()` 可能回傳 `undefined`，因此很容易產生 `Cannot read property of undefined` 的 run-time 錯誤。

> 當然可以搭配 Ramda 的 `propOr()` 避免，但只要使用端稍微不小心，還是會忘記使用 `propOr()`

## safeNth()

```javascript
import { nth } from 'ramda';
import { isObject, safe } from 'crocks';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 }
];

// safeNth :: Number -> [a] -> Maybe a
let safeNth = ofst => arr => safe(isObject)(nth(ofst, arr));

console.log(safeNth(1)(data).option({ title: 'No title' }).title);
console.log(safeNth(3)(data).option({ title: 'No title' }).title);
```

第 2 行

```javascript
import { isObject, safe } from 'crocks';
```

使用 Crocks 的 `isObject()` 與 `safe()`。

第 10 行

```javascript
// safeNth :: Number -> [a] -> Maybe a
let safeNth = ofst => arr => safe(isObject)(nth(ofst, arr));
```

自行建立 `saveNth()`，signature 與 Ramda 的 `nth()` 完全相同，差異只在於回傳 `Maybe`，而不是 `a | Undefiend`。

> **safeNth()**
> `Number -> [a] -> Maybe a`
> 回傳 array 的 nth element

`Number`：`n`th element

`[a]`：data 為 array

`Maybe a`：無論找不找得到都回傳 `Maybe`

```javascript
safe(isObject)
```

由 Crocks 的 `safe()` 與 `isObject()` 組合出 function，若 `nth()` 結果為 object，則以 `Just` 包進 `Maybe`，否則以 `Nothing` 包進 `Maybe`，最後 `safeNth()` 回傳 `Maybe`。

13 行

```javascript
console.log(safeNth(1)(data).option({ title: 'No title' }).title);
```

因為 `safeNth()` 回傳為 `Maybe`，所以取出資料時一定要透過 `option()` 才能取回 object，因此必須提供 default value 處理 `Nothing`。

> `Maybe` 強迫你要使用 `option()` 才能取回 object，因此使用端不可能忘記處理 `Nothing`；不像 `undefined` 並沒有強迫處理，這就是常見 bug 來源

![nth001](/images/crocks/safenth/nth001.png)

## Conclusion

* 可自行建立 `safeNth()` 取代 Ramda 的 `nth()`，藉由 `Maybe` 保護，就不會再出現  `Cannot read property of undefined` 的 run-time 錯誤
* 實務上應使用 `Maybe` 取代 `undefined`，藉由 function 回傳 `Maybe`，強迫使用端透過 `option()` 處理 `undefined`

## Reference

[Ramda](https://ramdajs.com), [nth()](https://ramdajs.com/docs/#nth)
[Crocks](https://evilsoft.github.io/crocks/), [Maybe](https://evilsoft.github.io/crocks/docs/crocks/Maybe.html)