title: Ramda 之 none()
tags:
  - Ramda
  - Ramda/none
feature: images/feature/ramda.png
date: 2019-05-15 14:24:43
---
實務上我們常需判斷 Array 是否 `全部不` 符合某條件，若都不存在則傳回 `true`，否則傳回 `false`。

<!-- more -->

## Version

VS Code 1.33.0
Quokka 1.0.205
Ramda 0.26.1

## Imperative

```javascript
let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 }
];

// none :: (a -> Boolean) -> [a] -> Boolean
let none = pred => arr => {
  for (let elem of arr) {
    if (pred(elem)) return false;
  }
  return true;
};

// fn :: [a] -> Boolean
let fn = none(x => x.price === 400);

console.log(fn(data));
```

建立 `none()`，Imperative 會使用 `for` loop 搭配 `if` 判斷，若符合條件就直接回傳 `false`，若都不符合條件則回傳 `true`。

![none000](/images/ramda/none/none000.png)

## any()

```javascript
import { compose, any, complement } from 'ramda';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 }
];

// none :: (a → Boolean) → [a] → Boolean
let none = compose(complement, any);

// fn :: [a] -> Boolean
let fn = none(x => x.price === 400);

console.log(fn(data));
```

其實仔細想一想，`none()` 不就是 not `any()` 嗎 ? 因此我們也可以將 `complement()` 與 `any()` 組合產生 `none()`。

> **complement()**
> `(*… → *) → (*… → Boolean)`
> 將原本回傳 `true` 的 function 變成傳回 `false` 的 function，反之亦然

![none001](/images/ramda/none/none001.png)

## none()

```javascript
import { none } from 'ramda';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 }
];

// fn :: [a] -> Boolean
let fn = none(x => x.price === 400);

console.log(fn(data));
```

其實 Ramda 已經內建 `none()`，可直接使用。

> **none()**
> `(a -> Boolean) -> [a] -> Boolean`
>
> 判斷符合條件的值是否 `不` 存在於 array 

`(a -> Boolean)`： 判斷條件

`[a]`：data 為 array

`Boolean`：回傳比較結果

![none003](/images/ramda/none/none003.png)

## Point-free

```javascript
import { none, propSatisfies, gte, __ } from 'ramda';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 }
];

console.log(none(propSatisfies(gte(__, 400), 'price'))(data));
```

`none()` 的 callback 也可改用 `propSatisfies()` 產生，語意更佳。


![none002](/images/ramda/none/none002.png)

## Conclusion

* ECMAScript 並沒有內建 `none()`
* `none()` 也可以使用 `any()` 組合出來，再次見證 Function Composition 的威力
* `none()` 的 callback 也可用 Ramda 的 function 產生，point-free 更精簡，且可讀性更高

## Reference

[Ramda](https://ramdajs.com), [none()](https://ramdajs.com/docs/#none)
[Ramda](https://ramdajs.com), [any()](https://ramdajs.com/docs/#any)
[Ramda](https://ramdajs.com), [complement()](https://ramdajs.com/docs/#complement)
[Ramda](https://ramdajs.com), [propSatisfies()](https://ramdajs.com/docs/#propSatisfies)
[Ramda](https://ramdajs.com), [lte()](https://ramdajs.com/docs/#lte)
[Ramda](https://ramdajs.com), [__()](https://ramdajs.com/docs/#__)
