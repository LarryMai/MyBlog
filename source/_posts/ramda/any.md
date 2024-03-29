title: 使用 any() 找出 Array 中符合部分條件的 Element
tags:
  - Ramda
  - Ramda/equals
  - Ramda/any
feature: images/feature/ramda.png
date: 2019-05-15 14:23:43
---
實務上我們常需判斷 Array 是否 `部分` 符合某條件，若存在則傳回 `true`，若不存在則傳回 `false`。

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

// any :: (a -> Boolean) -> [a] -> Boolean
let any = pred => arr => {
  for (let elem of arr) {
    if (pred(elem)) return true;
  }
  return false;
};

// fn :: [a] -> Boolean
let fn = any(x => x.price === 100);

console.log(fn(data));
```

建立 `any()`，imperative 會使用 `for` loop 搭配 `if` 判斷，若找到就直接回傳  `true` 結束，若都找不到則回傳 `false`。

![any000](/images/ramda/any/any000.png)

## Array.prototype.some()

```javascript
let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 }
];

console.log(data.some(x => x.price === 100));
```

`Array.prototype` 有內建 `some()`，直接傳入 arrow function 即可。

> `some()` 就功能上相當於 `any()`，但 `some()` 屬 OOP 風格，為 data 的 method，而 `any()` 為 FP 風格，data 以 argument 傳入 function，且為最後一個 argument，方便 point-free

![any001](/images/ramda/any/any001.png)

## any()

```javascript
import { any, propEq } from 'ramda';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 }
];

// fn :: [a] -> Boolean
let fn = any(x => x.price === 100);

console.log(fn(data));
```

其實 Ramda 已經內建 `any()`，可直接使用。

> **any()**
> `(a -> Boolean) -> [a] -> Boolean`
>
> 判斷符合條件的 data 是否存在於 array 

`(a -> Boolean)`： 判斷條件 predicate

`[a]`：data 為 array

`Boolean`：回傳比較結果

![any002](/images/ramda/any/any002.png)

## Point-free

```javascript
import { any, propEq } from 'ramda';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 }
];

// fn :: [a] -> Boolean
let fn = any(propEq('price', 100));

console.log(fn(data));
```

`any()` 的 callback 也可改用 `propEq()` 產生，語意更佳。


![any004](/images/ramda/any/any004.png)

## Conclusion

* `any()` 相當於內建的 `some()`，只是 `some()` 為 OOP 風格，而 `any()` 為 FP 風格
* `any()` 的 callback 也可用 Ramda 的 function 產生，point-free 更精簡，且可讀性更高

## Reference

[Ramda](https://ramdajs.com), [any()](https://ramdajs.com/docs/#any)
[Ramda](https://ramdajs.com), [propEq()](https://ramdajs.com/docs/#propEq)