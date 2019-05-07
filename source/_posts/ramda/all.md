title: Ramda 之 all()
tags:
  - Ramda
  - Ramda/all
  - Ramda/propSatisfies
feature: images/feature/ramda.png
date: 2019-04-10 15:23:43
---
實務上我們常需判斷 Array 是否全部符合某條件，若存在則傳回 `true`，若不存在則傳回 `false`。

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

let all = cb => data => {
  for (let item of data) {
    if (!cb(item)) return false;
  }
  return true;
};

console.log(all(x => x.price >= 300)(data));
console.log(all(x => x.price >= 100)(data));
```

建立 `all()`，imperative 會使用 `for` loop 搭配 `if` 判斷，若不符合條件就直接回傳  `false` 結束，若都符合條件則回傳 `true`。

![all000](/images/ramda/all/all000.png)

## Array.prototype.every()

```javascript
let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 }
];

console.log(data.every(x => x.price >= 300));
console.log(data.every(x => x.price >= 100));
```

`Array.prototype` 有內建 `every()`，可直接傳入 arrow function 即可。

> `every()` 就功能上相當於 `all()`，但 `every()` 屬於 OOP 風格，為 data 的 method，而 `all()` 為 FP 風格，data 以參數傳入 function，且為最後一個參數，方便 point-free

![all001](/images/ramda/all/all001.png)

## all()

```javascript
import { all, propSatisfies, gte, __ } from 'ramda';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 }
];

console.log(all(propSatisfies(gte(__, 300), 'price'))(data));
console.log(all(propSatisfies(gte(__, 100), 'price'))(data));
```

事實上 Ramda 已經內建 `all()` ，且 callback 也可改用 `propSatisfies()` 與 `gte()` 產生，語意更佳。

> **all()**
> `(a → Boolean) → [a] → Boolean`
> 判斷 array 是否全部符合某條件

`(a -> Boolean)`： 判斷條件

`[a]`：data 為 array

`Boolean`：回傳比較結果

![all002](/images/ramda/all/all002.png)

## Conclusion

- `all()` 相當於內建的 `every()`，只是 `every()` 為 OOP 風格，而 `all()` 為 FP 風格
- `any()` 的 callback 也可用 Ramda 的 function 產生，point-free 更精簡，且可讀性更高
- Object 相等條件 predicate 可用 `propEq()` 產生，其他條件則要使用 `propSatisfies()` 產生

## Reference

[Ramda](https://ramdajs.com), [all()](https://ramdajs.com/docs/#all)
[Ramda](https://ramdajs.com), [propSatisfies()](https://ramdajs.com/docs/#propSatisfies)