title: Ramda 之 allPass()
tags:
  - Ramda
  - Ramda/allPass
  - Ramda/all
  - Ramda/includes
  - Ramda/lte
  - Ramda/propSatisfies
feature: images/feature/ramda.png
date: 2019-05-15 11:23:43
---
Ramda 有 `all()` 並不令人訝異，ECMAScript 也有 `every()`，但 Ramda 又另外提供了 `allPass()`，這與 `all()` 有什麼不同呢 ?

<!-- more -->

## Version

VS Code 1.33.0
Quokka 1.0.205
Ramda 0.26.1

## allPass()

```javascript
import { allPass, propSatisfies, lte, includes } from 'ramda';

let data = { title: 'FP in JavaScript', price: 100 };

// fn :: Object -> Boolean
let fn = allPass([
  propSatisfies(lte(100), 'price'),
  propSatisfies(includes('JavaScript'), 'title')
]);

console.log(fn(data));
```

若只要 `price` 大於 `100` 且 `title` 有 `JavaScript`，我們都認為是好書。

以 FP 的角度，我們必須提供兩個 predicate，且兩個 predicate 都必須成立，因此使用 Ramda 的 `allPass()` 將兩個使用 `propSatisfies()` 產生的 predicate 組合成單一 predicate。

注意新 predicate 接受的 data 與原本 predicate 都相同，一樣是 object。

> **allPass()**
> `[(*… → Boolean)] → (*… → Boolean)`
> 將眾多 predicate 組合成單一 predicate，必須所有的 predicate 同時都成立

`[(*… → Boolean)]`：眾多 predicate 組合在 array 中

`(*… → Boolean)`：組合成單一 predicate

![allpass000](/images/ramda/allpass/allpass000.png)

```javascript
import { allPass, propSatisfies, lte, all, includes } from 'ramda';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'Speaking JavaScript', price: 300 }
];

// fn :: [Object] -> Boolean
let fn = all(allPass([
  propSatisfies(lte(100), 'price'),
  propSatisfies(includes('JavaScript'), 'title')
]));

console.log(fn(data));
```

> Q：`all()` 與 `allPass()` 的差別在哪 ?

`all()` 接受的是單一 predicate，data 為 array，結果為 boolean。

`allPass()` 接受的是多個 predicate，data 為 object，結果還是 predicate。

因此若要 array 能接受多個 predicate，就必須組合 `all()` 與 `allPass()`。

![allpass001](/images/ramda/allpass/allpass001.png)

## Conclusion

* `all()` 的結果是 boolean；而 `allPass()` 的結果是 predicate
* `allPass()` 是 `all()` 的昇華：`all()` 的對象是 array，只能使用單一 predicate；而 `allPass()` 的對象是 object，能使用多個 predicate
* 若要 array 適用於多個 predicate，就必須組合 `all()` 與 `allPass()`

## Reference

[Ramda](https://ramdajs.com), [allPass()](https://ramdajs.com/docs/#allPass)
[Ramda](https://ramdajs.com), [all()](https://ramdajs.com/docs/#all)
[Ramda](https://ramdajs.com), [includes()](https://ramdajs.com/docs/#includes)
[Ramda](https://ramdajs.com), [lte()](https://ramdajs.com/docs/#lte)
[Ramda](https://ramdajs.com), [propSatisfies()](https://ramdajs.com/docs/#propSatisfies)

