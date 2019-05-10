title: Ramda 之 apply()
tags:
  - Ramda
  - Ramda/apply
feature: images/feature/ramda.png
date: 2019-05-07 15:23:43
---
將原本多 Argument Function，透過 Ramda 的 `apply()` 成為單一 Argument Function。

<!-- more -->

## Version

VS Code 1.33.1
Quokka 1.0.211
Ramda 0.26.1
ECMAScript 5
ECMAScript 2015

## Function

```javascript
let add = (x, y, z) => x + y + z;

console.log(add(1, 2, 3));
```

`add()` 為普通 function，提供了 3 個 argument。

![apply000](/images/ramda/apply/apply000.png)

## ECMAScript 5

```javascript
let data = [1, 2, 3];
let add = (x, y, z) => x + y + z;

console.log(add.apply(null, data));
```

ES5 的 `Function.prototype.apply()` 允許我們對 function 以 array 提供單一 argument，其中 `apply()` 第一個 argument 可提供 object 取代 function 中的 `this`，因為 `add()` 沒使用 `this`，且我們也沒有要取代 `this`，因此傳入 `null`。

> 換成 array 後，argument 從 3 個變成 1 個 array

![apply002](/images/ramda/apply/apply002.png)

## ECMAScript 2015

```javascript
let data = [1, 2, 3];
let add = (x, y, z) => x + y + z;

console.log(add(...data));
```

ES2015 支援 spread operator，可將單一 array 展開成為多 argument。

> ES2015 的 spread operator 會比 ES5 的 `apply()` 容易理解，且語義更為清楚

![apply005](/images/ramda/apply/apply005.png)

## apply()

```javascript
let data = [1, 2, 3];

let add = (x, y, z) => x + y + z;
let apply = fn => args => fn.apply(null, args);

console.log(apply(add)(data));
```

ES5 與 ES2015 都是以不改變 function signature 前提下，從 data 角度思考，將 array 轉成多 argument。

能否以 function 角度思考，希望提供 `apply()` 直接改變 signature，從多 argument function 轉成單一 argument function 呢 ?

第 2 行

```javascript
let apply = fn => args => fn.apply(null, args);
```

利用 higher order function 技巧，回傳新的 function 為 `args => fn.apply(null, args);`，其背後依舊使用 ES5 的 `apply()` 達成。

第 4 行

```javascript
apply(add)(data)
```

`add()` 先透過 `apply()` 產生新 function，其 signature 已經從原本多 argument 變成單一 argument function，因此可直接傳入單一 array。

![apply003](/images/ramda/apply/apply003.png)

```javascript
import { apply } from 'ramda';

let data = [1, 2, 3];

let add = (x, y, z) => x + y + z;

console.log(apply(add)(data));
```

事實上 Ramda 已經提供了 `apply()`，可直接使用。

> **apply()**
> `(*… → a) → [*] → a`
> 將 argument function 變成以 array 為輸入的單一 argument function

`(*… → a)`：原本多 argument function

`[*] -> a`：回傳以 array 為單一 argument function

![apply001](/images/ramda/apply/apply001.png)

## Conclusion

* ES5 的 `apply()`  與 ES205 的 spread operator，都是以 data 角度思考，在 signature 不變的前提下，將 array 轉成多 argument
* Ramda 是以 function 角度思考，直接將 signature 由多 argument 轉成單一 argument 的 array
* `apply()` 因為有 `a`，所以參數為 array，這樣聯想可以幫助記憶

## Reference

[Ramda](https://ramdajs.com), [apply()](https://ramdajs.com/docs/#apply)
[Samantha Ming](https://medium.com/@samanthaming), [Passing Array as Function Arguments](https://medium.com/dailyjs/passing-arrays-as-function-arguments-c1f3644ecb9c)