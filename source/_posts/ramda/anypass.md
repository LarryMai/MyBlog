title: 使用 anyPass() 產生符合部分條件的 Predicate
tags:
  - Ramda
  - Ramda/anyPass
  - Ramda/any
  - Ramda/propEq
feature: images/feature/ramda.png
date: 2019-05-15 12:23:43
---
Ramda 有 `any()` 並不令人訝異，ECMAScript 也有 `some()`，但 Ramda 又另外提供了 `anyPass()`，這與 `any()` 有什麼不同呢 ?

<!-- more -->

## Version

VS Code 1.33.0
Quokka 1.0.205
Ramda 0.26.1

## anyPass()

```javascript
import { anyPass, propEq } from 'ramda';

let data = { title: 'FP in JavaScript', price: 100 };

// fn :: Object -> Boolean
let fn = anyPass([
  propEq('price', 100),
  propEq('price', 200)
]);

console.log(fn(data));
```

若只要 `price` 為 `100` 或 `200`，我們都認為是合理價錢。

以 FP 角度，我們必須提供兩個 predicate，但只要其中一個 predicate 成立即可，因此使用 Ramda 的 `anyPass()` 將兩個使用 `propEq()` 產生的 predicate 組合成單一 predicate。

注意新 predicate 接受的 data 與原本 predicate 都相同，一樣是 object。

> **anyPass()**
> `[(*… → Boolean)] → (*… → Boolean)`
> 將眾多 predicate 組合成單一 predicate，只要任何 predicate 成立即可

`[(*… → Boolean)]`：眾多 predicate 組合在 array 中

`(*… → Boolean)`：組合成單一 predicate

![anypass000](/images/ramda/anypass/anypass000.png)

## Array

```javascript
import { anyPass, propEq, any } from 'ramda';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 }
];

// fn :: [Object] -> Boolean
let fn = any(anyPass([
  propEq('price', 100),
  propEq('price', 200)
]));

console.log(fn(data));
```
> Q：`any()` 與 `anyPass()` 的差別在哪 ?

`any()` 接受的是單一 predicate，data 為 array，結果為 boolean。

`anyPass()`  能接受的是多個 predicate，data 為 object，結果還是 predicate。

因此若要 array 接受多個 predicate，就必須組合 `any()` 與 `anyPass()`。

![anypass001](/images/ramda/anypass/anypass001.png)

## Conclusion

* `any()` 結果是 boolean；而 `anyPass()` 結果是 predicate
* `anyPass()` 是 `any()` 的昇華：`any()` 對象是 array，只能使用單一 predicate；而 `anyPass()` 對象是 object，能使用多個 predicate
* 若要 array 適用於多個 predicate，就必須組合 `any()` 與 `anyPass()`

## Reference

[Ramda](https://ramdajs.com), [any()](https://ramdajs.com/docs/#any)
[Ramda](https://ramdajs.com), [anyPass()](https://ramdajs.com/docs/#anyPass)
[Ramda](https://ramdajs.com), [propEq()](https://ramdajs.com/docs/#propEq)