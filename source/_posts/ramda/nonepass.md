title: Ramda 自行組合 nonePass()
tags:
  - Ramda
  - Ramda/compose
  - Ramda/complement
  - Ramda/anyPass
  - Ramda/propEq
  - Ramda/propSatisfies
  - Ramda/includes
  - Ramda/all
  - Ramda/nonePass
feature: images/feature/ramda.png
date: 2019-05-15 12:23:43
---
Ramda 有提供 `anyPass()` 與 `allPass()`，卻沒有提供 `nonePass()`，沒關係，藉由 Function Composition，我們也可以自行組合出 `nonePass()`。

<!-- more -->

## Version

VS Code 1.33.0
Quokka 1.0.205
Ramda 0.26.1

## nonePass

```javascript
import { anyPass, propEq, compose, complement, includes, propSatisfies } from 'ramda';

let data = { title: 'Rx in Action', price: 200 };

// nonePass :: [(*… → Boolean)] → (*… → Boolean)
let nonePass = compose(
  complement,
  anyPass
);

// fn :: Object -> Boolean
let fn = nonePass([
  propEq('price', 400),
  propSatisfies(includes('JavaScript'), 'title')
]);

console.log(fn(data));
```

若不存在 `price` 為 `400` ，也不包含 `title` 為 `JavaScript`，則傳回 `true`，否則傳回 `false`。

`none()` 可藉由`complement()` 與 `any()` 組合出來，同理，`nonePass()` 也可藉由 `complement()` 與 `anyPass()` 組合出來

> **nonePass()**
> `[(*… → Boolean)] → (*… → Boolean)`
> 將眾多 predicate 組合成單一 predicate，必須沒有任何 predicate 成立

`[(*… → Boolean)]`：眾多 predicate 組合在 array 中

`(*… → Boolean)`：組合成單一 predicate

![nonepass000](/images/ramda/nonepass/nonepass000.png)

```javascript
import { anyPass, propEq, compose, complement, includes, propSatisfies, all } from 'ramda';

let data = [
  { title: 'Exloring ES6', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Exploring ReasonML', price: 300 }
];

// nonePass :: [(*… → Boolean)] → (*… → Boolean)
let nonePass = compose(
  complement,
  anyPass
);

// fn :: [Object] -> Boolean
let fn = all(nonePass([
  propEq('price', 400),
  propSatisfies(includes('JavaScript'), 'title')
]));

console.log(fn(data));
```

若 array 所有 object 都不符合條件則傳回 `true`，否則傳回 `false`。

使用 `all()` 與 `nonePass()` 組合，則可針對 array 做判斷。

![nonepass001](/images/ramda/nonepass/nonepass001.png)

## Conclusion

* 因為較少使用，Ramda 並有提供 `nonePass()`，但藉由 Function Composition，我們仍可使用 `complement()` 與 `anyPass()` 組合出 `nonePass()`
* 當 `nonePass()` 要配合 array 時比較 tricky，並不是 `none()` 而是 `all()`，因為 `nonePass()` 已經做過一次 `反向邏輯`，再使用 `none()` 會變成 `負負得正` 效果，因此只能用 `all()`

## Reference

[Ramda](https://ramdajs.com), [compose()](https://ramdajs.com/docs/#compose)
[Ramda](https://ramdajs.com), [complement()](https://ramdajs.com/docs/#complement)
[Ramda](https://ramdajs.com), [anyPass()](https://ramdajs.com/docs/#anyPass)
[Ramda](https://ramdajs.com), [propEq()](https://ramdajs.com/docs/#propEq)
[Ramda](https://ramdajs.com), [propSatisfies()](https://ramdajs.com/docs/#propSatisfies)
[Ramda](https://ramdajs.com), [includes()](https://ramdajs.com/docs/#includes)
[Ramda](https://ramdajs.com), [all()](https://ramdajs.com/docs/#all)

